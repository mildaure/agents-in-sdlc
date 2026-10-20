---
slug: es-es/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Crea, verifica y entrega dos cambios en Tailspin Toys mientras aprendes los modos, las personalizaciones, las herramientas MCP y la automatización de pull requests de Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** incorpora GitHub Copilot a tu terminal como asistente de programación con agentes. Explora bases de código, genera código, ejecuta comandos y se conecta a herramientas externas, todo desde la línea de comandos, para que puedas mantener el ritmo sin cambiar a un editor gráfico.

El taller sigue un flujo de trabajo continuo de Tailspin Toys:

1. Prepara el proyecto en GitHub Codespaces, instala Copilot CLI y familiarízate con el entorno.
2. Haz un cambio acotado para mostrar las valoraciones por estrellas, revísalo en el navegador y fusiona manualmente tu primera pull request (PR).
3. Empieza por la incidencia de filtrado, define el enfoque en modo Plan, implementa la funcionalidad en modo Autopilot y revísala en modo Interactive.
4. Actualiza las instrucciones del repositorio y aplícalas al trabajo de filtrado.
5. Personaliza la habilidad existente `quality-checks` y úsala para ejecutar las comprobaciones del proyecto.
6. Añade el servidor Model Context Protocol (MCP) de Playwright y úsalo para explorar el filtrado en el navegador.
7. Crea un agente personalizado de control de calidad (QA) y úsalo para revisar los requisitos, la cobertura y las evidencias de verificación.
8. Revisa todos los cambios de filtrado y usa Agent Merge para fusionar la PR.
9. Explora comandos de barra útiles para gestionar el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

Para mantener el taller centrado, crearás dos PR: una para las valoraciones por estrellas y otra para el filtrado con las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad y las pruebas. Empieza cada una desde `main` actualizado. El flujo de filtrado y calidad comparte una conversación y una rama para que puedas aprovechar el trabajo realizado mientras exploras cada herramienta.

## Lecciones

| Lección | Tema | Descripción |
| ------ | ----- | ----------- |
| [0. Requisitos previos][ex0] | Configuración | Crea tu repositorio y codespace |
| [1. Instalar GitHub Copilot CLI][ex1] | Instalación | Instala y autentica Copilot CLI y familiarízate con el entorno |
| [2. Añadir valoraciones por estrellas: una mejora rápida][ex2] | Primer cambio | Muestra las valoraciones existentes y el valor alternativo cuando son nulas; después, fusiona tu primera PR |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la funcionalidad desde su incidencia, impleméntala con Autopilot y revísala en modo Interactive |
| [4. Guiar a Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones y aplícalas al filtrado |
| [5. Personalizar y usar la habilidad quality-checks][ex5] | Comprobaciones repetibles | Explora la habilidad existente, cambia el formato del informe y ejecútala |
| [6. Validar la funcionalidad con Playwright MCP][ex6] | Observación en el navegador | Configura MCP en la CLI y examina el comportamiento del filtrado |
| [7. Crear y usar un agente de control de calidad][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especializado y reúne las evidencias finales de verificación |
| [8. Crear y fusionar la PR de la funcionalidad][ex8] | Revisión y fusión | Revisa todos los cambios, crea la PR y usa Agent Merge |
| [9. Comandos de barra en GitHub Copilot CLI][ex9] | Funciones de CLI | Explora el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube |
| [10. Resumen y próximos pasos][ex10] | Resumen | Repasa el flujo de trabajo, las personalizaciones reutilizables y otros recursos |
| [Opcional: incorporar Foundry][foundry] | Agentes hospedados | Prepara un modelo, despliega un agente basado en el catálogo y conéctalo al sitio web |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo de **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Permiso para crear un repositorio y un codespace
- [ ] Conocimientos básicos sobre el uso de terminales o líneas de comandos

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis a través de [GitHub Education][student-plan]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que utiliza este taller.

> [!NOTE]
> Si usas Copilot Business o Copilot Enterprise, confirma que el administrador haya habilitado Copilot CLI.

## Empezar

**[Empieza por los requisitos previos →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
