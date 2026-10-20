---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto, pero los cambios más grandes requieren un proceso más sólido. GitHub Copilot CLI está diseñado para trabajar con el flujo de tu organización y ayudarte a crear lo adecuado de la forma correcta. Esta es la primera de varias lecciones en las que seguirás un proceso de desarrollo habitual basado en agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como esperas y, finalmente, la integrarás correctamente en el proyecto.

> [!NOTE]
> Continuarás el flujo de trabajo de la funcionalidad con la misma conversación y rama. Normalmente usarías ramas o pull requests distintas para los diferentes tipos de archivos, pero simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot a partir de una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la funcionalidad con el modo Autopilot.
- revisarás el código.
- validarás la funcionalidad manualmente en un navegador reenviado.

A medida que avances con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás la validación con MCP, crearás un agente de control de calidad y abrirás la pull request de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar algunos detalles, como la combinación de categorías. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los fundamentos; de hecho, los hace aún más importantes. La mayoría de los desarrolladores sigue un proceso parecido a este:

1. Empezar con una incidencia que describa lo que hay que hacer.
2. Crear un plan de lo que se debe construir.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una pull request (PR).
7. Cuando se haya revisado el código y se haya completado correctamente la integración continua, fusionar los cambios.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos siguen una estructura parecida a la anterior.

Al seguir este enfoque estándar, te aseguras de que el código generado por IA cumple los requisitos y pasa por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar de modo:

- **Interactive**: tú y el agente colaboráis. El agente propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma, escribe código, ejecuta pruebas y repite el proceso hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Empezar con la incidencia

Antes de empezar a trabajar en el filtrado, vuelve a tu codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve a tu codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que la pull request de la valoración por estrellas se ha fusionado.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y, después, crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora empieza por `#7` (o un número similar). El carácter `#` permite incluir en el contexto una incidencia o pull request (PR) de GitHub.

## Planificar la funcionalidad de filtrado

La planificación permite definir el enfoque que se seguirá para implementar una funcionalidad o realizar una tarea antes de asignársela a Copilot. Siempre conviene dedicar un tiempo a planificar las tareas complejas. Cambiemos al modo Plan y pidamos a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Comprueba que el indicador situado debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que has añadido en el paso anterior, introduce este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empezará a preparar el plan. Primero explorará el proyecto y, después, determinará el enfoque más adecuado.

3. Durante el proceso, Copilot puede hacerte preguntas sobre cómo debería funcionar el filtrado. Respóndelas según tus preferencias; no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que revise cualquier parte del plan que no refleje tus decisiones.

## Aprobar Autopilot

Una vez escrito y revisado el plan, es el momento de implementarlo. Deja que Copilot trabaje con Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que lo ha resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o una opción con un nombre parecido en la versión instalada.
2. Comprueba que el indicador situado debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente. Revísalo primero. Si Copilot indica que faltan dependencias o que hay un conflicto de puertos, resuelve el problema de configuración antes de dar por completadas las comprobaciones.

## Revisar y verificar la implementación

Una vez generado, hay que revisar el código antes de fusionarlo, igual que cualquier otro cambio. Revisaremos el código y ejecutaremos el sitio para comprobar que todo funciona correctamente.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Comprueba que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Introduce `/diff` y revisa la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y con las decisiones que tomaste durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva los fallos que encuentre.

## Explorar la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio mediante el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, el filtrado por editorial y las combinaciones acordadas en el plan.
4. Confirma que las funciones de restablecimiento y los resultados vacíos se comportan según la incidencia y tus decisiones.
5. Vuelve al codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- has iniciado una conversación de Copilot a partir de una incidencia de GitHub.
- has definido los requisitos en el modo Plan.
- has implementado la funcionalidad con el modo Autopilot.
- has revisado el código.
- has validado la funcionalidad manualmente en un navegador reenviado.

A continuación, profundizarás en cómo se genera el código y cómo garantizar que siga las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
