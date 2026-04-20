# Agente Analítico de Control de Riesgo
## Documento de diseño y decisiones de implementación

**Versión:** 1.0  
**Fecha:** Abril 2026  
**Estado:** Propuesta para revisión con equipo de IA

---

## 1. Visión del proyecto

Backend agentico que recibe peticiones en lenguaje natural y devuelve métricas de negocio confiables para la toma de decisiones en el área de control de riesgo. Las métricas se obtienen de modelos semánticos existentes en Microsoft Fabric / Power BI.

El sistema debe poder ser consumido por:

- Otras aplicaciones internas del área.
- Otros agentes de IA de la organización.
- Clientes compatibles con MCP (Claude Desktop, Copilot, agentes custom).
- Interfaces de chat o workflow a futuro.

**Principio rector:** el backend es un intérprete y orquestador de consultas, no una fuente de verdad. La verdad vive en los catálogos estructurados y en los modelos semánticos de Fabric. El LLM razona sobre lenguaje natural, pero nunca inventa métricas, filtros ni consultas.

---

## 2. Stack tecnológico decidido

| Componente | Decisión | Justificación |
|---|---|---|
| Lenguaje | Python | Velocidad de desarrollo, ecosistema IA maduro, cercanía a LLMs. |
| Framework de agentes | Microsoft Agent Framework 1.0 | Sucesor oficial de Semantic Kernel + AutoGen. Integración nativa con Azure, soporte empresarial, workflow graphs con control explícito de flujo, MCP y A2A nativos. |
| API HTTP | FastAPI | Estándar Python, tipado con Pydantic, async nativo. |
| Validación de contratos | Pydantic | Schemas estrictos para entrada/salida de cada capa. |
| Proveedor de LLM | Azure OpenAI Service | Cumple estándares de seguridad corporativos, residencia de datos, compromiso de no-training, integración Entra ID. |
| Vector store | Azure AI Search | Integración nativa con Azure OpenAI, soporta búsqueda vectorial + filtros estructurados combinados. |
| Observabilidad | Application Insights / Azure Monitor | Servicio gestionado de Azure, integración nativa con Agent Framework vía OpenTelemetry. |
| Autenticación a Fabric | Service Principal (Entra ID) | Acceso programático estándar a Power BI REST API. |
| Ejecución de DAX | Power BI Execute Queries REST API | Camino oficial y soportado para consultar semantic models desde fuera de Fabric. |
| Autenticación de consumidores | OAuth con Entra ID | Estándar corporativo Microsoft. |
| Control de versiones | Git (Azure DevOps o GitHub Enterprise) | Según lineamiento corporativo. |
| Despliegue | Azure App Service / Container Apps / AKS | Según lineamiento corporativo. |
| Gestión de secretos | Azure Key Vault | Estándar para credenciales, claves API, secretos de service principal. |

### Frameworks descartados y razones

- **Semantic Kernel standalone:** fue absorbido por Agent Framework. Microsoft recomienda oficialmente Agent Framework para proyectos nuevos desde abril 2026.
- **AutoGen standalone:** mismo caso, absorbido por Agent Framework.
- **LangGraph:** excelente framework, pero vive fuera del ecosistema Microsoft. En un banco con stack Azure, perdemos integración nativa con Entra ID, Azure Monitor y soporte contractual de Microsoft.
- **Pydantic AI:** muy bueno para proyectos chicos con tipado estricto, pero le falta madurez empresarial y no tiene la integración con Azure que necesitamos.
- **LangChain:** el propio equipo recomienda LangGraph para agentes. No es la herramienta correcta para workflows con loops explícitos.

---

## 3. Arquitectura lógica

### 3.1 Agentes del backend

**A. Agente de Interpretación Semántica**

- Recibe la petición en lenguaje natural.
- Mapea lenguaje de negocio a conceptos técnicos.
- Conoce sinónimos, vocabulario de negocio, aliases, nombres de métricas, vocabulario de filtros, expresiones de tiempo, dominios y patrones de ambigüedad.
- Produce una intención técnica normalizada.

**B. Agente Orquestador / de Recuperación**

- Recibe la intención técnica normalizada.
- Decide cómo resolverla usando los catálogos.
- Construye un plan de consulta validado.
- Ejecuta la consulta contra los modelos semánticos de Fabric.
- Devuelve el resultado con metadata de trazabilidad y validación.

**C. Capa de Validación y Control**

- Ejecuta loops, chequeos, guardrails, retries, validaciones, chequeos de ambigüedad, autorización y validación del shape del resultado.
- Previene consultas no soportadas o inseguras.
- Garantiza que la respuesta esté grounded en catálogos reales y resultados de consulta válidos.

