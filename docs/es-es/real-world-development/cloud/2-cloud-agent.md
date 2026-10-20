---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que haya pocas organizaciones que no tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o funcionalidades que llevan tiempo en el backlog porque nunca ha habido tiempo para implementarlas. El agente en la nube de GitHub Copilot está diseñado para tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea una pull request (PR) de borrador para que un desarrollador la revise. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en objetivos más amplios.

Con el agente en la nube de Copilot explorarás:

- cómo personalizar el entorno de generación de código.
- cómo garantizar que las operaciones se realizan de forma segura.
- la importancia de definir incidencias con un alcance claro.
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere abordar. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado deficiente: comprobarás que no hay ninguna. Como primer paso, el equipo quiere añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere facilitar el descubrimiento de juegos. Le gustaría que la página de detalles de cada juego mostrase juegos relacionados —otros títulos de la misma categoría— para que los visitantes puedan seguir explorando. Por ahora no necesitan una implementación pulida, sino algo que puedan usar para probar la experiencia de usuario. La tarea bloquea ese trabajo, pero hay otras incidencias con mayor prioridad.

Ambos casos son tareas que pueden perder prioridad fácilmente y que se pueden asignar al agente en la nube de Copilot. El agente trabaja en segundo plano mientras tú te ocupas de otras tareas y, cuando termina, puedes revisar su trabajo.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Puedes iniciar el trabajo de varias maneras, por ejemplo, [asignando una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Al asignarla, Copilot crea una pull request de borrador para seguir el progreso, prepara el entorno y empieza a trabajar. Puedes consultar la sesión mientras está en curso o después de que termine. Cuando la solución esté lista para revisarla, Copilot te mencionará en la pull request.

## La importancia de definir bien el alcance de las instrucciones

GitHub Copilot no hace magia: no elimina la necesidad de pensar. Incluso las tareas aparentemente sencillas pueden resultar complejas cuando se analizan a fondo, así que [define con cuidado el alcance de las tareas para el agente en la nube de Copilot][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. La IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has aprendido cómo orientan a Copilot las instrucciones personalizadas. Si ya has completado el ejercicio práctico sobre instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`; si no, aprovecha para consultarlo. Antes de asignar trabajo al agente en la nube, examina en modo de solo lectura los archivos de instrucciones incluidos en este repositorio para reconocer sus efectos más adelante.

Abre estos archivos en la interfaz web de GitHub o en un codespace que ya tengas en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las expectativas sobre las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que limita estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la siguiente sección, comprueba si la pull request incluye comentarios TSDoc, sigue las convenciones de TypeScript y añade encabezados de comentarios. Estas expectativas proceden de los archivos de instrucciones y volveremos a comentarlas al revisar las PR en una lección posterior.

## Configurar el entorno de desarrollo del agente en la nube

La creación de código suele requerir un entorno específico y scripts de configuración para que todo esté listo. Lo mismo sucede al asignar tareas a Copilot, que trabaja de manera parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] para preparar su entorno. Puedes personalizarlo con un [flujo de trabajo de configuración][setup-workflow] especial, definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece a trabajar. Así puede acceder a las herramientas y dependencias necesarias. Este flujo ya está configurado para facilitar el taller y ofrecer una oportunidad de aprendizaje. Proporciona a Copilot Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos iniciales para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Configura los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para realizar sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de la configuración, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (ahora toda la aplicación usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos iniciales en SQLite local para que la compilación y las pruebas dispongan de datos.
      - name: Set up the database
        run: npm run db:setup
```

Este archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Incluye un solo trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la pull request.
- El desencadenador `workflow_dispatch` permite ejecutar el flujo manualmente desde la pestaña **Actions** del repositorio. Esto resulta útil para comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añadir documentación

Todo el mundo entiende la importancia de la documentación, pero la mayoría de los proyectos tiene información desactualizada o carece de ella. Esta deuda técnica puede reducir la productividad y dificultar el mantenimiento del código o la incorporación de desarrolladores. Copilot puede ayudar a crear documentación, por lo que esta es una buena incidencia para asignar al agente en la nube. Trabajará en segundo plano y, en un ejercicio posterior, revisarás el resultado.

1. En una pestaña nueva del navegador, ve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el formulario de incidencias.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Code lacks documentation`.
6. En **Description**, escribe:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el panel de la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva pull request, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si no aparece ninguna PR nueva, espera uno o dos minutos y actualiza la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer en la PR. Copilot está preparando el entorno —ejecutando el flujo de trabajo descrito antes—, analizando el proyecto y decidiendo cómo abordar el problema.

13. Revisa la lista y las tareas que realizará.
14. Desplázate por la cronología de la pull request y busca una actualización que indique que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot resuelve el problema: explora el código y su estado, se detiene a pensar y elegir un plan y, después, crea código.

El proceso puede tardar varios minutos. Una de las ventajas principales del agente en la nube de Copilot es que puede trabajar de forma asíncrona y dejarte tiempo para otras tareas. Aprovecharemos esa posibilidad asignándole otra tarea mientras nos ocupamos de añadir una funcionalidad a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una gran ventaja del agente en la nube de GitHub Copilot es que permite repartir el trabajo: tú puedes centrarte en unas tareas mientras el agente se ocupa de otras. Añadir una sección de juegos relacionados quizá no lleve mucho tiempo, pero puedes dedicar ese tiempo a otra cosa. Asignemos la tarea al agente en la nube.

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el formulario de incidencias.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. En **Title**, escribe `Show related games on the game details page`.
6. En **Description**, escribe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el panel de la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, verás una serie de 👀 en el primer comentario de la incidencia. Indica que Copilot está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda tarea. Como el agente en la nube trabaja de manera parecida a un ingeniero de software, no hace falta supervisarlo constantemente; puedes revisar el resultado cuando termine. Ahora veremos cómo crear y usar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona. Sirve para abordar deuda técnica, crear funcionalidades o ayudar a migrar código de un framework a otro.

Has explorado estos conceptos:

- cómo personalizar el entorno de generación de código.
- cómo garantizar que las operaciones se realizan de forma segura.
- la importancia de definir incidencias con un alcance claro.
- cómo asignar incidencias a Copilot.

Mientras el agente en la nube trabaja en segundo plano, nos centraremos en crear y usar agentes personalizados. El [agente en la nube de Copilot también puede usar servidores MCP][cloud-agent-mcp] y las instrucciones personalizadas que hemos explorado en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
