---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar tareas opcionalmente al agente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye muchos comandos de barra. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. En esta lección explorarás otros comandos útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usarás `/model` para explorar los modelos disponibles.
- aprenderás cómo `/share` puede exportar o compartir una sesión.
- explorarás comandos opcionales para trabajar en paralelo, usar worktrees y delegar tareas al agente en la nube.

## Escenario

Ya has completado el flujo de trabajo principal de CLI. Ahora explorarás otras capacidades: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar trabajo opcionalmente en el [agente en la nube de Copilot][about-cloud-agent].

## Explorar el contexto de Copilot CLI

Al trabajar en tareas grandes o complejas, puedes acercarte al límite máximo de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando hace falta, y puedes inspeccionarla o compactarla mediante comandos de barra.

1. Vuelve a tu codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y cómo se distribuye el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Puede que no haya un cambio importante si la conversación aún es breve.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto a medida que se llena la ventana. Usa `/compact` para decidir cuándo compactarlo. Usa `/clear` o `/new` si vas a cambiar a una tarea que no está relacionada y quieres empezar una conversación nueva.

## Elegir un modelo

Los distintos modelos tienen puntos fuertes diferentes y cada desarrollador tiene sus propias preferencias. Copilot CLI permite ver y seleccionar el modelo que quieras usar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Colaborar y compartir lo que se aprende ayuda a todo el equipo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartirla o publicar un GitHub gist.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en la versión instalada.
2. Si quieres compartir la sesión, elige el destino que mejor se adapte a tus necesidades, por ejemplo, `/share file` para exportarla a un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo a alguien o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de la conversación que el equipo no tenga previsto compartir.

## Opcional: ampliar el trabajo o delegarlo

Has completado el taller principal. Copilot CLI también ofrece comandos para tareas más amplias:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un worktree aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente en la nube de Copilot, que trabaja de forma asíncrona y puede abrir una pull request.

Estos comandos son opcionales porque pueden crear worktrees adicionales o trabajo remoto. Antes de probarlos, empieza una tarea nueva y bien delimitada. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI te permiten configurarlo, compartir sesiones y consultar qué ocurre entre bastidores. En esta lección:

- has usado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- has usado `/model` para explorar los modelos disponibles.
- has aprendido cómo `/share` puede exportar o compartir una sesión.
- has explorado comandos opcionales para trabajar en paralelo, usar worktrees y delegar tareas al agente en la nube.

Hay muchos otros comandos de barra que puedes explorar. Terminemos el recorrido [repasando lo aprendido][next-lesson] y algunos próximos pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente en la nube de Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
