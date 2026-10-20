---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta —o hay información de fondo que Copilot debería conocer— conviene asegurarse de que ese contexto esté disponible. Hay varias maneras de proporcionar contexto específico a Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que ofrecen directrices para generar código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones con ámbito de ruta.
- generarás el primer bloque de datos para el filtrado —un helper de editoriales— con las instrucciones *actuales*.
- añadirás un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`.
- repetirás el mismo prompt y observarás cómo el código generado aplica el nuevo estándar.
- confirmarás la actualización de instrucciones y el bloque de filtrado en `main` para que Copilot pueda usar las nuevas directrices en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede apartarse de algunos de los estándares que establezcas. Copilot no es determinista. El objetivo es observar la *tendencia* en el comportamiento después de actualizar las instrucciones, no conseguir una salida idéntica carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys cuenta con un conjunto de directrices y requisitos para las prácticas de desarrollo. Entre ellos se incluyen:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un aspecto moderno.
- Se debe documentar el código mediante comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que explique su propósito.

Con los archivos de instrucciones te asegurarás de que Copilot disponga de la información adecuada para realizar las tareas según las prácticas indicadas.

### Instrucciones personalizadas

Las instrucciones personalizadas te permiten proporcionar contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Puedes especificar las convenciones de programación y bibliotecas que prefieres, e incluso los tipos de comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y aportar contexto para cada tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información del proyecto pertinente para la mayoría de las solicitudes de chat o CLI: por ejemplo, la pila tecnológica, una descripción de lo que se está construyendo, buenas prácticas y otras directrices generales.
- Los archivos `.github/instructions/*.instructions.md` se pueden crear para tareas o tipos concretos de archivos. Sirven para proporcionar directrices para lenguajes como TypeScript o Astro, o para tareas como crear un componente de interfaz o un nuevo conjunto de pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para las finalizaciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con el frontmatter `applyTo` al generar código.
>
> Además, Copilot [admite archivos de instrucciones que usan otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Explicar detalladamente cómo crear archivos de instrucciones queda fuera del alcance del taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén las instrucciones de `copilot-instructions.md` centradas en directrices del proyecto, como una descripción de lo que se está construyendo, la estructura del proyecto y los estándares generales de programación.
- Usa archivos `*.instructions.md` para proporcionar instrucciones específicas según el tipo de archivo —por ejemplo, pruebas unitarias, componentes Astro o la capa de datos— o según la tarea.
- Usa lenguaje natural y directrices claras. Incluye ejemplos de cómo debería ser el código y de cómo no debería serlo.

No hay una única forma de crear archivos de instrucciones, igual que no hay una única forma de usar la IA. La experimentación te permitirá descubrir qué funciona mejor en tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían contar con una colección sólida de archivos de instrucciones. Al explorar los de este proyecto, puede que veas que hay archivos para muchos tipos de tareas, incluidas las [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada interfaz lo ofrece de una manera distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente lo explicará cuando sea relevante.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones que incluye este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas **GitHub Actions Workflows**. Se aplican a todas las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros casos.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior establece un patrón glob, relativo a la raíz del repositorio, que determina a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript, por ejemplo, uno que cumpla el patrón `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos extensos en archivos más pequeños y reutilizables, y proporcionar ejemplos que Copilot pueda seguir al generar código. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentarios de archivo.

## Crear una rama para los cambios

Sigamos algunas prácticas recomendadas y creemos una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de continuar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea pertinente.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con las instrucciones actuales. Más adelante, actualizarás el archivo y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot use solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para que Copilot sepa dónde debe estar el helper.
3. En el desplegable de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar cambios en los archivos.

   ![Captura del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará actualizaciones de código, normalmente en el archivo del helper y sus pruebas.
6. Observa que la función tipada propuesta recibe un cliente `db` como primer argumento y devuelve un array tipado de editoriales. Esto procede de las convenciones de la capa de datos en `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** comentarios TSDoc y un encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot no es determinista, puede que añada comentarios de documentación aunque no se lo pidas. No pasa nada: lo importante es observar cómo mejora la *coherencia* después de actualizar las instrucciones.

## Añadir un nuevo estándar del repositorio

Como se indicó antes, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Documentemos los estándares de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que documenta los estándares de programación, pero aún no incluye una regla sobre la documentación en el código; por eso el helper generado no tenía comentarios.
3. Añade estas líneas de Markdown justo debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, puedes crear instrucciones en el ámbito del repositorio (`.github/copilot-instructions.md`) para ofrecer directrices generales, o archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla de comentarios de documentación que acabas de añadir.

## Volver a ejecutar el prompt y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto de la conversación anterior.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía **el mismo prompt** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Helpers de acceso a datos de editoriales para la plataforma de financiación colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su id y nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Promesa que se resuelve en un array de objetos de editoriales.
    */
   ```

Has orientado a Copilot para que siga un nuevo estándar del proyecto y lo aplique a código real sobre el que trabajarás en el próximo ejercicio.

## Confirmar las instrucciones y enviar la rama

Los archivos de instrucciones son recursos del repositorio como los demás, así que se gestionan con el mismo proceso de control de versiones.

1. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que están presentes las instrucciones actualizadas y los cambios del helper:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y todos los archivos que Copilot haya cambiado para crear el helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y fusionar una pull request

Ya has enviado la rama; ahora crearás una pull request asociada a la incidencia de estándares de documentación del backlog. Copilot puede buscar la incidencia, crear una PR que la cierre y fusionarla usando las herramientas de GitHub conectadas al proyecto.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actuará en GitHub por ti. La plantilla del proyecto ya configura las herramientas de GitHub, así que no tienes que hacer nada más. La primera vez que Copilot use una, puede que VS Code te pida iniciar sesión en GitHub. Sigue las indicaciones para autorizarlo. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y cree una PR desde la rama actual. Indica que la PR incluye tanto las instrucciones como el helper inicial de editoriales:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Si Copilot te pide permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** cuando corresponda.
6. Cuando se cree la PR, pide a Copilot que la fusione y vuelva a `main` para obtener el código actualizado:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Si Copilot te pide permiso para realizar acciones de GitHub o ejecutar comandos del shell, revisa el comando y selecciona **Allow** cuando corresponda.

Ya has creado y fusionado una pull request con ayuda de GitHub Copilot.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*.
- añadir un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`.
- volver a enviar el mismo prompt y observar cómo el código propuesto aplica el nuevo estándar.
- confirmar la actualización de instrucciones y el helper inicial de editoriales en `main` para que el siguiente ejercicio pueda usarlos.

A continuación, aplicarás esas instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una funcionalidad que abarca varias partes del código base.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
