---
title: "Lección 6 - Validar funcionalidades con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar en un navegador la funcionalidad de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos destacado, programar no consiste únicamente en escribir código. También hay que trabajar con datos y servicios externos, y proporcionar a Copilot automatizaciones adicionales. Para ello sirven los servidores MCP: amplían las herramientas y los servicios disponibles en Copilot CLI.

En esta lección:

- comprenderás qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- añadirás el servidor MCP de Playwright si aún no está disponible.
- pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz hay que interactuar con ella. Quieres que Copilot pueda usar el sitio web como lo haría un usuario y automatizar mejor la comprobación de los cambios, para tener más confianza en que funcionan como se espera.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos. Mediante MCP, los agentes pueden acceder a información actualizada y realizar acciones en tu nombre.

Se accede a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. Cada servidor MCP proporciona un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos son:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a API para gestionar repositorios, incidencias y pull requests de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: proporciona funciones de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y valora las implicaciones de seguridad.

## Añadir el servidor MCP de Playwright

Si el servidor MCP de Playwright aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría una persona.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos de configuración y continúa con la sección siguiente.

1. Vuelve a tu codespace.
2. En Copilot CLI, introduce el siguiente comando para abrir el cuadro de diálogo de servidores MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** pulsando <kbd>Enter</kbd> y, después, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo de MCP.

## Pedir a Copilot que explore la funcionalidad con Playwright

Antes has confirmado manualmente que la funcionalidad se comporta como se esperaba. Ahora pedirás al servidor MCP de Playwright disponible en Copilot que haga lo mismo.

1. Usa este prompt para pedir a Copilot que valide la funcionalidad con el servidor MCP de Playwright:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Normalmente no hace falta indicar expresamente que use el servidor MCP, porque Copilot suele detectarlo. Aun así, especificar la herramienta puede orientar mejor la tarea y hacer que los resultados sean más coherentes.

2. Observa los pasos que Copilot realiza en el navegador para confirmar que la funcionalidad funciona.
3. Lee el informe y comprueba que todo se comporta como esperas.

Copilot iniciará el servidor, usará Playwright para interactuar con el sitio, detendrá el servidor y te entregará un informe.

## Resumen y siguientes pasos

Has usado el servidor MCP de Playwright para explorar la funcionalidad con Copilot CLI en un navegador real. En resumen:

- has aprendido qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- has añadido el servidor MCP de Playwright si aún no estaba disponible.
- has pedido al agente que controle un navegador y explore la funcionalidad de filtrado.

A continuación, [crearás un agente personalizado de control de calidad][next-lesson] que combine la habilidad y las herramientas del navegador en un rol especializado.

## Recursos

- [¿Qué demonios es MCP y por qué todo el mundo habla de ello?][mcp-blog-post]
- [Servidor MCP de Playwright de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
