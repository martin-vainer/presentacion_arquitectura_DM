# Sistema de diseño compartido

Este documento define el marco visual común para las presentaciones del repositorio.

## Intención

Las presentaciones deben sentirse como piezas ejecutivas: claras, sobrias, confiables y preparadas para exposición oral. No son landing pages, dashboards ni demos visuales recargadas.

## Paleta base

| Uso sugerido | Color |
| --- | --- |
| Azul claro / acento primario | `#00AEEF` |
| Azul oscuro / autoridad / títulos | `#005BAA` |
| Naranja coral / dolor / transición | `#F37053` |
| Verde / mejora / futuro deseado | `#00B08D` |
| Púrpura / acento secundario | `#6B007B` |
| Rosa / acento complementario | `#CE819C` |
| Amarillo / énfasis puntual | `#E9D661` |
| Rojo / riesgo / alerta | `#EF413D` |

## Principios visuales

1. Mucho aire antes que mucha decoración.
2. Una idea central por slide.
3. Jerarquía tipográfica clara: título, bajada, contenido, nota.
4. Diagramas que se entiendan a simple vista.
5. Microinteracciones solo cuando aclaran, no cuando distraen.
6. Contraste alto y lectura cómoda en notebook/proyector.
7. Animaciones sutiles, rápidas y con propósito.

## Principios narrativos

La estructura recomendada es:

```text
contexto → problema → riesgo/oportunidad → propuesta → valor → implementación → cierre
```

Cada deck puede adaptar esa secuencia, pero no debería perder la progresión argumental.

## Restricciones técnicas recomendadas

- Un `index.html` autocontenido por presentación.
- CSS y JS inline para máxima portabilidad.
- Sin frameworks.
- Sin CDN.
- Sin dependencias externas obligatorias.
- Navegación por teclado y botones.
- Indicador de progreso visible.
- Responsive al menos para laptop/desktop.

## Qué evitar

- Estética genérica de startup.
- Efectos visuales que compitan con el mensaje.
- Slides con demasiadas ideas al mismo tiempo.
- Jerga técnica sin traducción ejecutiva.
- Copiar una presentación previa sin reinterpretar el tema nuevo.
