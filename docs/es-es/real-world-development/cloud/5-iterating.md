---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que añada documentación al código, cree una funcionalidad de juegos relacionados para que el equipo de diseño pueda iterar y añada funciones de accesibilidad con modos de alto contraste y claro. Revisemos los cambios y, si hace falta, enviemos comentarios a Copilot para mejorarlos.

### Escenario

Como ya hemos indicado varias veces, los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Con esto en mente, revisemos las sugerencias de GitHub Copilot para añadir documentación, crear la funcionalidad de juegos relacionados y mejorar la accesibilidad antes de permitir que el resto del equipo revise los cambios.

## Seguridad y agente en la nube de GitHub Copilot

El agente en la nube de Copilot realiza las tareas de forma asíncrona y sin supervisión, así que tiene varias restricciones de seguridad para garantizar que todo siga siendo seguro:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usará para su código.
- El agente en la nube se ejecuta en GitHub Actions, que crea un entorno independiente y temporal para su trabajo.
- Una persona debe aprobar todos los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por el primer pull request (PR) generado por el agente en la nube de GitHub Copilot: añadir documentación al código. Usarás la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Al explorar el PR, quizá veas una advertencia que indica que el firewall bloqueó a GitHub Copilot. Es lo esperado: el acceso del agente a recursos externos está limitado de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube de Copilot][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de pull requests.
3. Abre el pull request con un título parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, el pull request tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes aprovechar para descansar o reflexionar sobre lo que has aprendido.

4. Cuando el pull request esté listo, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que ahora incluye comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

    Mientras revisas los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Proceden de los archivos de instrucciones que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en la sección de comprobaciones del pull request. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar varios minutos.

## Pide cambios a GitHub Copilot

Trabajar con Copilot en un pull request no es una interacción unidireccional. Puedes mencionar a Copilot en comentarios, como harías con otra persona del equipo, en el pull request o en comentarios en línea del código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos darte un mensaje específico que debas usar. Algunas ideas de cambios que puedes solicitar:

- Añadir encabezados de comentarios al principio de cada archivo de código con una breve descripción de su contenido.
- Añadir comentarios TSDoc a los archivos TypeScript y Astro.
- Crear un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Usa una de las ideas anteriores o pide otro cambio de documentación que quieras ver en el código.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Verás que inicia una sesión nueva para hacer los cambios.
3. Puedes seleccionar **Back to pull request** para volver al pull request.

    ![Volver al pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, aparecerá un commit nuevo en el pull request.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, puedes marcar el PR en borrador como listo para revisión y combinarlo con la rama principal.

![Marcar el PR como listo para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Volvamos al PR que Copilot ha generado para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona el PR con un título parecido a **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que algunos flujos de trabajo están pendientes de aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos deberían aparecer en la sección de comprobaciones del pull request. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar varios minutos.
9. **Opcional:** puedes cambiar a esta rama en tu codespace para probar manualmente la funcionalidad de juegos relacionados. Ve al codespace, abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

¡Copilot ha creado la funcionalidad de juegos relacionados! Como antes, puedes trabajar de forma iterativa con el agente en la nube y solicitar actualizaciones. Por ejemplo, podrías pedirle que cambie el número de juegos relacionados que muestra o que añada encabezados de comentarios y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Como en los ejercicios anteriores, puedes pedir cambios añadiendo un comentario en la pestaña **Conversation**. Copilot lo verá e iniciará una sesión nueva.

## Revisa las funciones de accesibilidad

Por último, revisemos las funciones de accesibilidad implementadas mediante el agente personalizado de accesibilidad. Este PR debería incluir el modo de alto contraste que asignaste en el ejercicio 3 y el modo claro que solicitaste desde el panel de agentes en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona el PR con un título parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot aún está trabajando en la tarea, el pull request tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de interfaz para cambiar entre modos.
   - el uso del almacenamiento local para guardar las preferencias.
   - los cambios de CSS o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - el código JavaScript/TypeScript que gestiona los cambios de modo.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en la sección de comprobaciones del pull request. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar varios minutos.
10. **Opcional:** puedes cambiar a esta rama en tu codespace para probar manualmente las funciones de accesibilidad. Ve al codespace, abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los modos de alto contraste y claro en el navegador para comprobar que funcionan y que la configuración se conserva al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funciones siguiendo las prácticas recomendadas de accesibilidad. Si detectas algún problema o mejora, puedes mencionar a **@copilot** en un comentario y pedirle que actualice los cambios, como hiciste con los otros PR.

## Ejercicio opcional: sigue delegando

El agente en la nube funciona mejor cuando le asignas elementos reales de la lista de tareas y puedes centrarte en otra cosa. Para practicarlo, crea más incidencias en tu repositorio y asígnaselas a Copilot. Por ejemplo:

- Crear un formulario para que las personas interesadas en los juegos puedan dejar sus datos en la página de detalles.
- Implementar paginación en la página de la lista de juegos.
- Añadir validación de datos y gestión de errores a los ayudantes de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizados del repositorio** para observar su efecto en el trabajo del agente en la nube.
- **Has asignado incidencias al agente en la nube de Copilot** y has visto cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y usado un agente personalizado** para mejorar la accesibilidad, con modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar la sesión de accesibilidad y dirigirla durante el trabajo.
- **Has revisado los pull requests del agente en la nube e iterado sobre sus cambios**, mencionando a `@copilot` para solicitar actualizaciones y aprobando flujos de trabajo.

## Repaso y próximos pasos

Has completado el itinerario del agente en la nube. Si quieres seguir explorando, los otros itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y el comando opcional `/delegate` para volver a usar el agente en la nube.

Prueba estas ideas en tu propio repositorio:

- Asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
