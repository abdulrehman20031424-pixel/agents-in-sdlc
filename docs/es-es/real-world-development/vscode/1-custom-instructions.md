---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Lección siguiente: Modo agente →][next-lesson]

El contexto es fundamental cuando trabajas con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita conocer información de fondo, conviene que disponga de ese contexto. Los [archivos de instrucciones][instruction-files] son una de las principales formas de proporcionar pautas sobre cómo generar código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las pautas de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones específicos de rutas.
- generarás el primer fragmento de datos para el filtrado (un ayudante para consultar editoriales) con las instrucciones actuales.
- añadirás un nuevo estándar general al archivo `.github/copilot-instructions.md`.
- repetirás el mismo mensaje y observarás cómo el código generado sigue el nuevo estándar.
- confirmarás los cambios de instrucciones y del fragmento de filtrado en `main` para que Copilot pueda usar las pautas actualizadas en el próximo ejercicio.

> [!CAUTION]
> El código generado puede no seguir algunos de los estándares que hayas definido. Copilot no es determinista. El objetivo del ejercicio es observar la tendencia del comportamiento después de actualizar las instrucciones, no obtener siempre el mismo resultado exacto.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo competente, Tailspin Toys tiene pautas y requisitos para sus prácticas de desarrollo. Entre ellos:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su función.

Con los archivos de instrucciones, te asegurarás de que Copilot dispone de la información adecuada para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que entienda mejor tu estilo y tus requisitos de programación. Te permiten orientar sus sugerencias y fragmentos de código. Puedes especificar las convenciones de código, las bibliotecas y los tipos de comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en todas las solicitudes del repositorio. Debe contener información del proyecto relevante para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras pautas generales.
- `.github/instructions/*.instructions.md` contiene instrucciones para tareas o tipos de archivos concretos. Puedes proporcionar pautas para lenguajes como TypeScript o Astro, o para tareas como crear componentes de interfaz de usuario o pruebas unitarias.

> [!NOTE]
> Al trabajar en un IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para sugerir la siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan las instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo` para generar código.
>
> Copilot también admite instrucciones en otros formatos, como AGENTS.md y CLAUDE.md. Consulta la [referencia de compatibilidad con las instrucciones personalizadas][custom-instructions-support].

### Prácticas recomendadas para gestionar archivos de instrucciones

Una conversación completa sobre cómo crear archivos de instrucciones queda fuera del alcance del taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén `copilot-instructions.md` centrado en pautas generales del proyecto, como su descripción, estructura y estándares de programación.
- Usa `*.instructions.md` para proporcionar pautas sobre tipos de archivos (pruebas unitarias, componentes Astro, capa de datos) o tareas concretas.
- Escribe en lenguaje natural. Expresa las pautas con claridad y proporciona ejemplos de código correctos e incorrectos.

No hay una única manera correcta de crear archivos de instrucciones ni de usar la IA. La experimentación te permitirá averiguar qué funciona para tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, verás que hay instrucciones para muchos tipos de tareas, como [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. La forma de hacerlo varía según el entorno; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección del entorno que estés usando explicará cómo hacerlo cuando corresponda.
>
> Si buscas plantillas o un punto de partida, explora [Awesome Copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones que incluye el repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para tareas diferentes. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las pautas anidadas para los flujos de trabajo de GitHub Actions. Estas instrucciones se aplican a cualquier interacción con Copilot en este repositorio.
3. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior, que define un patrón global relativo a la raíz del repositorio. En este caso, se aplica a archivos de prueba TypeScript, como los que coinciden con `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que puede seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás las reglas para los comentarios TSDoc y los encabezados de comentarios de los archivos.

## Crea una rama para los cambios

Sigamos algunas prácticas recomendadas y creemos una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo presionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indica cuando corresponde.

