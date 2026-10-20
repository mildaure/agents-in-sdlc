---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você usa o GitHub Copilot no modo de agente, ele trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho ocorre em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo à medida que acontecem. Você pode revisar cada diff no próprio chat, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts complementares para refinar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições de arquivos propostas.
- orientar o agente durante a sessão para adicionar um seletor de modo claro junto com o modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado na [Lição 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design fez um pedido adicional: também quer um seletor de modo claro para pessoas que consideram o tema padrão escuro demais. Em vez de começar outra conversa, você orientará a sessão atual para ampliar o trabalho.

## Acompanhar o progresso do Copilot

A visualização Copilot Chat mostra cada etapa do agente: arquivos explorados, comandos executados e edições propostas. Cada edição de arquivo aparece no chat com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, role a conversa para acompanhar cada chamada de ferramenta. Você verá leituras e edições de arquivos e os comandos de terminal que ele pedir para executar.
3. Quando o agente propuser uma edição, o diff aparecerá no próprio chat:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para desfazer a alteração e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver o conjunto completo de arquivos modificados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão — por exemplo, para executar um comando no terminal —, o agente fará uma pausa e exibirá uma solicitação **Allow**. Aprove somente os comandos com os quais você se sente confortável.

5. Aguarde até que o agente conclua a implementação do modo de alto contraste. Uma mensagem final resumirá as alterações feitas.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está pronto, amplie a mesma conversa para adicionar um seletor de modo claro. Como você continuará na mesma sessão de chat, o agente manterá todo o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilos e o padrão de armazenamento local.

1. Na caixa de entrada do Copilot Chat, envie o seguinte prompt complementar:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. O agente continuará de onde parou, explorará as alterações já feitas e proporá os acréscimos do modo claro.
3. Revise cada nova edição à medida que aparecer, como fez com o modo de alto contraste. Use **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface dos seletores de uma maneira que não agrade —, interrompa-o com o botão **Stop** ao lado da caixa de entrada e envie uma mensagem complementar para esclarecer o pedido.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts complementares para pedir ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione um terceiro seletor; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para os dois seletores").

## Revisar o conjunto de alterações

Antes de fazer commit do trabalho, revise rapidamente tudo o que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deve ver atualizações em componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme que os padrões de acessibilidade do agente personalizado foram aplicados, incluindo atributos ARIA, navegação por teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações pelo painel Source Control. Você as publicará [na próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts complementares para ampliar o trabalho sem perder o contexto.

Nesta lição, você explorou:

- como acompanhar o trabalho de um agente em tempo real.
- como aceitar e rejeitar edições individuais.
- como orientar uma sessão em andamento com prompts complementares.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em uma pull request e revisará as alterações produzidas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
