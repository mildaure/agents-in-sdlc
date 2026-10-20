---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene que Copilot tenga acceso a esas directrices. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que ha utilizado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones con ámbito de ruta.
- actualizarás el archivo de instrucciones para garantizar que se siguen los estándares de programación.
- observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys cuenta con directrices y requisitos para las prácticas de desarrollo. Entre ellos se incluyen:

- Los comentarios deben explicar la intención y las decisiones que no sean obvias, no limitarse a repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos con comentarios TSDoc/JSDoc, incluido el argumento `db` inyectable cuando exista.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Se deben respetar las directrices existentes de formato y lint.

Con los archivos de instrucciones te asegurarás de que Copilot disponga de la información adecuada para realizar las tareas según las prácticas indicadas.

## Archivos de instrucciones

Las instrucciones personalizadas te permiten proporcionar contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta función te ayuda a obtener sugerencias y fragmentos de código más pertinentes. Puedes especificar las convenciones de programación y bibliotecas que prefieres, e incluso los tipos de comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y aportar contexto para cada tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información del proyecto pertinente para la mayoría de las solicitudes de chat o CLI.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para lenguajes, tipos de archivo o tareas concretas.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explorar los archivos de instrucciones personalizadas de este proyecto

El proyecto de inicio ya incluye varios archivos de instrucciones. Explóralos antes de hacer cambios para ver qué orientación proporcionan.

1. Vuelve a tu codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la descripción del proyecto y en las directrices de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros casos.
5. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior establece el patrón glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y fíjate en los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos extensos en archivos más pequeños y reutilizables, y proporcionar ejemplos que Copilot pueda seguir.

## Actualizar los archivos de instrucciones para reflejar las directrices del equipo

Los archivos existentes son un buen comienzo, pero falta algo. Modificarás el archivo `copilot-instructions.md` principal para que el código TypeScript generado incluya comentarios TSDoc.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade esta viñeta al final de la sección:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Usar las directrices actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar el archivo, sal de la conversación de filtrado y reanúdala para que Copilot use las nuevas directrices sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que la rama `game-filters-cli` está activa.
3. Pide a Copilot que aplique las nuevas directrices:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Introduce `/diff` y revisa los archivos TypeScript modificados. Comprueba que los comentarios TSDoc describen el código correctamente.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI obtiene el contexto de los archivos de instrucciones y has aplicado un nuevo estándar a la funcionalidad. En concreto:

- has explorado cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones con ámbito de ruta.
- has actualizado el archivo de instrucciones para garantizar que se siguen los estándares de programación.
- has observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad de comprobaciones de calidad reutilizables][next-lesson] para garantizar que el lint y las pruebas se ejecutan de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
