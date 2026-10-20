---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, prepara todo lo necesario. Crearás tu propia copia del repositorio Tailspin Toys, pondrás en marcha un [codespace][codespaces] y confirmarás que GitHub Copilot Chat funciona en el editor.

## Configurar el repositorio del laboratorio

Para crear una copia del repositorio donde trabajarás, crearás una instancia a partir de la [plantilla][template-repository]. La nueva instancia incluirá todos los archivos necesarios para el laboratorio y la usarás durante los ejercicios.

1. En una ventana nueva del navegador, ve a la [plantilla de Tailspin Toys][tailspin-template].
2. Crea una copia propia del repositorio seleccionando el botón **Use this template** en la página del repositorio del laboratorio. Después, selecciona **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. De lo contrario, puedes crear el nuevo repositorio en una organización donde tengas acceso a GitHub Copilot.

    ![Introduce la configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**), ya que la necesitarás más adelante en el laboratorio.

> [!NOTE]
> **El backlog está listo**
>
> Al crear el repositorio a partir de la plantilla, se genera automáticamente un backlog de incidencias de GitHub. Trabajarás con ellas durante el taller; no tienes que crear ninguna.

## Crear un codespace

A continuación, usarás un codespace para completar los ejercicios del laboratorio.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que te permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un IDE completo con compatibilidad para varios lenguajes de programación, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

    ![Selecciona el botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un codespace.

    ![Crea un codespace](../../../_images/ex0-create-codespace.png)

La creación del codespace tardará varios minutos, aunque seguirá siendo mucho más rápida que instalar manualmente todos los servicios. Mientras esperas, puedes explorar otras funciones de GitHub Copilot, que veremos a continuación.

> [!CAUTION]
> Volverás al codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está preparado para ejecutarse en un codespace o en un [contenedor de desarrollo local][dev-containers]. Ambos entornos incluyen los requisitos previos necesarios para que el taller funcione sin problemas. Si prefieres trabajar localmente, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite. VS Code creará el mismo contenedor de desarrollo que usa el codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usar GitHub Copilot Chat y el modo de agente

Para usar el modo de agente de GitHub Copilot Chat, debes tener instalada en el IDE la extensión GitHub Copilot Chat. Si usas GitHub Codespaces, ya debería estar instalada.

> [!TIP]
> Si no tienes instalada la extensión GitHub Copilot Chat, puedes [instalarla desde Visual Studio Code Marketplace][copilot-chat-extension]. También puedes abrir la vista Extensions de Visual Studio Code, buscar **GitHub Copilot Chat** y seleccionar **Install**.

Después de instalar la extensión, puede que tengas que autenticarte con tu cuenta de GitHub para habilitarla.

1. Vuelve a tu codespace.
2. Si Copilot Chat no aparece a la derecha del editor, selecciona el icono **Copilot Chat** en la parte superior del codespace.
3. Escribe un mensaje como «Hola, mundo» en la ventana de Copilot Chat y pulsa <kbd>Enter</kbd>. Esto debería activar Copilot Chat.
4. Si aún no te has autenticado, se te pedirá que inicies sesión en tu cuenta de GitHub. Sigue las instrucciones para autenticarte.

    ![Ejemplo de solicitud de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Después de autenticarte, debería aparecer la ventana de Copilot Chat.

## Resumen

Enhorabuena: has creado una copia del repositorio del laboratorio. También has iniciado la creación del codespace que usarás cuando empieces a programar.

## Siguiente paso

Empecemos a trabajar con Copilot. Continúa con el [ejercicio 1: Instrucciones personalizadas][next-lesson], donde enseñarás a Copilot las convenciones del proyecto.

## Recursos

- [Introducción a GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Guía de inicio rápido de Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
