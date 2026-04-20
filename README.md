# Repositorio de presentaciones generadas por IA

Este repositorio organiza presentaciones web autocontenidas generadas con asistencia de IA, manteniendo un marco común de diseño, narrativa ejecutiva y documentación de decisiones.

## Estructura

```text
presentations/
  datamart-riesgos/
    index.html
    docs/
      CONTEXTO_CODEX.md
      DECISIONES.md
shared/
  DESIGN_SYSTEM.md
templates/
  PRESENTATION_BRIEF.md
  DECISION_LOG.md
  PRESENTATION_README.md
docs/
  repo/
    AI_PRESENTATION_WORKFLOW.md
```

## Principios del repo

- Cada presentación vive en su propia carpeta dentro de `presentations/`.
- Cada presentación debe poder abrirse localmente desde su propio `index.html`.
- El deck final debe ser autocontenido: HTML, CSS y JS inline, sin CDN ni frameworks.
- La presentación inicial `datamart-riesgos` queda como referencia de calidad y flujo de trabajo.
- Las decisiones narrativas y visuales se documentan por presentación.
- El marco compartido vive en `shared/` y `templates/`, no mezclado dentro de una presentación puntual.

## Cómo crear una nueva presentación

1. Crear una carpeta en `presentations/<slug-presentacion>/`.
2. Copiar `templates/PRESENTATION_README.md` como `presentations/<slug-presentacion>/README.md`.
3. Copiar `templates/PRESENTATION_BRIEF.md` como `presentations/<slug-presentacion>/docs/BRIEF.md`.
4. Copiar `templates/DECISION_LOG.md` como `presentations/<slug-presentacion>/docs/DECISIONES.md`.
5. Usar `shared/DESIGN_SYSTEM.md` como marco visual común.
6. Seguir `docs/repo/AI_PRESENTATION_WORKFLOW.md` para trabajar con IA sin perder criterio humano.

## Presentaciones disponibles

- [`datamart-riesgos`](presentations/datamart-riesgos/) — Presentación ejecutiva sobre DATAMART de Riesgos y arquitectura de datos.
- [`agente-analitico-riesgo`](presentations/agente-analitico-riesgo/) — Propuesta técnica para validar un backend agéntico de consulta analítica sobre Control de Riesgo.
