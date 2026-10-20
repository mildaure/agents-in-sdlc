---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su pull request y comprobar el Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica el Space Quiz con GitHub Pages para que todo el mundo pueda acceder a él. Copilot puede preparar el flujo de trabajo y guiarte durante la publicación, pero tú eres responsable de revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure una implementación en GitHub Pages.
- revisarás el flujo de trabajo y la pull request antes de fusionarla.
- seguirás la implementación y comprobarás el sitio publicado.

## Escenario

El Space Quiz ya está en un repositorio de GitHub, pero la gente aún necesita una forma sencilla de probarlo. Publicar el sitio, que consta de un único archivo, con GitHub Pages proporciona una URL pública sin añadir dependencias de servidor ni de entorno de ejecución.

## Pedir a Copilot que publique el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de tres etapas: una pull request de space-quiz fusionada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y el Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pide al agente que cree y fusione una pull request y cambie la configuración de Pages del repositorio. Revisa las diferencias, los permisos de implementación y cualquier solicitud de aprobación de GitHub antes de continuar.

## Revisar el flujo de trabajo y comprobar la implementación

1. Confirma que el README solo se añade si el repositorio aún no tiene uno.
2. Revisa los cambios de la pull request, incluidos los pasos de compilación e implementación del flujo de trabajo.
3. Fusiona la pull request solo después de que se superen las comprobaciones obligatorias.
4. Sigue la ejecución de implementación de GitHub Actions hasta que termine correctamente.
5. Abre la URL proporcionada y confirma que se carga el Space Quiz.

Si el agente no puede habilitar Pages ni fusionar la pull request, sigue las instrucciones específicas que te proporcione. No des la tarea por terminada hasta que la implementación se complete correctamente y el sitio publicado se cargue.

## Resumen y siguientes pasos

El Space Quiz ya está disponible como sitio publicado y has comprobado la implementación en lugar de confiar únicamente en el informe del agente. Continúa con la [Lección 13: Revisión y próximos pasos][next-lesson].

[next-lesson]: ../13-review/