### 3.2 Loops de control

1. **Loop de interpretación:** entender el request, resolver candidatos de métrica y filtros. Si la confianza es baja, pedir clarificación.
2. **Loop de grounding:** validar que las entidades interpretadas existen en los catálogos. Normalizar a nombres canónicos. Rechazar lo que no esté grounded.
3. **Loop de query planning:** convertir la intención en un plan técnico. Elegir modelo semántico, métrica, filtros y lógica temporal correctos. Validar el plan antes de ejecutar.
4. **Loop de validación de query:** validar el DAX generado contra patrones permitidos. Asegurar que referencia solo medidas/entidades/dimensiones autorizadas. Retries acotados si es inválido.
5. **Loop de ejecución:** ejecutar la consulta, manejar fallos transitorios con retries acotados, capturar latencia y metadata.
6. **Loop de validación de resultado:** validar que el resultado matchea el contrato de la métrica. Manejar edge cases (null, empty, múltiples filas inesperadas).
7. **Loop de respuesta final:** construir respuesta consumer-friendly con payload estructurado y metadata de provenance. Si la confianza no se cumple, no presentar el resultado como verdad final.

---

## 4. Diseño de catálogos (el corazón del sistema)

### 4.1 Por qué catálogos estructurados y no solo RAG

- **Embeddings son fuzzy:** devuelven lo semánticamente parecido, no lo correcto. Para métricas críticas de riesgo, eso no es aceptable.
- **Sin grounding estructurado, el LLM puede inventar campos, DAX o filtros que "suenan parecido".**
- **Debugging imposible** cuando el único filtro es la similitud semántica.

### 4.2 Arquitectura en dos capas

**Capa 1: Catálogo estructurado (fuente de verdad)**

Base de datos o YAML/JSON versionado en Git con esquema Pydantic estricto. Cada métrica tiene:

- `metric_id`, `business_name`, `aliases`
- `definition` (definición de negocio)
- `business_owner`, `technical_owner`
- `semantic_model`, `workspace_id`, `dataset_id`
- `dax_measure` o estrategia de recuperación
- `default_grain`, `supported_filters`, `required_filters`
- `time_intelligence` (MTD, YTD, QoQ, YoY, etc.)
- `data_lineage` (tablas fuente, proceso ETL, frecuencia de refresh)
- `examples` (preguntas válidas en NL)
- `status`, `version`, `last_updated`
- `trust_level`, `certification_flag`

**Capa 2: RAG semántico (asistente de búsqueda)**

Vector store (Azure AI Search) indexando:

- Definiciones de negocio.
- Aliases y sinónimos.
- Ejemplos de preguntas.
- Documentación extendida.

**Regla clave:** los embeddings se generan automáticamente desde el catálogo estructurado, no al revés. El catálogo estructurado sigue siendo la fuente de verdad.

### 4.3 Flujo de resolución de consulta

1. Match directo contra aliases/nombres del catálogo (rápido, determinista).
2. Si no hay match claro, retrieval semántico → top-K candidatos.
3. El LLM decide entre candidatos **ya validados** contra el catálogo. Nunca inventa uno nuevo.
4. Si la confianza es baja, pide clarificación al usuario.

### 4.4 Catálogos adicionales requeridos

- **Catálogo de ubicación:** mapea cada métrica a su origen físico/lógico (semantic model, dataset, workspace, measure).
- **Léxico semántico de negocio:** sinónimos, aliases, abreviaturas, terminología de dominio, frases típicas en español.
- **Catálogo de filtros y entidades:** entidades de negocio soportadas, segmentos, productos, carteras, períodos, dimensiones, combinaciones permitidas y prohibidas.
- **Catálogo de políticas de consulta:** qué se puede consultar, qué requiere clarificación, qué está prohibido, límites de tamaño de resultado, reglas de fallback.

### 4.5 Linaje técnico desde ETLs

Scrapear los procesos ETL permite construir un linaje técnico complementario:

- De dónde viene cada columna fuente.
- Qué transformaciones sufrió el dato.
- Qué proceso ETL la genera.

**Importante:** el linaje técnico es complementario, no reemplazo de la definición de negocio. Las decisiones de negocio (ej: "cliente activo = transacción en últimos 30 días, excluyendo cuentas corporativas") no están en el código ETL; hay que escribirlas a mano.

