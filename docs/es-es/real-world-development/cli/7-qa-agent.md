---
title: "Lección 7 - Crear y usar un agente de QA"
description: "Crea un agente personalizado de QA que combina los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la habilidad `quality-checks` para ejecutar comprobaciones automáticas y Playwright MCP para observar la experiencia de filtrado en un navegador. Ahora reunirás estas funciones en un agente personalizado con un proceso de QA bien definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- crearás e inspeccionarás un perfil de control de calidad reutilizable.
- seleccionarás el agente de QA y revisarás sus conclusiones respecto a la incidencia de filtrado.

## Escenario

Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automáticas, la cobertura de pruebas y el comportamiento en el navegador antes de abrir un pull request (PR). Un agente personalizado puede coordinar el proceso de QA y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil de Markdown. El perfil describe el propósito, las instrucciones y las herramientas disponibles para el agente. En este taller definirás el rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has usado cumplen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` agrupa comprobaciones repetibles. Playwright MCP proporciona herramientas para el navegador. El perfil de QA indica a Copilot cómo utilizar estas funciones para evaluar los requisitos y comunicar los resultados. No sustituye a los demás elementos ni requiere otra conversación.

## Crea el perfil de QA

Antes de abrir el PR de la funcionalidad, pedirás a Copilot que cree un perfil de QA reutilizable. El perfil definirá las comprobaciones y los límites que debe respetar el agente.

1. Vuelve a tu codespace y confirma que la conversación de filtrado está en el modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecciona el perfil

Antes de usar el agente nuevo, revisa su perfil para confirmar que Copilot ha captado el flujo de QA y los límites previstos. Así evitarás que un perfil incompleto o demasiado amplio modifique la funcionalidad cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero incluirlo da al agente un nombre claro en la interfaz.
3. Lee las instrucciones del perfil y confirma que el proceso de QA parte de los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y usa Playwright MCP.
4. Confirma que el agente presenta evidencias, pide permiso antes de cambiar el código de implementación y no confirma cambios ni abre pull requests.
5. Si falta alguna responsabilidad o límite, pide al agente predeterminado que corrija el perfil antes de continuar.

## Ejecuta el control de QA para la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Después de crear el perfil, reanuda la conversación de filtrado y selecciona QA para que pueda usar la incidencia y las decisiones de planificación que ya están en el contexto.

1. Activa el agente con este comando:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, quizá no aparezca en la lista de agentes disponibles. Aun así, el comando anterior lo activará.

2. Envía este mensaje para pedir al agente de QA que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. El agente de QA empezará a trabajar.
4. Cuando termine, lee el informe.

## Resumen y próximos pasos

Has añadido un rol especializado reutilizable al flujo de trabajo y has revisado sus resultados. En esta lección:

- exploraste cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- creaste e inspeccionaste un perfil de control de calidad reutilizable.
- seleccionaste el agente de QA y revisaste sus conclusiones respecto a la incidencia de filtrado.

Ya tienes la implementación, la actualización de la habilidad, el perfil de QA, las pruebas y el informe de verificación listos para revisarlos. A continuación, [los reunirás en un PR de funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
