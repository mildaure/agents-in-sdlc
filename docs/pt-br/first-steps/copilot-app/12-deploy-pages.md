---
title: "Lição 12 - Implantar o site com o GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um fluxo de trabalho do Pages, revisar a pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dê ao Space Quiz um endereço público implantando-o com o GitHub Pages. O Copilot pode preparar o fluxo de trabalho e orientar a implantação, mas cabe a você revisar as alterações e confirmar que o site está no ar.

Nesta lição, você vai:

- pedir ao Copilot que configure uma implantação do GitHub Pages.
- revisar o fluxo de trabalho e a pull request antes de mesclá-la.
- acompanhar a implantação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma maneira simples de experimentá-lo. Publicar o site de arquivo único com o GitHub Pages disponibiliza um URL público sem adicionar um servidor ou dependência de runtime.

## Pedir ao Copilot que implante o site

Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de três etapas: uma pull request do space-quiz mesclada; uma execução Deploy website bem-sucedida do GitHub Actions com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz publicado em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pede ao agente que crie e mescle uma pull request e altere as configurações do Pages no repositório. Revise o diff, as permissões de implantação e todos os pedidos de aprovação do GitHub antes de prosseguir.

## Revisar o fluxo de trabalho e verificar a implantação

1. Confirme que o README foi adicionado somente se ainda não havia um no repositório.
2. Revise as alterações da pull request, incluindo as etapas de compilação e implantação do fluxo de trabalho.
3. Faça a mesclagem somente depois que as verificações obrigatórias da pull request forem aprovadas.
4. Acompanhe a execução de implantação do GitHub Actions até a conclusão bem-sucedida.
5. Abra o URL informado e confirme que o Space Quiz é carregado.

Se o agente não puder habilitar o Pages ou mesclar a pull request, siga as instruções específicas fornecidas. Não considere a tarefa concluída até que a implantação seja bem-sucedida e o site publicado seja carregado.

## Resumo e próximos passos

Agora, seu Space Quiz está disponível como um site publicado e você verificou a implantação em vez de confiar apenas no relatório do agente. Continue com a [Lição 13: Revisão e próximos passos][next-lesson].

[next-lesson]: ../13-review/
