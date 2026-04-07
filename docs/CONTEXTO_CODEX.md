# Contexto de trabajo para Codex
## Proyecto: Presentación gerencial web — Arquitectura de Datos para Control de Riesgos

### 1. Objetivo del proyecto
Este proyecto consiste en una presentación ejecutiva interactiva desarrollada como una única página web autocontenida (`index.html` con HTML + CSS + JS inline), pensada para ser expuesta en vivo ante el gerente de control de riesgos de un banco.

La presentación debe comunicar de forma clara, visual y convincente una propuesta de reorganización de la arquitectura de datos de la gerencia de control de riesgos.

No es una app de producto ni un sitio tradicional: es una pieza de comunicación ejecutiva, orientada a exposición oral.

---

### 2. Objetivo comunicacional
La presentación debe ayudar al expositor a transmitir:

- por qué el esquema actual genera problemas
- por qué conviene cambiar
- qué propone la nueva arquitectura
- qué beneficios concretos trae
- por qué el modelo dimensional y el modelo semántico se complementan
- cómo podría implementarse por fases

La audiencia no es técnica en sistemas, pero sí técnica en negocio/riesgo.  
Por lo tanto:

- evitar jerga excesiva
- evitar sobrecarga textual
- priorizar mensajes claros y ejecutivos
- usar diagramas que “se expliquen solos”
- cada slide debe tener una idea central muy evidente

---

### 3. Contexto de negocio
#### Situación actual
Actualmente existen múltiples procesos ETL independientes que:
- toman datos de SQL Server y archivos
- generan salidas desnormalizadas
- alimentan reportes de Power BI directamente

Problemas del esquema actual:
- poca gobernanza
- múltiples fuentes de verdad
- baja agilidad para cambios
- pobre performance analítica
- escasa escalabilidad
- difícil mantenibilidad
- lógica de negocio dispersa en múltiples procesos

#### Restricciones del entorno
- entorno on-premise
- seguirá siendo on-premise
- herramientas de explotación: SAS y Power BI

---

### 4. Arquitectura objetivo a comunicar
La arquitectura propuesta es:

Fuentes → Staging → Modelo Relacional Consolidado → Modelo Dimensional → Modelo Semántico → Reportes

#### Significado de cada capa
- **Fuentes**: SQL Server, archivos planos y otras fuentes del banco
- **Staging**: aterrizaje crudo, desacople de fuentes y procesamiento
- **Modelo Relacional Consolidado**: integración y consolidación normalizada como fuente única de verdad
- **Modelo Dimensional**: base analítica reusable con hechos y dimensiones
- **Modelo Semántico**: contrato de consumo estable, gobernado y simplificado
- **Reportes**: Power BI y análisis en SAS consumiendo una capa más controlada

#### Mensaje conceptual clave
- El modelo dimensional y el modelo semántico no compiten
- No son capas redundantes
- Se complementan y resuelven problemas distintos
- La propuesta no agrega complejidad arbitraria: agrega orden, gobernanza, reutilización y escalabilidad

---

### 5. Principios de diseño que deben respetarse
#### Estilo
- minimalista
- serio
- elegante
- corporativo / ejecutivo
- mucho espacio en blanco
- tipografía clara y grande
- animaciones sutiles
- nada estridente ni “demo flashy”

#### Tono visual
Debe transmitir:
- profesionalismo
- confianza
- claridad
- control
- madurez arquitectónica

#### Tono verbal
Debe ser:
- ejecutivo
- concreto
- accesible
- persuasivo sin exagerar
- orientado a beneficios de negocio

---

### 6. Paleta obligatoria
Usar prioritariamente esta paleta:

- `#00AEEF` azul claro
- `#005BAA` azul oscuro
- `#F37053` naranja/coral
- `#6B007B` púrpura
- `#CE819C` rosa
- `#00B08D` verde
- `#E9D661` amarillo
- `#EF413D` rojo

Sugerencias de uso:
- azul oscuro: títulos, autoridad
- azul claro: acento primario
- verde: beneficios / mejora / futuro deseado
- naranja y rojo: dolor, riesgos, situación actual
- púrpura: acento secundario moderado
- fondo: blanco o gris muy claro
- texto: gris oscuro, excelente contraste

---

### 7. Restricciones técnicas obligatorias
- debe existir un solo archivo `index.html`
- todo debe estar inline: HTML + CSS + JS
- sin dependencias externas
- sin CDN
- sin frameworks
- debe abrirse localmente en navegador y funcionar
- navegación por teclado (flechas izquierda/derecha)
- navegación por click o botones
- indicador de progreso visible
- transiciones suaves entre slides
- diagramas en SVG inline o construidos con HTML/CSS
- responsive al menos para laptop/desktop
- código limpio, ordenado y comentado

