---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que assumisse três tarefas distintas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem trabalhe de forma assíncrona e autônoma, ainda é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página Agents][agents-page] no GitHub.com. Nesse centro de controle, você pode ver todas as tarefas de agente com pull requests (PRs) abertas. Também pode explorar as operações realizadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página Agents para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação de um modo de alto contraste, a equipe percebeu que também seria bom adicionar um modo claro. Como o trabalho já estava atualizando o estilo do site e adicionando seletores, pareceu fazer sentido incluir essa funcionalidade. Você orientará o trabalho do agente para que ele adicione o modo claro junto com o modo de alto contraste.

## Revisar as tarefas do agente de nuvem do Copilot

Vamos conferir o estado atual de todas as tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página Copilot agents](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você verá as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em execução. Revise as operações realizadas pelo Copilot, que podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel de pull request (PR) à direita. Nele, você pode ver a PR e os arquivos alterados para acompanhar o trabalho.

## Orientar o agente de nuvem

Agora que você viu as tarefas ativas, peça ao Copilot que inclua o seletor do modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à adição do modo de alto contraste. O título exato varia de acordo com o nome escolhido pelo Copilot e o estado atual do trabalho.

   ![Sessão de acessibilidade no centro de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Você saberá que isso aconteceu quando aparecerem mensagens semelhantes às mostradas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o seguinte prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de tela da tarefa do agente de nuvem na página Agents com a caixa Steer active session while Copilot is working destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot reconhece o prompt e o inclui no fluxo de trabalho.

## Deixar o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada! Ele incorporará a nova solicitação ao fluxo de trabalho depois de concluir a etapa que estava executando quando você enviou a mensagem.

Como isso levará alguns minutos, aproveite para refletir sobre tudo o que aprendeu e explorou até agora.

## Resumo e próximos passos

Esta lição apresentou a página Copilot agents, seu centro para acompanhar e orientar as tarefas do agente de nuvem do GitHub Copilot. Nela, você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar tarefas em andamento para ajustar o escopo ou fornecer orientações adicionais.

Você explorou estes conceitos:

- usar o centro de controle e a página Agents para acompanhar as tarefas do agente de nuvem.
- redirecionar uma sessão em andamento para solicitar funcionalidade adicional.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar nas pull requests criadas pelo Copilot][next-lesson].

## Recursos

- [Página Copilot agents][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
