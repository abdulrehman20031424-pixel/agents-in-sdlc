---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido al agente en la nube de Copilot que se encargue de tres tareas diferentes para mejorar la experiencia de usuario y añadir funcionalidades. Aunque está diseñado para trabajar de forma asíncrona y autónoma, sigue siendo importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas que asignas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este centro puedes ver las tareas de los agentes que tienen pull requests (PR) abiertos. Puedes explorar las operaciones realizadas e incluso dirigir una sesión que esté en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo se ha dado cuenta de que también convendría añadir un modo claro. Como ya se está trabajando en actualizar los estilos del sitio y añadir una función para cambiar de tema, parece lógico incluirlo. Quieres dirigir el trabajo del agente para que añada también el modo claro.

## Revisa las tareas del agente en la nube de Copilot

Veamos el estado de las tareas que has asignado al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
    - actualizar la documentación del código.
    - generar API para modificar juegos.
    - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
    - obtener el código del repositorio.
    - configurar el entorno de trabajo de Copilot.
    - configurar servidores MCP.
    - realizar distintos pasos para completar la tarea.

> [!NOTE]
> Los pasos concretos variarán según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Fíjate también en el panel de pull requests (PR) del lado derecho, donde puedes consultar el PR y los archivos modificados.

## Dirige el trabajo del agente en la nube

Ahora que has visto las tareas en curso, vamos a pedir a Copilot que incluya el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en el panel de agentes](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Lo sabrás cuando empiece a mostrar mensajes como los del ejemplo.
3. En el diálogo **Steer active session while Copilot is working**, escribe este mensaje:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la nube en la página de agentes con el diálogo para dirigir la sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Presiona <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot lo recibe y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo de trabajo cuando termine el paso en el que estaba trabajando al recibir el mensaje.

El proceso tardará varios minutos. Es un buen momento para detenerte y reflexionar sobre lo que has aprendido y explorado hasta ahora.

## Resumen y próximos pasos

En esta lección has explorado la página de agentes de Copilot, el centro para supervisar y dirigir las tareas del agente en la nube de GitHub Copilot. Desde este panel puedes seguir todas las tareas activas y completadas, revisar el trabajo que se está realizando e incluso redirigir una sesión en curso para cambiar su alcance o proporcionar más indicaciones.

Has explorado estos conceptos:

- supervisar las tareas del agente en la nube desde el centro de control y la página de agentes.
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre los pull requests que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Lección siguiente: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
