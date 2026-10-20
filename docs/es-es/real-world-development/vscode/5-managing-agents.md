---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo de agente de GitHub Copilot, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como todo ocurre en tu máquina y en tiempo real, Copilot Chat en VS Code muestra las llamadas a herramientas y las ediciones de archivos a medida que se producen. Puedes revisar cada diferencia en línea, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios de archivo propuestos.
- orientarás al agente durante la sesión para que añada un control de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha solicitado también un modo claro para las personas a quienes el tema predeterminado les resulta demasiado oscuro. En lugar de empezar otra conversación desde cero, orientarás la sesión existente para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat y observa cada llamada a herramientas. Verás lecturas y ediciones de archivos, además de los comandos del terminal que solicite ejecutar.
3. Cuando el agente proponga una edición, las diferencias aparecerán en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos que ha modificado el agente en esta sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso —por ejemplo, para ejecutar un comando del terminal—, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final que resuma los cambios realizados.

## Orientar al agente para que añada el modo claro

Ahora que el modo de alto contraste está listo, ampliarás la misma conversación para añadir un control de modo claro. Como continúas la sesión de chat, el agente conserva el contexto del trabajo anterior: los componentes que modificó, el enfoque de estilos y el patrón de almacenamiento local.

1. En el cuadro de entrada de Copilot Chat, envía este prompt de seguimiento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente retomará el trabajo, explorará los cambios que ya ha hecho y propondrá actualizaciones para el modo claro.
3. Revisa cada edición nueva cuando aparezca, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** para cada diferencia.
4. Si el agente toma una dirección que no quieres —por ejemplo, intenta refactorizar la interfaz de los controles de una forma que no te gusta—, deténlo con el botón **Stop** junto al cuadro de entrada y envía un mensaje de seguimiento que aclare lo que necesitas.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Envía prompts de seguimiento para pedir ajustes —«simplifica el CSS y usa variables en lugar de duplicar selectores»—, corregir el enfoque —«no añadas un tercer control; integra el modo claro en el panel de configuración existente»— o ampliar el alcance —«añade también atajos de teclado para ambos controles».

## Revisar los cambios

Antes de confirmar el trabajo, revisa rápidamente todos los archivos que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos modificados y examina las diferencias. Confirma que reflejan las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has usado el modo de agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición a medida que aparecía, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- has supervisado el trabajo del agente en tiempo real.
- has aceptado y rechazado cambios individuales en los archivos.
- has orientado una sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una pull request y revisarás los cambios que ha producido el agente personalizado local durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
