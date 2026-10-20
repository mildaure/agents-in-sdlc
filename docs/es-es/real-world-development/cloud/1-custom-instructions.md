---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente en la nube de Copilot →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta —o hay información de fondo que Copilot debería conocer— conviene asegurarse de que ese contexto esté disponible. Los [archivos de instrucciones][instruction-files] proporcionan esas directrices para que Copilot entienda no solo *qué* debe hacer, sino también *cómo* hacerlo.

En este ejercicio:

- explorarás cómo llega a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones con ámbito de ruta.
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no ejecutarás un prompt antes y después del cambio. El agente en la nube de Copilot trabaja de forma asíncrona en incidencias de GitHub, por lo que es más difícil mostrar su efecto en tiempo real. Verás la influencia del archivo de instrucciones más adelante, al revisar las pull requests que genere el agente en la nube.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys cuenta con directrices y requisitos para las prácticas de desarrollo. Entre ellos se incluyen:

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
> Al trabajar en el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para las finalizaciones de código ni las sugerencias de siguiente edición.
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
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas **GitHub Actions Workflows**. Estas instrucciones se aplican a todas las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros casos.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior establece un patrón glob, relativo a la raíz del repositorio, que determina a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript, por ejemplo, uno que cumpla el patrón `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos extensos en archivos más pequeños y reutilizables, y proporcionar ejemplos que Copilot pueda seguir al generar código. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentarios de archivo.

## Añadir un nuevo estándar del repositorio

Este es el único cambio que harás en este ejercicio: añadir una regla para todo el proyecto que indique que la documentación debe incluirse en el código mediante comentarios TSDoc y un encabezado de comentario a nivel de archivo. El agente en la nube tendrá en cuenta esta regla en las incidencias que le asignes más adelante.

Antes de editar el archivo, crea una rama de trabajo. El agente en la nube leerá las instrucciones de la rama a la que se dirija la incidencia; aun así, confirmarás los cambios de forma ordenada:

1. Desde el terminal del codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se indicó antes, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Añadamos los estándares de programación del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que documenta los estándares de programación, pero aún no incluye una regla sobre la documentación en el código; por eso el helper generado no tenía comentarios de documentación.
3. Añade estas líneas de Markdown justo debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, puedes crear instrucciones en el ámbito del repositorio (`.github/copilot-instructions.md`) para ofrecer directrices generales, o archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla de comentarios de documentación que acabas de añadir.

## Confirmar, enviar y fusionar la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se dirige la incidencia. En el siguiente ejercicio, cuando asignes una incidencia a Copilot, el agente creará una rama a partir de `main`. Por eso, los cambios en las instrucciones deben llegar a `main` para que el agente pueda usarlos.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una pull request desde `custom-instructions-cloud` hacia `main` y fusiónala. El agente en la nube leerá estas instrucciones al trabajar en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir el paso de crear una rama y confirmar el cambio en `main`. La rama permite practicar un proceso habitual en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones de este proyecto y has añadido un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`. Verás cómo se aplica este estándar a las pull requests que genere el agente en la nube durante el resto del recorrido.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
