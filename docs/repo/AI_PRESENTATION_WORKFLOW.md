# Flujo de trabajo para presentaciones generadas por IA

Este flujo captura la forma de trabajo usada en la primera presentación y la generaliza para futuros decks.

## 1. Brief estratégico

Antes de escribir código, definir:

- audiencia;
- objetivo de la exposición;
- contexto de negocio;
- mensaje principal;
- restricciones técnicas;
- tono visual;
- tono verbal;
- criterios de aceptación.

La IA ejecuta mejor cuando el humano dirige con contexto. Sin brief, la IA rellena huecos con promedio estadístico. Y el promedio estadístico rara vez es una buena presentación ejecutiva.

## 2. Narrativa primero, slides después

No empezar por el layout. Primero ordenar el argumento:

```text
¿Qué problema existe?
¿Por qué importa?
¿Qué cambio proponemos?
¿Por qué esa solución es mejor?
¿Cómo se implementa?
¿Qué decisión o mensaje debe quedar al final?
```

Después se decide cuántas slides hacen falta.

## 3. Marco visual común

Usar `shared/DESIGN_SYSTEM.md` como punto de partida:

- paleta común;
- sobriedad ejecutiva;
- jerarquía visual;
- uso moderado de microinteracciones;
- diagramas simples.

El marco da coherencia, pero no debe convertirse en jaula. Cada tema puede requerir su propia metáfora visual.

## 4. Generación incremental

Trabajar por iteraciones:

1. crear estructura base del deck;
2. validar narrativa;
3. refinar copy;
4. refinar layout;
5. agregar interacción solo donde aporte claridad;
6. registrar decisiones.

No conviene pedir “haceme todo perfecto” en un único prompt. Eso es pedirle a un arquitecto que construya una torre sin planos. Locura cósmica.

## 5. Bitácora de decisiones

Cada presentación debe tener un `docs/DECISIONES.md` con decisiones como:

- cambios de narrativa;
- metáforas visuales elegidas;
- interacciones agregadas o descartadas;
- simplificaciones de contenido;
- tradeoffs técnicos.

La bitácora evita que el próximo cambio destruya decisiones que tenían sentido.

## 6. Validación final

Validar cada presentación contra estas preguntas:

- ¿La audiencia entiende el punto sin leer demasiado?
- ¿Cada slide tiene una idea dominante?
- ¿La estética transmite confianza?
- ¿La navegación es simple?
- ¿La interacción aclara o distrae?
- ¿El cierre deja un mensaje memorable?
- ¿El archivo abre localmente sin depender de internet?

## 7. Rol humano vs rol IA

La IA puede proponer, ordenar, escribir, refinar y ejecutar. Pero el criterio lo pone la persona.

El humano define:

- intención;
- audiencia;
- verdad del negocio;
- prioridades;
- qué se sacrifica y qué no.

La IA ejecuta sobre ese marco. Es así de fácil, pero hay que ponerte las pilas con el contexto.
