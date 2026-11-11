---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan problemas con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que hay que actualizar o solicitudes de funcionalidades que llevan mucho tiempo pendientes porque no había tiempo para implementarlas. El agente en la nube de GitHub Copilot está diseñado para tareas como actualizar código y añadir funcionalidad de forma autónoma. Cuando termina, genera un borrador de PR para que lo revise una persona desarrolladora. Así se pueden delegar tareas tediosas, acelerar el desarrollo y dejar que el equipo se centre en asuntos más importantes.

Con el agente en la nube de Copilot explorarás:

- cómo personalizar el entorno de generación de código.
- cómo garantizar que las operaciones se realizan de forma segura.
- la importancia de definir incidencias con un alcance claro.
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Las personas contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: comprobarás que falta por completo. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar cómo se descubren los juegos. Quiere que la página de detalles de cada juego muestre juegos relacionados —otros títulos de la misma categoría— para que las personas visitantes puedan seguir explorando. No necesita una implementación refinada todavía; quiere algo que pueda usar para probar la experiencia de usuario durante las pruebas de aceptación. Esta tarea bloquea el trabajo, pero ahora mismo hay otras incidencias de mayor prioridad.

Estos son ejemplos de tareas que pueden pasar a segundo plano y que conviene asignar al agente en la nube de Copilot. El agente trabaja en ellas de forma asíncrona mientras la persona desarrolladora se centra en otras tareas; después, puede revisar su trabajo y confirmar que todo está como esperaba.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a una persona desarrolladora. Igual que trabajarías con otra persona del equipo, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Después de la asignación, Copilot crea un pull request en borrador para registrar su progreso, configura el entorno y empieza a trabajar. Puedes explorar su sesión mientras está en curso o una vez haya terminado. Cuando la solución esté lista para que la revises, te mencionará en el pull request.

## La importancia de definir bien el alcance de las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el análisis. Incluso las tareas que parecen sencillas pueden ser complejas al examinarlas en detalle, así que [define cuidadosamente el alcance de las tareas para el agente en la nube][cloud-agent-best-practices]. Trátalo como un asistente de programación con IA: trabaja por etapas, aprende, experimenta y adapta el proceso. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas guían a Copilot. Si ya has completado el ejercicio sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción. Si no, este es un buen momento para echarle un vistazo. Antes de asignar tareas al agente en la nube, revisa los archivos de instrucciones que incluye el repositorio para que después puedas observar su efecto.

Abre estos archivos en la interfaz web de GitHub para tu repositorio o en un codespace que ya tengas iniciado:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las expectativas para las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentarios del pull request resultante. Proceden de estos archivos de instrucciones. Volveremos a señalarlo al revisar los PR en un ejercicio posterior.

## Configura el entorno de desarrollo para el agente en la nube de Copilot

El desarrollo suele requerir un entorno específico y algunos scripts de configuración para instalar las herramientas necesarias. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a una persona ingeniera de software.

El agente en la nube usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo con un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece a trabajar. Así puede acceder a las herramientas y dependencias necesarias. El taller ya ha preparado este flujo para facilitar el aprendizaje. Proporciona a Copilot Node.js, las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y poblada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en los pasos de configuración, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación - Node.js (ahora toda la aplicación usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`. Este trabajo se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en el pull request.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

Todos entendemos la importancia de la documentación, pero muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica suele quedar desatendida, lo que reduce la productividad y dificulta mantener el código o incorporar nuevas personas al equipo. Copilot destaca en la generación de documentación, así que esta es una tarea perfecta para asignar al agente en la nube. Trabajará en segundo plano para crear la documentación necesaria. En un ejercicio posterior volverás a revisar su trabajo.

1. Abre una pestaña nueva del navegador y visita tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el diálogo de incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre el nuevo pull request (PR), que tendrá un título parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas de Copilot puede tardar varios minutos en aparecer en el PR. Copilot está preparando el entorno —ejecuta el flujo de trabajo que acabas de ver—, analiza el proyecto y determina la mejor forma de abordar el trabajo.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología del pull request. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Quizá tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa que puedes desplazarte por la sesión en directo para ver cómo Copilot resuelve el problema: explora el código y comprende su estado, se detiene a pensar y decidir el plan más adecuado y crea código.

El proceso puede tardar varios minutos. Una de las principales ventajas del agente en la nube de Copilot es que trabaja de forma asíncrona y te permite centrarte en otras tareas. Aprovecharemos esa función y le asignaremos otra tarea; después, nos centraremos en añadir funciones a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente en la nube de GitHub Copilot es que permite repartir el trabajo para que tú puedas centrarte en unas tareas mientras el agente trabaja en otras. Puede que añadir una sección de juegos relacionados para el equipo de diseño no lleve mucho tiempo, pero ese tiempo se puede dedicar a otra tarea. ¡Asignemos el trabajo al agente en la nube de Copilot!

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el diálogo de incidencias.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el diálogo de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, verás varios emoji 👀 en el primer comentario de la incidencia. Indican que Copilot ya está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando con diligencia en tu segunda solicitud. El agente en la nube de Copilot trabaja como una persona ingeniera de software: no hace falta supervisarlo continuamente, puedes revisar el resultado cuando haya terminado. Ahora, centrémonos en crear y usar agentes personalizados.

## Resumen y próximos pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignar incidencias a Copilot para que las resuelva de forma asíncrona. Puedes usarlo para abordar deuda técnica, crear funcionalidades o ayudar a migrar código de un framework a otro.

Has explorado estos conceptos:

- cómo personalizar el entorno de generación de código.
- cómo garantizar que las operaciones se realizan de forma segura.
- la importancia de definir incidencias con un alcance claro.
- cómo asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes centrarte en crear y usar agentes personalizados. [El agente en la nube de Copilot también puede usar servidores MCP][cloud-agent-mcp] y puede acceder a instrucciones personalizadas, que hemos visto en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]

| [Lección siguiente: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
