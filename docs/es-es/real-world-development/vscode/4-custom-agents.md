---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o ámbitos concretos del flujo de desarrollo. Al definirlos mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, prácticas recomendadas, patrones de programación y conocimiento del dominio para ayudarle a realizar ciertos tipos de trabajo con mayor eficacia. Los equipos pueden codificar su experiencia en agentes reutilizables: por ejemplo, uno de accesibilidad que aplique las directrices de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo incluye un frontmatter YAML con al menos los campos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la especialización y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se solapan en algunos aspectos. Ambos se definen principalmente mediante archivos Markdown y explican a una IA cómo realizar tareas. La diferencia más clara es que el **agente personalizado** es quien trabaja y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades e incluso otros agentes como parte de su trabajo. En este laboratorio, el agente personalizado de accesibilidad revisa y actualiza el sitio según las directrices de accesibilidad. Podría, por ejemplo, invocar habilidades como una habilidad para gestionar pull requests o una que ejecute y gestione pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba e itera para averiguar qué funciona mejor en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que su plataforma de financiación colectiva sea accesible para todo el mundo, independientemente de sus capacidades visuales o preferencias. Algunos comentarios recientes indican que a ciertas personas les cuesta leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es fundamental, así que quieres implementar esta mejora cuanto antes. Usarás un agente personalizado para generar la funcionalidad.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- usarás el agente de accesibilidad en Copilot Chat para implementar el modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisemos su contenido para entender cómo orientará a Copilot.

Vuelve a tu codespace, abre un terminal y crea una rama nueva a partir de `main` para el trabajo de accesibilidad. Mantendrás separada la pull request de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que destacan:
   - Las responsabilidades principales al generar código para un sitio web accesible.
   - Las prácticas recomendadas de accesibilidad.
   - Ejemplos de código HTML, CSS y JavaScript.
   - Una lista de errores y problemas habituales.

## Usar el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el desplegable de agentes situado en la parte inferior de Copilot Chat. Puedes seleccionar un agente personalizado para aplicar a la sesión de chat sus instrucciones y herramientas.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de continuar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea pertinente.

1. En el desplegable de agentes de la vista Chat, selecciona **Agent** si aún no está seleccionado.

   ![Captura del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el desplegable de agentes situado en la parte inferior de la vista Chat. Muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía este prompt al agente de accesibilidad:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará el código base, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y las diferencias para que puedas revisarla.
6. A medida que se apliquen cambios, el indicador **Files changed** se actualizará para mostrar los archivos modificados por el agente.

> [!NOTE]
> El proceso puede tardar unos minutos. Copilot realizará cambios en el repositorio: editará componentes Astro, CSS y las pruebas relacionadas, si las hay.

En el siguiente ejercicio revisarás y orientarás el trabajo mientras la sesión siga en curso.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados en tareas y ámbitos concretos. Permiten codificar la experiencia y los estándares del equipo en agentes reutilizables que orientan a Copilot para realizar ciertos tipos de trabajo con mayor eficacia.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo usar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson], revisarás los cambios a medida que se realicen y añadirás un control de modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados de awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
