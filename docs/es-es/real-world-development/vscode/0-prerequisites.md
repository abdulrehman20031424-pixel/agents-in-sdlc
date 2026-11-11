---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, tendrás que preparar el entorno. Crearás una copia propia del repositorio Tailspin Toys, iniciarás un [codespace][codespaces] y confirmarás que GitHub Copilot Chat funciona en el editor.

## Configura el repositorio del taller

Para crear una copia del repositorio donde añadirás el código, crearás una instancia a partir de la [plantilla][template-repository]. La nueva instancia incluirá todos los archivos necesarios para el taller y trabajarás en ella durante los ejercicios.

1. Abre una ventana nueva del navegador y visita la [plantilla de Tailspin Toys][tailspin-template].
2. Crea una copia del repositorio seleccionando el botón **Use this template** en la página del repositorio del taller. Después, selecciona **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si vas a completar el taller en un evento organizado por GitHub o Microsoft, sigue las instrucciones de las personas mentoras. De lo contrario, puedes crear el repositorio nuevo en una organización donde tengas acceso a GitHub Copilot.

    ![Configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**), ya que volverás a utilizarla más adelante.

> [!NOTE]
> **La lista de tareas está lista**
>
> Al crear el repositorio a partir de la plantilla, también se crea automáticamente una lista de incidencias de GitHub. Trabajarás con ellas durante el taller; no tienes que crear ninguna.

## Crea un codespace

A continuación, usarás un codespace para completar los ejercicios del taller.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que te permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un IDE completo con compatibilidad con varios lenguajes, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

    ![Selecciona el botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un codespace.

    ![Crea un codespace](../../../_images/ex0-create-codespace.png)

El codespace tardará varios minutos en crearse, aunque el proceso es mucho más rápido que instalar todos los servicios manualmente. Mientras tanto, puedes explorar otras funciones de GitHub Copilot que veremos a continuación.

> [!CAUTION]
> Volverás al codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está diseñado para ejecutarse en un codespace o en un [contenedor de desarrollo][dev-containers] local. Ambos entornos incluyen todos los requisitos previos necesarios para facilitar el taller. Si prefieres trabajar de forma local, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite. VS Code creará el mismo contenedor de desarrollo que usa el codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usa GitHub Copilot Chat y el modo agente

Para usar el modo agente de GitHub Copilot Chat, debes tener la extensión GitHub Copilot Chat instalada en el IDE. Si usas un GitHub Codespace, ya debería estar instalada.

> [!TIP]
> Si aún no tienes instalada la extensión GitHub Copilot Chat, puedes [instalarla desde Visual Studio Code Marketplace][copilot-chat-extension]. También puedes abrir la vista Extensions de Visual Studio Code, buscar **GitHub Copilot Chat** y seleccionar **Install**.

Después de instalar la extensión, quizá tengas que autenticarte con tu cuenta de GitHub.

1. Vuelve a tu codespace.
2. Si Copilot Chat no aparece a la derecha del editor, selecciona su icono en la parte superior del codespace.
3. Escribe un mensaje como «Hello world» en la ventana Copilot Chat y presiona <kbd>Enter</kbd>. Copilot Chat debería activarse.
4. Si aún no te has autenticado, se te pedirá que inicies sesión en GitHub. Sigue las instrucciones para autenticarte.

    ![Ejemplo de un mensaje de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Después de autenticarte, debería aparecer la ventana Copilot Chat.

## Resumen

¡Enhorabuena! Has creado una copia del repositorio del taller y has iniciado la creación del codespace que usarás cuando empieces a escribir código.

## Próximo paso

Empecemos a trabajar con Copilot. Continúa con el [Ejercicio 1: Instrucciones personalizadas][next-lesson], donde enseñarás a Copilot las convenciones del proyecto.

## Recursos

- [Información general sobre GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Introducción a Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
