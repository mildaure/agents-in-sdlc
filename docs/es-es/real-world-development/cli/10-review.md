---
title: "Lección 10 - Resumen y próximos pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos pull requests, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI a lo largo de un flujo de trabajo continuo con Tailspin Toys. Has:

- preparado un codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado del backlog.
- añadido valoraciones por estrellas, revisado el resultado en un navegador reenviado y fusionado manualmente tu primera pull request (PR).
- empezado por la incidencia de filtrado, definido el enfoque en el modo Plan, creado la funcionalidad con Autopilot y revisado el resultado en el modo Interactive.
- orientado al agente con instrucciones personalizadas, personalizado la habilidad existente `quality-checks` y la usado para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadido el servidor Model Context Protocol (MCP) de Playwright y lo has usado para explorar el filtrado en un navegador real.
- creado y seleccionado un agente personalizado de control de calidad para evaluar los requisitos, la cobertura, los resultados de las habilidades y las pruebas en el navegador.
- revisado todos los cambios de filtrado y autorizado Agent Merge para la PR de filtrado.
- explorado comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar tareas opcionalmente al agente en la nube.

## Qué has publicado

El taller incluye dos hitos de pull request, cada uno en su propia rama creada a partir de `main` actualizado:

1. **Valoraciones por estrellas:** muestra el `starRating` existente y un estado explícito para los juegos sin valoración en sus tarjetas.
2. **Filtrado y flujo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de control de calidad e incluye las pruebas asociadas.

Desde la planificación del filtrado hasta la apertura de su pull request, has usado la misma conversación y rama. Combinamos ese trabajo en una única PR para simplificar el taller.

## Diferentes tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, revisándolo tú mismo en el navegador y usando Copilot para explorar el navegador mediante MCP. La habilidad `quality-checks` ha ejecutado las pruebas unitarias, el lint y las comprobaciones de tipos, y ha presentado los resultados en el nuevo formato. El agente de control de calidad ha combinado esos resultados con una revisión de los requisitos y la cobertura de pruebas antes de la PR.

Las pruebas nuevas deben cubrir carencias reales; una ejecución de control de calidad que no necesite pruebas adicionales puede ser correcta. Revisa el código y las evidencias antes de autorizar la fusión, y actualiza las evidencias afectadas cuando haya cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas determinan el trabajo de Copilot. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil de control de calidad y configurado un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados reales de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para definir el enfoque antes de empezar, **Interactive** para participar en los cambios acotados y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para las ediciones rutinarias y uno más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir claramente *qué* quieres construir, *por qué* y *cómo* cambia significativamente el resultado.

## Más por explorar

Ya has cubierto el flujo de trabajo principal. También puedes probar estas funciones de CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para razonar sobre un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Próximos pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola. Úsala con código de producción, proyectos personales o esa pequeña aplicación que llevas años pensando en crear. Comparte lo que aprendas con el equipo y aprende también de sus experiencias. Y, como siempre, consulta la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar pull requests con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
