---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido al agente en la nube de Copilot que realice tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente está diseñado para trabajar de forma asíncrona y autónoma, sigue siendo importante supervisar las tareas.

Hay varias herramientas para gestionar las tareas que asignas al agente en la nube, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de agentes con pull requests (PR) abiertas, consultar las operaciones realizadas y orientar una sesión que esté en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear el modo de alto contraste, el equipo se ha dado cuenta de que también sería buen momento para añadir un modo claro. Como ya se está actualizando el estilo del sitio y se está añadiendo una función de alternancia, parece lógico incluirlo. Orientarás el trabajo del agente para que añada el modo claro además del alto contraste.

## Revisar las tareas del agente en la nube de Copilot

Veamos el estado de todas las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas, tanto en el panel principal como en el lateral. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que ha realizado Copilot, por ejemplo:
   - obtener el código del repositorio.
   - preparar el entorno de trabajo.
   - configurar servidores MCP.
   - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos que aparezcan dependerán del estado de la tarea y del enfoque que haya seguido Copilot.

4. Observa también el panel de pull requests de la derecha. Desde allí puedes ver la PR y los archivos modificados para supervisar mejor el trabajo.

## Orientar al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que incluya el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. Su título exacto dependerá del nombre que haya elegido Copilot y del estado de la tarea.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para orientar la sesión activa mientras Copilot trabaja resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que ha recibido el mensaje y lo incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Como antes, Copilot continuará con la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso que estaba realizando al recibir el mensaje.

El proceso puede tardar varios minutos. Mientras tanto, puedes hacer una pausa y repasar lo que has aprendido hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro para supervisar y orientar las tareas del agente en la nube de GitHub Copilot. Desde este centro puedes seguir las tareas activas y completadas, revisar el trabajo realizado e incluso redirigir las tareas en curso para ajustar su alcance o proporcionar más instrucciones.

Has explorado estos conceptos:

- has usado mission control y la página de agentes para supervisar las tareas del agente en la nube.
- has redirigido una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, pasaremos a [iterar sobre las pull requests que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
