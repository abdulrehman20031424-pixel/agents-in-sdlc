---
title: "Ejercicio 2 - Añadir funciones con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones, como ejecutar pruebas. El proceso habitual de desarrollo incluye buscar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, hacer otro cambio... y así sucesivamente.

El modo agente de Copilot puede ayudarte con estas tareas.

El modo agente de Copilot trabaja con más autonomía en el IDE. Se comporta de forma parecida a una persona desarrolladora: explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo usar el modo agente para añadir funciones al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite ciclos de respuesta rápidos e interactivos. El agente en la nube está diseñado para trabajar de forma asíncrona como una persona del equipo, interactuando contigo mediante incidencias y pull requests.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot puede explorar el proyecto e identificar los archivos pertinentes para hacer cambios coordinados.
- el modo agente de GitHub Copilot puede implementar funciones en la interfaz de usuario y la capa de datos.
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de combinarlos en el código.

## Escenario

A medida que aumenta la lista de juegos, quieres que las personas puedan filtrarlos por categoría y por editorial. En el ejercicio anterior ya añadiste un ayudante para consultar editoriales. Ahora terminarás el trabajo restante en la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio web Tailspin Toys

Antes de hacer cambios, exploremos el sitio web Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite mostrar una lista de juegos y ver sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz de cliente. Los ayudantes reutilizables para acceder a los datos están en `src/lib/`, y la interactividad se añade mediante un pequeño `<script>` de Astro con las API DOM estándar.

### Inicia el sitio web

Para facilitar la ejecución del sitio web, se ha incluido la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Puedes ejecutarla en GitHub Codespaces siguiendo estos pasos:

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal nuevo en el codespace presionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir una ventana del navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo presionada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace de localhost en el terminal de Codespaces, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al codespace, donde ahora se ejecuta el servidor web.

### Explora el sitio web

Cuando el sitio esté abierto, puedes explorar sus funciones. Entre ellas:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora la lista de tareas con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indica cuando corresponde.

La implementación inicial del sitio web funciona, pero queremos añadir más funciones. Empecemos revisando la lista de tareas. Al crear el proyecto a partir de la plantilla, también se crearon automáticamente varias incidencias. Pide a GitHub Copilot que te las muestre.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente puede planificar e implementar cambios en varios archivos, ejecutar comandos de terminal y usar herramientas.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Las personas autoras de este taller no expresan preferencia por un modelo concreto. Durante su elaboración usamos Claude Sonnet 4.5 y por eso aparece en estas instrucciones. El objetivo es que las sugerencias sean relativamente coherentes y la experiencia sea más fluida. Sin embargo, como los modelos de lenguaje no son deterministas, las sugerencias pueden diferir de los ejemplos del taller. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje, Copilot quizá use un comando MCP diferente, pero debería poder completar la tarea.

3. Envía este mensaje a Copilot para preguntarle por las incidencias pendientes:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que muestra todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot ha ordenado las incidencias según lo útiles que considera que serán para las personas usuarias.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio web.

1. En tu codespace, abre `.github/instructions/ui.instructions.md`.
2. Fíjate en las pautas generales para añadir funciones. Incluyen:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, pruebas y accesibilidad.
   - enlaces a archivos de instrucciones para distintos tipos de archivos, como:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Los archivos de instrucciones permiten enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así se pueden reutilizar pautas y dividir conjuntos de instrucciones grandes en partes más manejables, además de proporcionar ejemplos y plantillas a Copilot.

## Implementa los filtros

Para completar el filtrado, tendrás que hacer al menos tres cambios distintos en la aplicación:

- añadir o perfeccionar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de los filtros.
- actualizar la página de la lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios en el código. ¡El modo agente de Copilot puede realizar estas tareas! Añadamos la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o seleccionar **New Chat** para empezar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de cómo seleccionar el archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el selector de agentes de la vista Chat.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada de la lista:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad. Deberías ver que encuentra los ayudantes de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código. Mientras trabaja, quizá veas los controles **Keep** y **Undo** en el editor. Cuando termine, aparecerán para todos los cambios; no tienes que seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Diálogo de Copilot Chat que pide confirmación para ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alternará entre la generación de código y las pruebas hasta completar la tarea y no detectar errores.

   ![Sesión completa de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o por cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio web y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y próximos pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo agente de GitHub Copilot para añadir funciones al sitio web Tailspin Toys. Hemos visto cómo:

- el modo agente de GitHub Copilot puede implementar funciones en la interfaz de usuario y la capa de datos.
- el modo agente de Copilot puede explorar el proyecto e identificar los archivos pertinentes para hacer cambios coordinados.
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de combinarlos en el código.

A continuación, [probaremos la funcionalidad con el servidor Playwright MCP][next-lesson] y abriremos un pull request.

### Ejercicio de exploración adicional: implementar paginación

A medida que aumenta la lista de juegos, será necesario añadir paginación. Usa lo que has aprendido para pedir a Copilot que actualice el sitio. Al escribir el mensaje, considera:

- seguir las prácticas recomendadas y usar los archivos de instrucciones existentes.
- cómo quieres implementar la paginación, por ejemplo, si las personas usuarias podrán seleccionar el tamaño de página o si será fijo.
- proporcionar a Copilot la información necesaria para implementar la paginación de la forma que necesitas.
- iterar con GitHub Copilot y proporcionar cambios y contexto. Es lo habitual al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
