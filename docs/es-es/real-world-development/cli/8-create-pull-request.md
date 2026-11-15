---
title: "Lección 8 - Crea y combina la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de las instrucciones, la modificación de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Es hora de revisarlos en conjunto y abrir una solicitud de incorporación de cambios. Combinaste manualmente la PR de la valoración con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de las instrucciones, el cambio de la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una misma conversación y rama, y lo incluirás en una única PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el proceso de combinación;
- inspeccionarás la funcionalidad completa y las pruebas de verificación;
- crearás la PR del filtrado;
- habilitarás Agent Merge solo después de revisarla y confirmarás que se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener el control de autorización de la combinación en manos del desarrollador.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para integrar una solicitud de incorporación de cambios. Cuando lo habilitas, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y actualiza la base mediante rebase cuando es necesario. Después, habilita la combinación automática de GitHub si el repositorio la permite.

Hasta ahora seleccionabas **Merge pull request** manualmente. Puedes delegar esa responsabilidad a Agent Merge. Antes de habilitarlo, revisa el trabajo y confirma que está listo.

## Usa Agent Merge para gestionar la PR

Ya has creado todo el código. Revisémoslo en conjunto, creemos la PR y dejemos que Agent Merge gestione el resto del proceso.

1. Vuelve al codespace.
2. Abre el cuadro de diálogo de agentes con `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo se complete correctamente, la combinará.
7. Comprueba que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo y, cuando el repositorio lo permita, completa manualmente la combinación revisada.

## Resumen y próximos pasos

Has automatizado varias partes del proceso de desarrollo: generar, probar y validar código, y ahora gestionar la solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el proceso de combinación;
- has inspeccionado la funcionalidad completa y las pruebas de verificación;
- has creado la PR del filtrado;
- has habilitado Agent Merge solo después de revisarla y has confirmado que se ha combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos y el uso compartido, y para delegar trabajo opcionalmente al agente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