Uso del linaje:
- Validar consistencia entre definición de negocio y ETL real.
- Detectar cambios silenciosos en el ETL que afectan métricas.
- Generar documentación inicial automatizada (que después se cura manualmente).

---

## 5. Integración con Fabric

### 5.1 Estrategia de consulta

**No usar:** generación libre de DAX por LLM. Es inseguro y alucinable.

**Usar:** templates parametrizados de DAX asociados a cada métrica del catálogo. El LLM elige la métrica y los filtros, no escribe DAX crudo.

Patrones:
- Templates de DAX parametrizados.
- Referencias a medidas aprobadas.
- Construcción canónica de filtros.
- Validación de dimensiones y lógica de fechas antes de ejecutar.

### 5.2 Por qué no Semantic Link / SemPy

Semantic Link (SemPy) es muy potente pero Microsoft lo soporta solo **dentro** de Fabric notebooks. Para un backend externo reusable, la opción correcta es:

- **Power BI Execute Queries REST API** (primera iteración, más simple).
- **XMLA endpoint** (opción alternativa, más potente pero más complejo).

### 5.3 Autenticación

Service Principal registrado en Entra ID con permisos de lectura sobre los workspaces Fabric correspondientes. Credenciales en Key Vault, consumidas vía MSAL.

---

## 6. Exposición del backend: API REST + MCP

### 6.1 Aclaración conceptual sobre MCP

**MCP (Model Context Protocol)** es un protocolo estándar para que clientes de IA se conecten a servidores que exponen herramientas, recursos y prompts. No es una plataforma ni una tecnología específica: es un contrato de comunicación, como HTTP o gRPC.

**Relación con el backend:**

```
┌─────────────────────────────────────────┐
│  Backend Analítico (FastAPI + Agent FW) │
│  - Catálogos                             │
│  - Agentes de interpretación/orquest.    │
│  - Adaptador Fabric                      │
│  - Lógica de negocio                     │
└────────────┬────────────────────────────┘
             │
     ┌───────┴────────┐
     │                │
┌────▼─────┐    ┌────▼──────┐
│ API REST │    │ Servidor  │
│ (HTTP)   │    │ MCP       │
└──────────┘    └───────────┘
     │                │
 Apps web,       Agentes MCP
 servicios,      (Claude, Copilot,
 otros backs     agentes custom)
```

- El backend analítico contiene toda la lógica.
- El servidor MCP es una **fachada delgada** que expone capacidades del backend siguiendo el protocolo MCP.
- La API REST es la interfaz principal; MCP es una capa adicional de transporte.
- **No debe haber lógica de negocio en el servidor MCP.** Solo traduce llamadas MCP a llamadas internas del backend.

### 6.2 Endpoints REST propuestos

- `POST /query` — consulta en lenguaje natural.
- `POST /interpret` — solo interpreta, no ejecuta.
- `GET /metrics` — lista de métricas disponibles.
- `GET /metrics/{metric_id}` — detalle de métrica.
- `GET /catalog/metrics` — catálogo completo.
- `GET /catalog/filters` — filtros disponibles.
- `GET /trace/{request_id}` — trazabilidad de un request.
- `GET /health` — healthcheck.

### 6.3 Capacidades MCP propuestas

- `consultar_metrica` — herramienta principal de query.
- `listar_metricas_disponibles` — descubrimiento de métricas.
- `explicar_metrica` — definición de negocio y metadata.
- `validar_consulta` — dry-run de una consulta sin ejecutarla.

### 6.4 Decisión: MCP se implementa después del MVP

El servidor MCP se agrega como capa adicional una vez que el backend REST esté estable. Agregar MCP al día uno suma complejidad sin haber validado la base.

---

## 7. Roadmap de implementación

### Fase 0 — Fundación
- Estructura del repo.
- Configuración y gestión de dependencias.
- Bootstrap de la aplicación.
- Logging y tracing base.
- Gestión de settings y secretos.

### Fase 1 — Catálogos primero
- Modelo del catálogo de métricas (Pydantic schemas).
- Léxico semántico.
- Catálogo de filtros.
- Seeds iniciales con 5-10 métricas reales.
- Contratos de API base.
- Pipeline de interpretación simple.
- Resolución determinista de métricas.

### Fase 2 — Orquestación con Agent Framework
- Integración con Microsoft Agent Framework.
- Agente Intérprete.
- Agente Orquestador.
- Workflow graph de control.
- Retries acotados.
- Estado intermedio estructurado.

### Fase 3 — Ejecución contra Fabric
- Adaptador de consulta.
- Path de ejecución DAX.
- Autenticación con service principal.
- Normalización de respuestas.
- Validación de resultados.
- Trazabilidad completa.