Para comprobar el efecto de las instrucciones personalizadas, empieza enviando un mensaje con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo mensaje para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot solo tenga el contexto necesario.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el ayudante.
3. Selecciona **Agent** en el selector de agentes de la vista Chat para que Copilot pueda modificar los archivos.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este mensaje:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios al código, a menudo en el archivo del ayudante y en las pruebas.
6. Observa que el ayudante propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esta convención procede de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot no es determinista, quizá añada comentarios de documentación aunque no se los pidas. No pasa nada: el objetivo es observar la mejora de coherencia tras actualizar las instrucciones.

## Añade un nuevo estándar para el repositorio

Como ya hemos indicado, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añadamos los estándares de programación al repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa cómo documenta los estándares de programación, pero aún no incluye requisitos de documentación dentro del código; por eso el ayudante generado no tenía comentarios TSDoc.
3. Añade las líneas siguientes después de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, las instrucciones pueden estar en el archivo del repositorio (`.github/copilot-instructions.md`) para proporcionar pautas generales, o en archivos `*.instructions.md` para lenguajes, tipos de archivos o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales como la regla de los comentarios de documentación.

## Vuelve a enviar el mensaje y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Vuelve a seleccionar `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía el **mismo mensaje** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Ayudantes de acceso a datos de editoriales para la plataforma de financiación colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta incluye ahora un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su id y nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos de editorial.
    */
   ```

Has guiado a Copilot para que siga un estándar del proyecto nuevo y lo aplique al código real sobre el que trabajarás en el ejercicio siguiente.

## Confirma los cambios de instrucciones y publica la rama

Los archivos de instrucciones son recursos del proyecto y se gestionan con el mismo sistema de control de versiones que los demás archivos. Confirmemos y publiquemos la rama.

1. Abre una ventana nueva del terminal presionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba en el terminal que están presentes las actualizaciones de instrucciones y del ayudante ejecutando:

   ```bash
   git status
   ```

3. Prepara y confirma las actualizaciones de instrucciones y todos los archivos modificados por Copilot para el ayudante:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra actualizaciones complementarias (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama en el repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina un pull request

Ya has publicado la rama. Ahora, crea un pull request y relaciónalo con la incidencia sobre los estándares de documentación de la lista de tareas. Puedes hacerlo manualmente, pero pidamos a Copilot que encuentre la incidencia, cree el PR para cerrarla y lo combine.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actuará en GitHub en tu nombre. La plantilla del proyecto ya configura las herramientas de GitHub para el espacio de trabajo, así que no hace falta configurar nada. La primera vez que Copilot use una herramienta de GitHub, VS Code quizá te pida que inicies sesión. Sigue los pasos para autorizarlo. Aprenderás cómo funciona esta conexión (Model Context Protocol) en un ejercicio posterior.

1. Abre Copilot Chat en tu codespace.
2. Presiona <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y comprueba que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que encuentre la incidencia relacionada con las instrucciones y cree un PR desde la rama actual que describa las actualizaciones de instrucciones y el nuevo ayudante para editoriales:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará el PR.
5. Cuando Copilot te pida **Allow** para realizar acciones en GitHub, revisa el comando y selecciona **Allow** según corresponda.
6. Cuando se haya creado el PR, pide a Copilot que lo combine y vuelva a `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot te pida **Allow** para realizar acciones en GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** según corresponda.

¡Has creado y combinado un pull request con ayuda de GitHub Copilot!

## Resumen y próximos pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un mensaje de generación de código y observar el resultado con las instrucciones *existentes*.
- añadir un nuevo estándar general a `.github/copilot-instructions.md`.
- volver a enviar el mismo mensaje y observar cómo el código propuesto sigue el nuevo estándar.
- confirmar los cambios de instrucciones y el ayudante en `main` para que el próximo ejercicio pueda aprovecharlos.

A continuación, aplicarás esas instrucciones en el [modo agente][next-lesson] mientras Copilot añade una funcionalidad que afecta a varios archivos.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
