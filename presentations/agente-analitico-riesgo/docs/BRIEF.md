# Brief — Agente Analítico de Control de Riesgo

## Nombre de la presentación

Agente Analítico de Control de Riesgo

## Audiencia

Equipo funcional-técnico de IA de una empresa del sector bancario.

## Objetivo

Validar con el equipo de IA si las herramientas, arquitectura y enfoques propuestos son viables dentro de los estándares corporativos antes de iniciar el desarrollo.

## Contexto

Backend agéntico en Python que recibe peticiones en lenguaje natural sobre métricas de negocio de Control de Riesgo y devuelve respuestas confiables obtenidas desde modelos semánticos existentes en Microsoft Fabric / Power BI.

## Principio rector

El backend es un intérprete y orquestador de consultas, no una fuente de verdad. La verdad vive en catálogos estructurados y modelos semánticos. El LLM razona sobre lenguaje natural, pero no inventa métricas, filtros ni consultas.

## Restricciones

- HTML autocontenido.
- Sin emojis.
- Sin imágenes externas.
- Sin Mermaid.
- SVG inline para diagramas.
- Tono bancario profesional y minimalista.
- No inventar números, porcentajes ni métricas.

## Extensión narrativa (2026-04)

Se incorpora una profundización del diseño de catálogo con tres secciones nuevas ubicadas después de “Catálogos” y antes de “Integración con Fabric”:

1. **Catálogo en dos capas**: separación explícita entre capa estructurada (fuente de verdad) y capa vectorial (asistente semántico).
2. **Ejemplo: consulta de irregularidad en tarjetas**: flujo completo de 6 pasos desde lenguaje natural hasta ejecución DAX y respuesta trazable.
3. **Léxico semántico: puente entre negocio y modelo**: traducción estable de jerga de negocio a reglas técnicas del semantic model.

Objetivo de la extensión: reforzar que el LLM interpreta, pero la verdad técnica y la ejecución determinística viven en el catálogo estructurado y sus reglas.
