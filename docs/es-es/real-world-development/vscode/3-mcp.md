---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo de agente. Antes de abrir una pull request, confirma que funciona en el navegador. En vez de recorrer la aplicación manualmente, conectarás el **servidor MCP de Playwright** y dejarás que Copilot controle un navegador real para probar la funcionalidad. Después, publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo amplían los servidores MCP las herramientas disponibles en Copilot.
- añadirás el servidor MCP de Playwright al espacio de trabajo.
- pedirás a Copilot que lo use para probar manualmente la funcionalidad de filtrado en un navegador.
- publicarás la rama y abrirás una pull request con los cambios de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es mucho más potente cuando puede acceder a recursos externos al editor. Copilot lo consigue mediante Model Context Protocol (MCP), un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama que muestra el funcionamiento interno del modo de agente y cómo interactúa con el contexto, el LLM y las herramientas, incluidas las que proporcionan servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Se accede a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. El servidor gestiona la comunicación entre el agente y las herramientas externas, como las API existentes o herramientas locales como los paquetes de NPM. Cada servidor MCP proporciona un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos son:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a API para gestionar repositorios de GitHub. Permite al agente crear repositorios, actualizar los existentes y gestionar incidencias y pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona automatización del navegador mediante Playwright. Permite al agente navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que proporcionan acceso a diferentes herramientas y recursos. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> En materia de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa cuidadosamente su código fuente, verifica quién lo publica y valora las implicaciones de seguridad. Usa solo servidores en los que confíes y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu codespace.
2. Deberías ver que ya hay un servidor `github` configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto ya incluye esta entrada `github`; por eso Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Usa el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hay que instalarlo localmente: VS Code se conecta por HTTP y, la primera vez que Copilot usa una de sus herramientas, te autenticas con GitHub.

## Añadir el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador controlable para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`, de modo que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en tu máquina mediante `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin una ventana visible, algo necesario dentro de un codespace, que no tiene un escritorio donde mostrarla.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para sus pruebas de extremo a extremo, así que normalmente el navegador necesario ya está instalado. Si Copilot informa más adelante de que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Iniciar y confiar en el servidor

VS Code inicia los servidores MCP **cuando hacen falta**. La primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json`, aparece la acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pide que confirmes que confías en el servidor, revisa la configuración y elige confiar en él para que pueda iniciarse.

El inicio manual es opcional. Si lo omites, VS Code iniciará el servidor y te pedirá que confirmes que confías en él la primera vez que Copilot necesite sus herramientas en el paso siguiente.

## Probar la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real que puede controlar. En vez de recorrer la aplicación para comprobar el trabajo, el agente puede abrir una página, navegar, aplicar filtros y leerte los resultados para después resumir lo que ha observado. Así puedes confirmar rápidamente el comportamiento de una funcionalidad sin salir de la conversación.

Internamente, el servidor MCP de Playwright usa el [árbol de accesibilidad][playwright-mcp-server] de la página, no capturas de pantalla. El agente razona sobre elementos estructurados y etiquetados —botones, enlaces y elementos de lista— igual que la tecnología de asistencia. Por eso, una comprobación funcional rápida también sirve como verificación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe el filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te contará lo que ha observado. Compara el informe con los criterios de aceptación de la incidencia. Si algo no parece correcto, haz preguntas de seguimiento o pídele que corrija el código antes de abrir una pull request.

> [!NOTE]
> Para esta prueba, la aplicación debe estar ejecutándose en `http://localhost:4321`. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot usa Playwright MCP puede que tenga que descargar un navegador. Si informa de que falta uno, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una pull request

Ya has confirmado que la funcionalidad funciona. Ahora puedes abrir una pull request (PR) para que el equipo la revise. Primero, publicarás la rama `filtering-vscode`.

1. En el codespace, abre el panel **Source Control** y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crear la pull request

Puedes crear una pull request de varias maneras, por ejemplo, desde github.com o con la interfaz de línea de comandos (CLI) de GitHub. Como ya estás usando GitHub Copilot, pídele que cree la PR. Puede encontrar la incidencia relacionada y asociarla a la PR.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Comprueba que **Agent** está seleccionado en el desplegable de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones necesarias. La primera vez que use una herramienta de GitHub, puede que tengas que autenticarte; sigue las instrucciones.
5. Observa cómo Copilot busca entre las incidencias, encuentra la correcta y crea la PR.
6. Selecciona el enlace generado por Copilot para revisar la pull request, pero **no la fusiones todavía**.

## Resumen y siguientes pasos

En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo amplían sus servidores las herramientas disponibles en Copilot.
- has añadido el servidor MCP de Playwright al espacio de trabajo.
- lo has usado para probar manualmente la funcionalidad de filtrado en un navegador antes de publicarla.
- has publicado la rama y abierto una pull request con los cambios de filtrado.

Has usado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una de las maneras de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué demonios es MCP y por qué todo el mundo habla de ello?][mcp-blog-post]
- [Servidor MCP de Playwright de Microsoft][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
