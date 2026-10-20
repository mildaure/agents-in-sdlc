---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

A lo largo de este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que añada documentación al código, cree una sección de juegos relacionados para que el equipo de diseño pueda explorarla y añada funciones de accesibilidad, incluidos los modos de alto contraste y claro. Revisemos los cambios y, si hace falta, demos indicaciones a Copilot para mejorarlos.

### Escenario

Como ya hemos señalado varias veces, la IA generativa no cambia los fundamentos del diseño de software ni de DevOps. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Revisaremos las propuestas de GitHub Copilot para la documentación, la funcionalidad de juegos relacionados y las mejoras de accesibilidad antes de habilitar las revisiones para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot trabaja de forma asíncrona y sin supervisión directa, se aplican ciertas restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usa para su trabajo.
- El agente se ejecuta dentro de GitHub Actions, en un entorno efímero independiente.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que pueda ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empecemos revisando la primera pull request (PR) creada por el agente en la nube de GitHub Copilot: la que añade documentación al código. Usarás la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Al abrir la PR puede que veas un aviso indicando que un cortafuegos ha bloqueado a GitHub Copilot. Es **normal**: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o desactivar el cortafuegos del agente en la nube de Copilot][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la pull request titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la pull request tendrá la marca **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes hacer una pausa o repasar lo aprendido.

4. Cuando la pull request esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, incluidos los comentarios TSDoc y la documentación añadida. Los cambios exactos pueden variar.

   Mientras revisas los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Proceden de los archivos de instrucciones personalizadas que consultaste al principio del ejercicio 2.

6. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Busca un indicador que señale que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en la sección de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían completarse correctamente. El proceso puede tardar unos minutos.

## Pedir cambios a GitHub Copilot

La colaboración con Copilot en una pull request no es unidireccional. Puedes mencionarlo en comentarios generales o comentarios insertados en el código, igual que a otros miembros del equipo. Copilot leerá esos comentarios e iniciará otra sesión para responder. Como los resultados no son deterministas, no podemos proporcionar un prompt prescriptivo. Por ejemplo, podrías pedirle que:

- añada a cada archivo de código un encabezado que describa brevemente su propósito.
- añada comentarios TSDoc a los archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para pedir un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otro usuario. Puedes usar una de las ideas anteriores o sugerir otra mejora para la documentación del código.
2. Selecciona **View Session** para ver el trabajo de Copilot. Verás que inicia una sesión nueva para realizar los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

    ![Volver a la pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, verás una confirmación nueva en la pull request.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que el resultado te convenza. Cuando esté listo, convierte la PR de borrador en una PR lista para revisión y fusiónala en la rama principal.

![Convertir la PR en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Volvamos a la PR que Copilot creó para resolver la incidencia sobre la sección de juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Show related games on the game details page**.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Busca un indicador que señale que hay flujos de trabajo pendientes de aprobación.
7. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían completarse correctamente. Puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en el codespace para probar manualmente la funcionalidad de juegos relacionados. Ve al codespace, abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama que creó Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes iterar con el agente en la nube para pedirle cambios. Por ejemplo, puedes solicitar que ajuste la cantidad de juegos relacionados o que añada encabezados de comentarios y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Puedes pedir los cambios añadiendo un comentario en la pestaña **Conversation**; Copilot lo verá e iniciará otra sesión.

## Revisar las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir tanto el modo de alto contraste que asignaste en el ejercicio 3 como el modo claro que solicitaste en mission control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la pull request tendrá la marca **[WIP]**. En ese caso, espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - Los componentes de interfaz para alternar entre los modos.
   - El uso del almacenamiento local para conservar las preferencias.
   - Los cambios CSS o de estilos para los modos de alto contraste y claro.
   - Los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - El código JavaScript o TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Busca un indicador que señale que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían completarse correctamente. Puede tardar unos minutos.
10. **Opcional:** cambia a esta rama en el codespace para probar manualmente las funciones de accesibilidad. Ve al codespace, abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama que creó Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Comprueba que funcionan y que las preferencias se conservan al recargar la página.

Observa cómo el agente personalizado de accesibilidad ha orientado a Copilot para implementar estas funciones según las prácticas recomendadas de accesibilidad. Si detectas problemas o mejoras posibles, puedes mencionar a **@copilot** en un comentario para solicitar cambios, igual que hiciste con las PR anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube es más útil cuando puedes asignarle elementos reales del backlog y ocuparte de otras tareas. Para practicar, crea algunas incidencias más en tu repositorio y asígnalas a Copilot. Por ejemplo:

- Crear un formulario para que los patrocinadores expresen su interés en la página de detalles del juego.
- Implementar la paginación de la lista de juegos.
- Añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas del repositorio** para ver más adelante su efecto en el trabajo del agente en la nube.
- **Has asignado incidencias al agente en la nube de Copilot** y has observado cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y usado un agente personalizado** para accesibilidad y has añadido modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad mientras estaba en curso.
- **Has revisado e iterado sobre las pull requests del agente en la nube**, has mencionado a `@copilot` para solicitar cambios y has aprobado flujos de trabajo.

## Revisión y siguientes pasos

Has completado el recorrido del agente en la nube. Si quieres seguir explorando, los otros recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP directamente en el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de control de calidad, Playwright MCP, Agent Merge y el puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
