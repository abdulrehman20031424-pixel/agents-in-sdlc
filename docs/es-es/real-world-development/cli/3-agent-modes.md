---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear filtros a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos por añadir una pequeña funcionalidad al proyecto. Sin embargo, los cambios mayores requieren un proceso más sólido. Por suerte, GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo de una organización y ayudarnos a crear lo adecuado de la manera correcta. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como esperas, y finalmente la combinarás en el proyecto.

> [!NOTE]
> A medida que avances con esta funcionalidad, usarás la misma conversación y rama. Normalmente trabajarías en ramas o PR diferentes para cada tipo de archivo, pero haremos este atajo para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación nueva de Copilot desde una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la funcionalidad con el modo Autopilot.
- revisarás el código.
- validarás manualmente la funcionalidad en un navegador conectado a un puerto reenviado.

A medida que avances, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks` existente, añadirás validación MCP, crearás un agente de QA y abrirás el PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys crece y las personas visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia pendiente describe la funcionalidad, pero es necesario acordar algunos detalles, como la combinación de categorías, antes de escribir código. Usarás el modo Plan para decidirlos y después autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia sus fundamentos. De hecho, los hace aún más importantes. La mayoría de las personas desarrolladoras siguen un proceso parecido a este:

1. Empiezan con una incidencia registrada que describe el trabajo.
2. Crean un plan de lo que hay que construir.
3. Desarrollan el código y lo revisan.
4. Ejecutan pruebas para validar el código.
5. Validan manualmente la nueva funcionalidad.
6. Crean un pull request (PR).
7. Cuando se ha revisado el código y se ha completado la integración continua, combinan el código.

> [!NOTE]
> Los detalles exactos dependerán del equipo y de la organización, pero la mayoría de los procesos seguirán este patrón.

Si sigues este proceso estándar, te asegurarás de que el código generado por IA cumple los requisitos y pasa por las mismas revisiones que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla la autonomía del agente. Presiona <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar de modo:

- **Interactive**: trabajas con el agente. Te propone cambios y espera tu aprobación antes de continuar.
- **Plan**: el agente crea un plan, pero no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas y repite el proceso hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y después usa Autopilot para implementarlo.

## Empieza por la incidencia

Antes de empezar a trabajar en el filtrado, vuelve a tu codespace y asegúrate de que el repositorio y el terminal están listos.

1. Vuelve a tu codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que el PR de las valoraciones por estrellas se ha combinado.
3. Si el terminal no está abierto, presiona <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Presiona <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Presiona <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Presiona <kbd>C</kbd> para añadir la incidencia al mensaje y volver a la pestaña **Session**.

Observa que el mensaje ahora empieza por `#7` (o por un número parecido). El carácter `#` permite incluir en el contexto una incidencia o un pull request (PR) de GitHub.

## Planifica la funcionalidad de filtrado

La planificación te permite definir cómo se implementará una funcionalidad o se realizará una tarea antes de encargársela a Copilot. Para cualquier trabajo complejo, siempre conviene dedicar un tiempo a planificar. Cambiemos al modo Plan y pidamos a Copilot que cree el plan.

1. Presiona <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del mensaje muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe el siguiente mensaje:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empezará a trabajar en el plan. Primero explorará el proyecto y después determinará el mejor enfoque.

3. Durante el proceso, Copilot quizá te haga preguntas sobre el funcionamiento de los filtros. Respóndelas según tus preferencias. ¡No hay respuestas incorrectas!
4. Cuando termine el plan, presiona <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que modifique las partes que no coincidan con tus decisiones.

## Aprueba Autopilot

Una vez redactado y revisado el plan, es el momento de implementarlo. Dejemos que Copilot trabaje con el modo Autopilot.

Autopilot permite que Copilot repita el proceso hasta que considere que la tarea está terminada.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador debajo del mensaje muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente, así que revísalo antes. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por terminadas.

## Revisa y comprueba la implementación

Hay que revisar el código generado antes de combinarlo, como cualquier otro código. Revisemos el código y ejecutemos el sitio para comprobar que todo funciona.

1. Presiona <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación de los filtros y sus pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste durante la planificación.
4. Cuando hayas revisado el código, presiona <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los errores.

## Explora la nueva funcionalidad

Vale, el código parece correcto, pero ¿se ejecuta? Iniciemos la aplicación como antes y abramos el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y por quien publica, así como las combinaciones que acordaste en el plan.
4. Confirma que el reinicio de los filtros y el comportamiento sin resultados coinciden con la incidencia y tus decisiones.
5. Vuelve a tu codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y próximos pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- iniciaste una conversación nueva de Copilot desde una incidencia de GitHub.
- definiste los requisitos en el modo Plan.
- implementaste la funcionalidad con el modo Autopilot.
- revisaste el código.
- validaste manualmente la funcionalidad en un navegador conectado a un puerto reenviado.

A continuación, profundizaremos en cómo se genera el código y cómo seguir las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
