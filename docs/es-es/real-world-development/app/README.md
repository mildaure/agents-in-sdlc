---
slug: es-es/real-world-development/app
title: "Aplicación GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-09-17
---

La **[aplicación GitHub Copilot](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** es una aplicación de escritorio basada en Copilot CLI que integra el desarrollo con agentes en un espacio de trabajo único y dedicado. Añade sesiones de agentes en paralelo, modos de sesión intercambiables, lienzos compartidos y gestión nativa de incidencias y pull requests de GitHub, incluido **Agent Merge**, que gestiona las actualizaciones de la rama base, los comentarios de revisión, las correcciones de integración continua (CI) y la fusión de una pull request.

El taller sigue un único flujo continuo de Tailspin Toys:

1. Prepara el proyecto, instala la aplicación, conecta el repositorio y explora el espacio de trabajo y el backlog inicial.
2. Haz un cambio acotado para mostrar las valoraciones por estrellas, revísalo en el navegador y fusiona manualmente tu primera pull request (PR).
3. Empieza por la incidencia de filtrado, define el enfoque en modo **Plan**, implementa la funcionalidad en modo **Autopilot** y revísala en modo **Interactive**.
4. Actualiza las instrucciones del repositorio y aplícalas al trabajo de filtrado.
5. Personaliza la habilidad `quality-checks` existente y úsala para ejecutar las comprobaciones del proyecto.
6. Añade el servidor Model Context Protocol (MCP) de Playwright y úsalo para explorar el filtrado en el navegador.
7. Crea un agente personalizado de control de calidad (QA) y úsalo para revisar los requisitos, la cobertura y las pruebas de verificación.
8. Revisa el cambio completo de filtrado y utiliza Agent Merge para la segunda PR.
9. Usa el lienzo existente Database Explorer y, después, crea y prueba un lienzo de clasificación conectado al repositorio.

Para mantener el taller centrado, crearás dos PR: una para las valoraciones por estrellas y otra para el filtrado con las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad y las pruebas. Empieza cada una desde `main` actualizado. El flujo de filtrado y calidad comparte una sesión, un worktree y una rama para que puedas aprovechar el trabajo realizado mientras exploras cada herramienta. El ejercicio final del lienzo continúa en su propia sesión para que puedas centrarte en crear y probar la interfaz compartida en lugar de repetir el flujo de PR.

## Lecciones

| Lección | Tema | Descripción |
|--------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Instala Node.js y crea tu copia del proyecto Tailspin Toys |
| [1. Instalar la aplicación Copilot][ex1] | Configuración | Instala la aplicación, conecta el proyecto y familiarízate con el espacio de trabajo |
| [2. Añadir valoraciones por estrellas: una mejora rápida][ex2] | Primer cambio | Muestra las valoraciones existentes y el texto alternativo cuando son nulas; después, fusiona la PR 1 |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la funcionalidad desde su incidencia, impleméntala con Autopilot y revísala en modo Interactive |
| [4. Guiar a Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones y aplícalas al filtrado |
| [5. Personalizar y usar una habilidad quality-checks][ex5] | Comprobaciones repetibles | Explora la habilidad existente, cambia el formato del informe y ejecútala |
| [6. Validar la funcionalidad con Playwright MCP][ex6] | Observación en el navegador | Configura MCP desde Customize y examina el comportamiento del filtrado |
| [7. Crear y usar un agente de control de calidad][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especializado y reúne las evidencias finales de verificación |
| [8. Crear y fusionar la PR de la funcionalidad][ex8] | Revisión y fusión | Revisa el filtrado, las instrucciones, la habilidad, el perfil de control de calidad y las pruebas; después, usa Agent Merge para la segunda PR |
| [9. Explorar y crear lienzos][ex9] | Colaboración | Usa Database Explorer y, después, crea y prueba un lienzo de clasificación conectado al repositorio |
| [10. Resumen y próximos pasos][ex10] | Resumen | Repasa el flujo de trabajo, los recursos creados y otros materiales |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan **Copilot Student, Pro, Pro+, Business o Enterprise** activo
- [ ] Un ordenador con **macOS, Linux o Windows**
- [ ] [Git instalado][install-git] en el ordenador

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis mediante [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye el agente, MCP, la revisión de código y las funcionalidades de Copilot CLI que se utilizan en este taller, por lo que permite completar todos los recorridos.

> [!NOTE]
> Como la aplicación Copilot se ejecuta en tu equipo y no en un codespace, el [ejercicio de requisitos previos][ex0] explica cómo instalar Node.js y crear tu copia del proyecto antes de instalar la aplicación.

> [!NOTE]
> Si utilizas Copilot Business o Copilot Enterprise, el administrador debe habilitar la directiva **Copilot CLI** para que puedas utilizar la aplicación.

## Primeros pasos

**[Empieza por los requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students