### Fase 4 — Hardening
- Tests unitarios y de integración.
- Modos de fallo.
- Observabilidad completa en Application Insights.
- Performance.
- Seguridad.
- Extensibilidad.
- Preparación para MCP.

### Fase 5 — Exposición MCP
- Servidor MCP como fachada.
- Registro en clientes MCP corporativos.
- Documentación de capacidades.

---

## 8. Recursos a solicitar a las áreas técnicas

### Accesos y permisos
- Service Principal en Entra ID con permisos de lectura sobre workspaces Fabric correspondientes.
- Permisos para Power BI REST API, especialmente endpoint Execute Queries.
- Licencias apropiadas de Fabric/Power BI según requisitos de Execute Queries API.

### Infraestructura
- Entorno de desarrollo en Azure (App Service / Container Apps / AKS según lineamiento).
- Acceso a Azure OpenAI Service: endpoint, deployment de modelo clase GPT-4 o superior para razonamiento, modelo menor para tareas auxiliares.
- Instancia de Azure AI Search para vector store.
- Azure Key Vault para secretos.
- Repositorio Git corporativo.

### Observabilidad
- Instancia de Application Insights / Azure Monitor con connection string.

### Gobierno y semántica
- Contacto con stewards de datos / dueños funcionales de los modelos semánticos.
- Documentación existente: diccionarios de datos, definiciones DAX, glosarios.
- Políticas vigentes de gobierno de datos.

### Seguridad
- Lineamiento sobre autenticación de consumidores (OAuth Entra ID).
- Revisión de qué datos pueden salir del modelo semántico hacia el LLM.
- Validación del compromiso de no-training de Azure OpenAI.

---

## 9. Principios innegociables

1. **Confianza primero:** nunca inventar métricas, dimensiones, medidas, datasets, tablas, filtros ni definiciones. Si no está grounded, devolver clarificación estructurada o error.

2. **Catalog-driven:** cada métrica se resuelve a través de metadata/catálogos, no solo a través de razonamiento del prompt.

3. **Ejecución determinista:** el lenguaje natural puede ser ambiguo, pero la ejecución tiene que ser validada y acotada. Generación de queries constreñida por catálogos, templates y validación.

4. **Separación de responsabilidades:** interpretación, orquestación, resolución de catálogo, ejecución, validación y formato deben estar en módulos separados.

5. **Auditable y explicable:** cada respuesta debe poder incluir metadata de trazabilidad (métrica interpretada, modelo usado, dataset/workspace IDs, filtros aplicados, granularidad temporal, flags de confianza/ambigüedad, resultado de validación, status de ejecución, correlation id).

6. **Fallo seguro:** si hay ambigüedad o faltan filtros requeridos, pedir clarificación o devolver estado no resuelto estructurado. Nunca adivinar en silencio cuando se comprometería la confianza.

---

## 10. Riesgos identificados

### Técnicos
- Agent Framework 1.0 es muy nuevo (GA abril 2026). Esperar rough edges en documentación y ejemplos los primeros meses.
- Comunidad más chica que LangGraph en este momento (se equilibrará con el tiempo).
- Dependencia de Azure OpenAI: si hay caída del servicio, el backend queda degradado.

### De gobierno
- El mayor riesgo del proyecto no es técnico: es el mantenimiento del catálogo. Si las métricas no se mantienen actualizadas, el sistema se desactualiza en meses y la gente pierde confianza.
- Mitigación: definir claramente al dueño del catálogo y el proceso de actualización desde el día cero. En este caso, ese rol lo asume directamente el owner del proyecto.

### De adopción
- Los consumidores pueden esperar que el sistema "entienda cualquier pregunta". Hay que comunicar claramente que el sistema responde sobre el catálogo disponible y pide clarificación cuando no puede grounded la consulta.

---

## 11. Decisiones abiertas / a discutir con el equipo de IA

- ¿Lineamiento corporativo sobre framework de agentes ya existente?
- ¿Preferencia de despliegue: App Service vs Container Apps vs AKS?
- ¿Vector store: Azure AI Search confirmado o evaluar alternativas corporativas?
- ¿Autenticación de consumidores: OAuth Entra ID vs API keys gestionadas vs otro?
- ¿Hay un catálogo de métricas corporativo preexistente (Purview, otro) con el que haya que integrar o complementar?
- ¿Qué modelos de Azure OpenAI están aprobados para uso corporativo?
- ¿Hay lineamiento sobre datos que pueden enviarse a LLMs externos?
