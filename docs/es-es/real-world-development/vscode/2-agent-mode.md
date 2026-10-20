---
title: "Ejercicio 2 - Añadir funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Hasta las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. Normalmente, el flujo de trabajo de un desarrollador consiste en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, actualizarlo y repetir el proceso.

El modo de agente de Copilot ayuda a coordinar este trabajo.

El modo de agente está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentre. Veamos cómo usarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE y permite ciclos rápidos de comentarios e interacción. El agente en la nube funciona de forma asíncrona como un compañero del equipo y se comunica contigo mediante incidencias y pull requests.

En este ejercicio aprenderás a:

- usar el modo de agente de Copilot para explorar el proyecto, localizar los archivos pertinentes y coordinar los cambios.
- implementar funcionalidades en la interfaz y en la capa de datos con el modo de agente de GitHub Copilot.
- revisar los cambios y las pruebas generados antes de fusionarlos en el código base.

## Escenario

A medida que crece la lista de juegos, quieres que los usuarios puedan filtrarlos por categoría y editorial. En el ejercicio anterior ya añadiste un helper de editoriales; ahora completarás la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva para juegos de mesa con una temática de desarrollo. Permite mostrar una lista de juegos y consultar sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz de cliente. Los helpers reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con un pequeño `<script>` de Astro y las API estándar del DOM.

### Iniciar el sitio web

Para facilitar el inicio del sitio, hay una tarea `npm run dev` que ejecuta el servidor de desarrollo de Astro. Sigue estos pasos para iniciarlo en GitHub Codespaces:

1. Vuelve a tu codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal nuevo en Codespaces pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, verás un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pregunta si quieres abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace a la URL localhost en el terminal, se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Este túnel privado permite que el servidor web se ejecute en tu codespace.

### Explorar el sitio web

Cuando el sitio esté en ejecución, explora sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de continuar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea pertinente.

La implementación inicial del sitio funciona, pero queremos añadirle nuevas capacidades. Empecemos revisando el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esas tareas.

1. En el desplegable de agentes de la vista Chat, selecciona **Agent**. Este agente planifica e implementa cambios en varios archivos de forma autónoma, ejecuta comandos del terminal e invoca herramientas.

   ![Captura del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. En la lista de modelos disponibles, selecciona **Claude Sonnet 4.5**.

> [!CAUTION]
> Los autores del taller no prefieren un modelo frente a otro. Al preparar el taller usamos Claude Sonnet 4.5 y lo indicamos para que las sugerencias fueran relativamente coherentes y la experiencia, más uniforme. Sin embargo, como los modelos de lenguaje extensos son probabilísticos, las sugerencias pueden diferir de los ejemplos del taller. Es normal.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje extensos, Copilot puede usar un comando MCP distinto, pero debería poder completar la tarea igualmente.

3. Pide a Copilot que muestre el backlog con este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que muestra todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado las tareas según cuáles considera más útiles para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funcionalidades:
   - Una descripción de la arquitectura.
   - Principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - Enlaces a archivos de instrucciones para tipos de archivo específicos, incluidos:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Esto facilita la reutilización, divide conjuntos complejos en partes más manejables y permite incluir ejemplos y plantillas.

## Implementar la funcionalidad de filtrado

Para completar el filtrado hay que realizar al menos tres cambios en la aplicación:

- Añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- Añadir o actualizar las pruebas del comportamiento del filtrado.
- Actualizar la página de la lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y pasar antes de fusionar los cambios en el código base. El modo de agente de Copilot puede realizar estas tareas.

1. Continúa con la conversación actual de Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de la selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. En el desplegable de agentes de la vista Chat, confirma que sigue seleccionado **Agent**.

   ![Captura del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que sigue seleccionado **Claude Sonnet 4.5** como modelo.
5. Pide a Copilot que implemente la funcionalidad basándose en la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad, incluidos los helpers de la capa de datos, la interfaz y las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorará el proyecto, modificará archivos y ejecutará pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código base. Mientras trabaja, pueden aparecer los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás uno de estos botones para cada cambio; no hace falta seleccionarlos mientras el trabajo sigue en curso.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo de Copilot Chat que pide confirmar la ejecución de pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que el proceso se detenga en algún momento o que algunas pruebas fallen. No pasa nada: Copilot alterna entre la generación de código y la ejecución de pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión completa de Copilot Agent Mode en Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código que genere Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

En este ejercicio has explorado cómo usar el modo de agente de GitHub Copilot para añadir funciones al sitio web de Tailspin Toys. Has aprendido a:

- implementar funcionalidades en la interfaz y en la capa de datos con el modo de agente de GitHub Copilot.
- usar el modo de agente para explorar el proyecto, localizar los archivos pertinentes y coordinar los cambios.
- revisar los cambios y las pruebas generados antes de fusionarlos en el código base.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una pull request.

### Ejercicio adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Usa lo aprendido en este ejercicio y pide a Copilot que añada esa funcionalidad al sitio. Al preparar el prompt:

- sigue las prácticas recomendadas y usa los archivos de instrucciones existentes.
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
- incluye en el prompt las directrices necesarias para que Copilot implemente la solución que quieres.
- puede que tengas que iterar con GitHub Copilot, pedir cambios y aportar contexto. Es normal trabajar así con Copilot.

## Recursos

- [Copilot: modos Ask, Edit y Agent; qué hacen y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
