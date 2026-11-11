---
title: "Lección 8 - Crear y combinar el PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea un PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Es el momento de revisarlos en conjunto y abrir un pull request. Combinaste manualmente el pull request (PR) de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones, la actualización de la habilidad y el agente de QA en varios PR. Para agilizar el taller, has mantenido todo el trabajo de filtrado y calidad en una conversación y una rama, y lo incluirás en un solo PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionarás todo el cambio de la funcionalidad y las evidencias de verificación.
- crearás el PR de filtrado.
- activarás Agent Merge solo después de la revisión y confirmarás que el PR se ha combinado.

## Escenario

Durante el flujo de trabajo del filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante del PR y dejar la autorización para combinarlo en manos de la persona desarrolladora.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar un pull request. Al activarlo, Copilot resuelve los bloqueos del PR —por ejemplo, corrige comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a aplicar la base cuando hace falta— y, después, activa la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esta tarea. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usa Agent Merge para gestionar el PR

Ya tienes todo el código. Revisémoslo, creemos el PR y dejemos que Agent Merge se encargue del resto del proceso.

1. Vuelve a tu codespace.
2. Abre el diálogo de agentes con `/agent`.
3. Selecciona **Default** en la lista y presiona <kbd>Enter</kbd>.
4. Crea un PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua del PR. Cuando todo salga bien, lo combinará.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no evita las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y próximos pasos

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo, y ahora gestionar el pull request. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionaste todo el cambio de la funcionalidad y las evidencias de verificación.
- creaste el PR de filtrado.
- activaste Agent Merge solo después de la revisión y confirmaste que el PR se había combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Recursos

- [Gestionar pull requests con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
