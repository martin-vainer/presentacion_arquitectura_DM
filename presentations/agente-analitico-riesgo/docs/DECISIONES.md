# Decisiones — Agente Analítico de Control de Riesgo

## 2026-04-20 — Deck con navegación lateral ejecutiva

- **Decisión**: Usar un deck HTML con sidebar fija, progreso superior y navegación por teclado.
- **Por qué**: La audiencia funcional-técnica necesita ubicarse rápido en una presentación larga de 16 secciones.
- **Alternativas consideradas**: Tabs horizontales o slides full-screen sin índice.
- **Tradeoff**: La sidebar ocupa ancho, pero mejora orientación y discusión en reunión.
- **Impacto**: UX, navegación, legibilidad.

## 2026-04-20 — Dirección visual banca técnica editorial alineada a paleta base

- **Decisión**: Aplicar una estética sobria con la paleta base del repo (#005BAA, #00AEEF, #00B08D, #F37053), grises neutros, tarjetas técnicas y diagramas SVG lineales.
- **Por qué**: El contexto bancario requiere confianza, precisión y bajo ruido visual.
- **Alternativas consideradas**: Estética más tecnológica/dark o más comercial.
- **Tradeoff**: Menos impacto visual inicial, más credibilidad técnica.
- **Impacto**: Diseño transversal.

## 2026-04-20 — Catálogos como pieza narrativa central

- **Decisión**: Dar una slide completa al sistema de catálogos como corazón del diseño.
- **Por qué**: El mayor riesgo del proyecto no es el LLM sino la gobernanza de definiciones de negocio.
- **Alternativas consideradas**: Mencionar catálogos solo dentro de arquitectura detallada.
- **Tradeoff**: Ocupa una sección adicional, pero evita que el equipo lea el proyecto como “chatbot sobre datos”.
- **Impacto**: Narrativa técnica y mitigación de riesgos.

## 2026-04-20 — Profundización del catálogo con tres secciones consecutivas

- **Decisión**: Insertar tres secciones nuevas entre “Catálogos” e “Integración con Fabric”: “Catálogo en dos capas”, “Ejemplo: consulta de irregularidad en tarjetas” y “Léxico semántico: el puente entre negocio y modelo”.
- **Por qué**: Se necesitaba explicar con más precisión cómo conviven la capa estructurada (fuente de verdad) y la capa vectorial (asistente de búsqueda), y demostrar el flujo operativo con un caso real.
- **Alternativas consideradas**: Expandir la sección “Catálogos” existente sin agregar nuevas secciones o reemplazarla por una única sección extensa.
- **Tradeoff**: Se incrementó la longitud total del deck (19 slides), pero se redujo ambigüedad técnica en el corazón del sistema.
- **Impacto**: Narrativa técnica, arquitectura de dominio, claridad para audiencia funcional-técnica.

## 2026-04-20 — Regla de no alucinación explicitada como contrato visual

- **Decisión**: Repetir explícitamente la regla “El LLM nunca inventa métricas” en cita destacada y en el cierre del flujo del caso de irregularidad.
- **Por qué**: Es la premisa de confianza más sensible del proyecto frente a una audiencia de riesgo y gobierno.
- **Alternativas consideradas**: Dejar la regla solo implícita en el texto de arquitectura y principios.
- **Tradeoff**: Mayor redundancia deliberada, a cambio de una señal más fuerte de control y determinismo.
- **Impacto**: Mensaje rector de confiabilidad y alineación con estándares de control.

## 2026-04-20 — Conflictos e inconsistencias detectadas

- **Resultado**: No se detectaron conflictos materiales entre la solicitud de ampliación, el design system compartido y las decisiones previas de esta presentación.
- **Nota**: Se mantuvo la sección “Catálogos” como introducción y se extendió con tres secciones nuevas en secuencia, en lugar de duplicar contenido equivalente.

