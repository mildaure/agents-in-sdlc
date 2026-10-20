---
title: "Lección 8 - Crear y fusionar la pull request de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la habilidad, el perfil de control de calidad y las pruebas; después, crea una pull request y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad y las pruebas están guardados en una misma rama. Es el momento de revisarlos conjuntamente y abrir una pull request. Fusionaste tú la pull request (PR) de la valoración por estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo habitual sería dividir la funcionalidad, las actualizaciones de instrucciones, la actualización de la habilidad y el agente de control de calidad en varias PR. Para simplificar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una sola conversación y rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de la fusión.
- inspeccionarás todos los cambios de la funcionalidad y las evidencias de verificación.
- crearás la PR del filtrado.
- habilitarás Agent Merge solo después de revisar los cambios y confirmarás que la PR se ha fusionado.

## Escenario

A lo largo del flujo de trabajo del filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el resto del trabajo de la PR, manteniendo el control de la autorización de fusión en manos del desarrollador.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para fusionar una pull request. Cuando lo habilitas, Copilot resuelve los problemas que bloquean la PR —por ejemplo, corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y actualiza la rama base cuando sea necesario— y, después, habilita la fusión automática de GitHub si el repositorio lo permite.

Hasta ahora, tú mismo has seleccionado **Merge pull request**. Agent Merge puede encargarse de esa responsabilidad. Antes de habilitarlo, revisa el trabajo y decide si está listo.

## Usar Agent Merge para gestionar la PR

Con el código creado, revisémoslo, creemos la PR y dejemos que Agent Merge gestione el resto del proceso.

1. Vuelve a tu codespace.
2. Escribe `/agent` para abrir el cuadro de diálogo de agentes.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando se complete correctamente, realizará la fusión.
7. Comprueba que Copilot muestre un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si el proceso se bloquea, lee el motivo indicado y, si el repositorio lo permite, completa manualmente la fusión después de revisarla.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación, y ahora el proceso de pull request. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de la fusión.
- has inspeccionado todos los cambios de la funcionalidad y las evidencias de verificación.
- has creado la PR del filtrado.
- has habilitado Agent Merge solo después de revisar los cambios y has confirmado que la PR se ha fusionado.

A continuación, explorarás [otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto y los modelos, compartir sesiones y delegar tareas opcionalmente al agente en la nube.

## Recursos

- [Gestionar pull requests con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
