---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o ámbitos concretos de tu flujo de desarrollo. Al definir los agentes en archivos Markdown de la carpeta `.github/agents` del repositorio, puedes proporcionar instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos del ámbito para que Copilot trabaje con mayor eficacia. Los equipos pueden convertir sus conocimientos en agentes reutilizables: uno de accesibilidad que aplique los requisitos de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` en el proyecto o de forma global en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML con, como mínimo, `name` y `description`, seguido de una instrucción en Markdown que define el comportamiento, la experiencia y las pautas del agente.

### Diferencias entre los agentes personalizados y las habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] comparten algunos aspectos: ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar ciertas operaciones. La diferencia más sencilla es que un **agente personalizado** es quien realiza el trabajo y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y coordinan habilidades e incluso otros agentes para realizar el trabajo. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad; para hacerlo, podría utilizar habilidades como la de flujo de trabajo de pull requests o la de ejecución y gestión de pruebas.

> [!NOTE]
> No hay una única manera correcta de crear agentes personalizados. Como ocurre con todo lo relacionado con la IA, prueba distintas opciones y adáptalas a tu entorno y a tus necesidades.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio explorarás:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

## Escenario

Tailspin Toys se compromete a que todas las personas puedan usar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes han indicado que algunas personas tienen dificultades para leer el tema oscuro actual debido al contraste insuficiente entre el texto y el fondo. Para responder a este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante, así que quieres implementar esta funcionalidad cuanto antes. Para ello, usarás un agente personalizado.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisemos su contenido para entender cómo guiará a Copilot.

Vuelve a tu codespace y abre el archivo del agente personalizado de accesibilidad:

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> `name` y `description` son campos obligatorios del frontmatter de los agentes personalizados.

3. Lee las secciones siguientes, que presentan:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - una lista de errores y problemas habituales.

## Crea y asigna una incidencia

El panel de agentes es el centro para trabajar con agentes en tu entorno. Puedes asignar tareas al agente en la nube de Copilot, supervisarlas e incluso redirigir una sesión en curso para orientarla. Asignemos a Copilot la tarea de crear el modo de alto contraste.

1. Ve a tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el diálogo de incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Add high contrast mode to website` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el diálogo de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Diálogo de asignación del agente en la nube con el agente personalizado Accessibility resaltado](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y próximos pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot: asistentes de IA especializados para tareas y ámbitos concretos. Los agentes personalizados permiten convertir los conocimientos y estándares del equipo en agentes reutilizables que guían a Copilot para que trabaje con mayor eficacia.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

Mientras Copilot trabaja en el modo de alto contraste, puedes [supervisar y dirigir la sesión del agente][next-lesson] desde el panel de agentes.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Prepararse para usar agentes personalizados en una organización][org-custom-agents]
- [Prepararse para usar agentes personalizados en una empresa][enterprise-custom-agents]

---

| [← Lección anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Lección siguiente: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