---

### 8. Estructura narrativa esperada
La estructura base sugerida es:

1. Portada  
2. Situación actual  
3. Por qué cambiar  
4. Arquitectura propuesta  
5. Detalle de capas  
6. Antes vs. después  
7. Beneficios concretos  
8. Roadmap de implementación  
9. Cierre  

Codex puede ajustar la secuencia si mejora el flujo narrativo, pero debe respetar la lógica ejecutiva:
problema → riesgo → propuesta → valor → implementación → cierre.

---

### 9. Estado actual del desarrollo
IMPORTANTE:
Antes de proponer cambios, inspeccioná el proyecto actual y detectá:

- estructura HTML existente
- sistema de navegación entre slides
- componentes ya implementados
- inconsistencias visuales
- problemas de UX
- textos demasiado técnicos o demasiado largos
- problemas de responsividad
- fragmentos duplicados
- CSS difícil de mantener
- JS innecesariamente complejo
- oportunidades de mejorar jerarquía visual

No rehagas todo desde cero salvo que sea estrictamente necesario.  
Preferí evolucionar la versión actual con cambios controlados y justificados.

---

### 10. Forma de trabajar esperada
Al trabajar sobre este proyecto:

1. primero inspeccioná el estado actual del código
2. resumí en pocas líneas qué ya está bien y qué falta
3. proponé un plan breve de mejoras por prioridad
4. ejecutá los cambios de forma incremental
5. mantené coherencia visual y narrativa
6. no agregues complejidad técnica innecesaria
7. cuidá que cada slide tenga un mensaje muy claro
8. evitá “paredes de texto”
9. privilegiá claridad visual sobre decoración
10. si cambiás la narrativa, explicá por qué mejora la exposición

---

### 11. Criterios de calidad para aceptar cambios
Cada cambio debe mejorar al menos una de estas dimensiones:

- claridad ejecutiva
- impacto visual
- legibilidad
- consistencia estética
- calidad de narrativa
- facilidad de exposición en vivo
- mantenibilidad del código
- robustez de navegación
- alineación con el objetivo del proyecto

No introducir cambios “porque sí”.

---

### 12. Cosas a evitar
- diseño recargado
- animaciones exageradas
- estilo startup / marketing / landing page
- colores usados sin criterio
- bloques largos de texto
- tecnicismo innecesario
- explicaciones de implementación demasiado profundas
- widgets innecesarios
- lógica JS compleja para interacciones menores
- íconos o recursos externos
- romper el requisito de archivo único autocontenido

---

### 13. Qué priorizar si hay trade-offs
Orden de prioridad:

1. claridad del mensaje
2. narrativa ejecutiva
3. calidad visual sobria
4. facilidad de exposición oral
5. consistencia entre slides
6. código mantenible
7. microinteracciones

---

### 14. Entregables esperados cuando trabajes
Cuando realices cambios:
- describí brevemente qué ajustaste
- explicá por qué mejora la presentación
- mantené intacto el enfoque ejecutivo
- si modificás textos, hacelos más claros y más breves
- si modificás diagramas, que sean más comprensibles a simple vista
- si modificás navegación, que sea más intuitiva y estable

---

### 15. Tareas típicas que sí podés hacer
- refinar la jerarquía visual de slides
- mejorar diagramas
- simplificar textos
- mejorar copy ejecutivo
- ajustar layout y spacing
- mejorar accesibilidad visual
- ordenar CSS y JS
- corregir bugs de navegación
- mejorar progress indicator
- hacer más convincente la comparación antes/después
- reforzar visualmente la separación entre modelo dimensional y semántico
- mejorar el roadmap para que sea más entendible por management

---

### 16. Tareas que requieren mucho cuidado
- cambiar la estructura narrativa completa
- reducir demasiada información de negocio
- agregar interactividad que distraiga
- introducir un estilo demasiado moderno o informal
- convertir la presentación en una app tipo dashboard

---

### 17. Instrucción operativa permanente
Actuá como un desarrollador front-end senior con criterio de diseño ejecutivo y comunicación gerencial.

Tu objetivo no es solamente “hacer que se vea bien”, sino construir una herramienta visual efectiva para una exposición de negocio sobre arquitectura de datos en banca/riesgo.

Cada decisión debe responder a esta pregunta:
**¿esto ayuda a que un gerente entienda más rápido, confíe más en la propuesta y vea con claridad el valor del cambio?**