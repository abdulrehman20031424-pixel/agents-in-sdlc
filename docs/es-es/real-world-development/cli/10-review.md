---
title: "Lección 10 - Repaso y próximos pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, los dos pull requests, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI en un flujo de trabajo continuo de Tailspin Toys. Has:

- preparado un codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado preparada.
- añadido valoraciones por estrellas, revisado el resultado en un navegador reenviado y combinado manualmente tu primer pull request (PR).
- partido de la incidencia de filtrado, definido el enfoque en el modo Plan, creado la funcionalidad con Autopilot y revisado el resultado en Interactive.
- guiado al agente con instrucciones personalizadas, personalizado la habilidad `quality-checks` existente y usado la habilidad para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadido el servidor Playwright Model Context Protocol (MCP) y usado el servidor para explorar el filtrado en un navegador real.
- creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las evidencias del navegador.
- revisado todo el cambio de filtrado y autorizado Agent Merge para el PR de filtrado.
- explorado comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Qué has entregado

El taller incluye dos hitos de PR, cada uno en una rama propia creada a partir de `main` actualizado:

1. **Valoraciones por estrellas:** muestra `starRating` y un estado explícito sin valoración en las tarjetas de juegos.
2. **Flujo de trabajo de filtrado y calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, has usado la misma conversación y rama. Hemos agrupado el trabajo en un solo PR para agilizar el taller.

## Distintas formas de verificación

Has comprobado el código de varias maneras: con pruebas automáticas, inspeccionando el navegador y dejando que Copilot explorase el navegador mediante MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, lint y comprobaciones de tipos, y generó un informe en el nuevo formato. El agente de QA reunió estos resultados con la revisión de los requisitos y la cobertura de pruebas antes del PR.

Las pruebas nuevas deben cubrir carencias reales; si el agente de QA no necesita añadir pruebas, puede ser correcto. Revisa el código y las evidencias antes de autorizar la combinación, y actualiza las evidencias afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en distintas conversaciones y ajústalas a las necesidades del equipo. Las instrucciones definen estándares, las habilidades describen tareas repetibles, los agentes personalizados establecen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas; no te limites al resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para considerar el enfoque antes de empezar, **Interactive** para mantener el control de los cambios acotados y **Autopilot** para tareas bien definidas. Elige un modelo más rápido para ediciones rutinarias y uno más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir claramente *qué* quieres crear, *por qué* y *cómo* cambia mucho el resultado.

## Más cosas que explorar

Ya has cubierto el flujo de trabajo principal. Estas funciones de CLI también pueden ser útiles:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar una tarea distinta.
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Próximos pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola. Úsala en código de producción, en proyectos personales o para crear esa pequeña aplicación que tienes en mente desde hace años. Comparte lo aprendido con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Incorporar Foundry][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar pull requests con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
