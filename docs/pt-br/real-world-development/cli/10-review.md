---
title: "Lição 10 - Encerramento e próximos passos"
description: "Recapitule o fluxo de trabalho do Copilot CLI, as duas pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo de trabalho contínuo no Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou o issue de filtragem já criado.
- adicionou classificações por estrelas, revisou o resultado em um navegador encaminhado e mesclou manualmente sua primeira pull request (PR).
- partiu do issue de filtragem, definiu a abordagem no modo Plan, desenvolveu no modo Autopilot e revisou no modo Interactive.
- orientou o agente com instruções personalizadas, personalizou a skill `quality-checks` existente e a usou para executar testes unitários, lint e verificações de tipos.
- adicionou o servidor Model Context Protocol (MCP) do Playwright e usou-o para explorar a filtragem em um navegador real.
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências no navegador.
- revisou a alteração completa de filtragem e autorizou o Agent Merge para a PR de filtragem.
- explorou comandos de barra para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria branch criada a partir da `main` atualizada:

1. **Classificações por estrelas:** exibir o `starRating` existente e um estado explícito para jogos sem classificação nos cartões.
2. **Filtragem e fluxo de qualidade:** implementar a filtragem, atualizar as instruções e aplicá-las ao recurso, personalizar o relatório de `quality-checks`, criar um perfil de QA e incluir os testes relacionados.

Do planejamento da filtragem à abertura da PR, você usou a mesma conversa e branch. Reunimos esse trabalho em uma única PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: testes automatizados, inspeção no navegador e exploração do navegador pelo Copilot usando o MCP. A skill `quality-checks` executou testes unitários, lint e verificações de tipos e apresentou os resultados no novo formato. O QA reuniu esses resultados e revisou os requisitos e a cobertura dos testes antes da PR.

Os testes adicionados devem resolver lacunas reais; uma execução de QA que conclui não ser necessário adicionar testes também pode estar correta. Revise o código e as evidências antes de autorizar a mesclagem e atualize as evidências afetadas depois de fazer alterações.

## Práticas recomendadas

O contexto e as ferramentas fornecidos ao Copilot influenciam o trabalho. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações em outras conversas e ajuste-as conforme as necessidades da equipe mudarem. Instruções definem padrões, skills descrevem tarefas repetíveis, agentes personalizados representam funções especializadas e servidores MCP conectam ferramentas externas. Revise as alterações e os resultados reais das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** de acordo com a tarefa. Use **Plan** para pensar em uma abordagem antes de desenvolver, **Interactive** para acompanhar alterações focadas e **Autopilot** para tarefas com escopo bem definido. Escolha um modelo mais rápido para alterações rotineiras e um modelo mais capaz para trabalhos complexos.

O contexto continua sendo tão importante quanto a infraestrutura. Descrever claramente *o que* você quer desenvolver, *por que* e *como* muda significativamente o resultado.

## Mais para explorar

Você concluiu o fluxo principal. Veja outros recursos da CLI:

- `/review` para pedir que o agente de revisão de código analise as alterações.
- `/rubber-duck` para conversar sobre um problema e obter outra perspectiva.
- `/fleet` para coordenar tarefas independentes em paralelo.
- `/worktree` para isolar uma tarefa separada.
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximos passos

A melhor maneira de melhorar no uso de qualquer ferramenta é continuar usando-a! Use-a em código de produção, em projetos pessoais ou no pequeno aplicativo que você pensa em criar há anos. Compartilhe aprendizados com a equipe e aprenda com ela. E, como sempre, explore a documentação.

Se quiser estender o Tailspin Toys pelo terminal, continue com a [série opcional Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
