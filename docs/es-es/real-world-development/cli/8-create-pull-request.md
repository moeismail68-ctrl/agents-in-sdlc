---
title: "Lección 8 - Crea y combina la PR de la funcionalidad"
description: "Revisa en conjunto el filtrado, las instrucciones, la habilidad actualizada, el perfil de QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la habilidad actualizada, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es hora de revisarlos en conjunto y abrir una solicitud de incorporación de cambios. Tú combinaste la solicitud de incorporación de cambios (PR) de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones, la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido el flujo de trabajo de filtrado y calidad en una conversación y una rama, y lo incluirás todo en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- inspeccionarás el cambio completo de la funcionalidad y las pruebas de verificación;
- crearás la PR de filtrado;
- habilitarás Agent Merge solo después de revisarla y confirmarás que se ha combinado.

## Escenario

Durante el flujo de trabajo del filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo pendiente de la PR y mantener el control de la autorización para combinarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Cuando lo habilitas, Copilot resuelve los impedimentos que haya —corrige comprobaciones de integración continua (CI) con errores, responde a comentarios de revisión y vuelve a aplicar los cambios sobre la rama base cuando sea necesario— y, después, activa la combinación automática de GitHub cuando el repositorio lo permita.

Hasta ahora has seleccionado **Merge pull request** por tu cuenta. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y confirma que está listo antes de habilitarlo.

## Utiliza Agent Merge para gestionar la PR

Ya has creado el código. Ahora revisémoslo en conjunto, creemos la PR y dejemos que Agent Merge se ocupe del resto.

1. Vuelve al codespace.
2. Escribe `/agent` para abrir el cuadro de agentes.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo se complete correctamente, la combinará.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo que se muestra y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y próximos pasos

Has automatizado varias partes del proceso de desarrollo: generación de código, pruebas y verificación, y ahora la gestión de la solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- has inspeccionado el cambio completo de la funcionalidad y las pruebas de verificación;
- has creado la PR de filtrado;
- has habilitado Agent Merge solo después de revisar los cambios y has confirmado la combinación.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
