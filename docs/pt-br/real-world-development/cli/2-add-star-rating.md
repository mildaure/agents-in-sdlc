---
title: "Lição 2 - Adicionar classificações por estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cartões de jogos, revisá-la em um navegador encaminhado e mesclá-la como sua primeira pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e iniciou uma conversa, é hora de fazer sua primeira alteração no projeto. Ela será pequena: os jogos já têm uma classificação por estrelas nos dados, mas os cartões da página inicial ainda não a exibem. Você pedirá ao agente que mostre a classificação, revisará a alteração e a mesclará como sua primeira pull request.

Nesta lição, você vai:

- iniciar uma conversa focada no Copilot em uma branch de recurso.
- pedir ao agente que faça uma pequena alteração no projeto.
- revisar a alteração com `/diff`.
- executar o aplicativo para confirmar a alteração em um navegador encaminhado.
- abrir e mesclar sua primeira pull request.

## Cenário

Cada jogo no Tailspin Toys pode ter uma classificação por estrelas, que já aparece na página de detalhes do jogo. No entanto, os cartões da página inicial exibem apenas o título, a categoria, a editora e a descrição. Como aquecimento, peça ao agente que mostre a classificação existente em cada cartão — uma alteração pequena e independente, ideal para sua primeira sessão.

## Estrutura de uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal na CLI usa o repositório e a branch Git atualmente selecionados no terminal, em vez de criar um worktree dedicado. As conversas salvas permitem retomar a mesma discussão mais tarde, enquanto os arquivos e a branch continuam sendo o estado normal do Git no disco.

Em uma conversa, você verá três elementos: seus prompts e as respostas do agente, as atividades das ferramentas que o agente usa para explorar e editar arquivos e as alterações que você pode inspecionar com `/diff`.

## Iniciar uma conversa e solicitar a alteração

Vamos iniciar uma conversa para começar a implementar o recurso.

1. Volte ao seu Codespace.
2. Se o terminal ainda não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se ainda não estiver aberto, inicie o Copilot com o seguinte comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando de barra `/new` e pressione <kbd>Enter</kbd>.
5. Use o seguinte prompt para solicitar a alteração:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explora o projeto, localiza os arquivos que exibem os detalhes dos jogos e cria o código necessário. Você acabou de adicionar um recurso usando o Copilot CLI!

## Revisar as diferenças

Todas as alterações geradas por IA devem ser revisadas antes da mesclagem, mesmo as pequenas. Vamos explorar as alterações diretamente no Copilot CLI.

1. Digite `/diff` e inspecione cada arquivo alterado.
2. Confirme que o cartão do jogo exibe a classificação numérica quando ela existe e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes abrangem os dois estados.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija eventuais falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diferenças.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico; portanto, o código exato pode variar. Revise o comportamento em vez de esperar uma implementação específica.

## Verificar as alterações

Não devemos apenas ler o código e presumir que funciona. Vamos pedir ao Copilot que inicie o site para podermos examinar a interface do usuário (UI) atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cartões dos jogos exibem as classificações em uma escala de cinco estrelas.
4. Volte ao Copilot e peça que pare o servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abrir e mesclar sua primeira pull request

Você já criou o recurso! Agora, crie uma pull request (PR) para mesclar o novo código ao projeto.

1. Peça ao agente padrão que faça commit da alteração:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. O Copilot CLI envia o commit existente ao criar a PR e exibe o URL dela.
3. Abra a PR mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o URL exibido pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme a mesclagem.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração usando o GitHub Copilot CLI! Em particular, você:

- iniciou uma conversa focada no Copilot em uma branch de recurso.
- orientou o agente a fazer uma pequena alteração nos cartões dos jogos.
- revisou a alteração com `/diff`.
- executou o aplicativo para confirmar a classificação por estrelas em um navegador encaminhado.
- abriu e mesclou sua primeira pull request.

Em seguida, você [partirá do issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para desenvolver um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
