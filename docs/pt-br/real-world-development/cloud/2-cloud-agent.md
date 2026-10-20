---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

É provável que poucas organizações — se houver alguma — não enfrentem problemas com dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que continuam no backlog por falta de tempo para implementá-los. O agente de nuvem do GitHub Copilot foi desenvolvido para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele gera uma PR de rascunho para uma pessoa desenvolvedora revisar. Assim, é possível delegar tarefas repetitivas, acelerar o desenvolvimento e liberar as pessoas desenvolvedoras para se concentrarem em questões mais amplas.

Você explorará os seguintes recursos do agente de nuvem do Copilot:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem dívida técnica para resolver. As pessoas terceirizadas inicialmente contratadas para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, você verá que ela está totalmente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas no aplicativo.

Além disso, a equipe de design está pronta para melhorar a descoberta de jogos. Ela quer que a página de detalhes de cada jogo mostre títulos relacionados — outros jogos da mesma categoria — para que as pessoas possam continuar explorando. Ainda não é preciso uma implementação refinada; a equipe só quer algo que possa usar em testes de aceitação da experiência do usuário (UX). Esse trabalho está atualmente bloqueado, embora existam issues de prioridade mais alta.

Esses dois exemplos representam tarefas que podem acabar sendo adiadas e são ótimas para atribuir ao agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona enquanto você se concentra em outras tarefas e, depois, volta para revisar o resultado.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano de maneira semelhante a uma pessoa desenvolvedora. Assim como quando se trabalha com outra pessoa, há várias maneiras de utilizá-lo, incluindo [atribuir um issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber a atribuição, o Copilot cria uma pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode acompanhar a sessão enquanto o trabalho está em andamento ou depois de concluído. Quando a solução proposta estiver pronta para revisão, o Copilot mencionará você na pull request.

## A importância de instruções com escopo bem definido

Não há mágica no GitHub Copilot — não é possível deixar de pensar. Até operações aparentemente simples podem ser complexas quando analisadas em detalhes; por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a chegada da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já concluiu o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para examiná-lo rapidamente. Antes de atribuir trabalho ao agente de nuvem do Copilot, leia os arquivos de instruções incluídos no repositório sem editá-los para que possa observar sua influência depois.

Abra os seguintes arquivos na interface Web do GitHub ou em um codespace em execução:

- `.github/copilot-instructions.md` — examine a seção **Code standards**, especialmente as expectativas de convenções TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica essas instruções a arquivos `**/*.test.ts`.

Quando você atribuir o issue `Code lacks documentation` ao agente de nuvem na próxima seção, procure comentários TSDoc, convenções TypeScript e cabeçalhos de comentários na pull request resultante. Essas orientações vêm dos arquivos de instruções. Voltaremos a esse assunto ao revisar PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem do Copilot

A geração de código, independentemente de quem a realiza, costuma exigir um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a uma pessoa desenvolvedora de software.

O agente de nuvem usa [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo criando um [fluxo de trabalho especial de configuração][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, que será executado antes de o agente começar. Isso permite fornecer as ferramentas e dependências de desenvolvimento necessárias. O fluxo já está configurado para facilitar o laboratório e esta oportunidade de aprendizagem. Ele garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos binários de navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e populado para o aplicativo Astro único:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia Actions do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões como as mínimas necessárias para *suas etapas*. O Copilot receberá seu próprio token para as operações dele.
    permissions:
      # Se você quiser clonar o repositório nas etapas de configuração, por exemplo, para instalar dependências, precisará da permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo - Node.js (o aplicativo inteiro agora é Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrar e popular o banco de dados SQLite local para que a compilação e os testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer outro fluxo de trabalho do GitHub, mas tem alguns detalhes importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar na pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar manualmente o fluxo de trabalho na guia **Actions** do repositório. Isso é útil para testar se ele funciona corretamente sem esperar que o Copilot o execute.

## Adicionar documentação

Embora todo mundo reconheça a importância da documentação, muitos projetos têm informações desatualizadas ou simplesmente não têm documentação. Esse tipo de dívida técnica costuma ficar sem solução, reduzindo a produtividade, dificultando a manutenção da base de código e tornando mais complexa a integração de novas pessoas à equipe. Felizmente, o Copilot é especialmente útil para criar documentação, então este é um issue ideal para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse seu repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de novo issue.
4. Selecione **Blank issue** para criar o issue.
5. Defina o **Title** como `Code lacks documentation`.
6. Defina a **Description** como:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar o issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuir um issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

   ![Detalhes da atribuição do Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra a pull request (PR) recém-gerada, cujo título será semelhante a `[WIP]: Code lacks documentation`. Se a PR ainda não aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar alguns minutos para aparecer na PR. O Copilot está preparando o ambiente, executando o fluxo de trabalho descrito anteriormente, analisando o projeto e decidindo a melhor abordagem.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo da pull request. Você verá uma atualização informando que o Copilot começou a trabalhar no issue.
15. Selecione o botão **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão ao vivo e acompanhar como o Copilot resolve o problema: explora o código e entende seu estado, faz uma pausa para pensar e definir um plano adequado e cria o código.

Isso provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é executar tarefas de forma assíncrona e liberar você para se concentrar em outros trabalhos. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, em seguida, dedicar nossa atenção à criação de funcionalidades para o aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Como destacamos, uma das grandes vantagens do agente de nuvem do GitHub Copilot é poder dividir o trabalho: você se concentra em um conjunto de tarefas enquanto ele cuida de outro. A seção de jogos relacionados solicitada pela equipe de design talvez não leve muito tempo, mas esse tempo poderia ser usado em outras tarefas. Vamos atribuí-la ao agente de nuvem do Copilot!

1. Volte ao repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de novo issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina o **Title** como `Show related games on the game details page`.
6. Defina a **Description** como:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar o issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuir um issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá um conjunto de 👀 no primeiro comentário do issue, indicando que o Copilot está trabalhando!

![O Copilot usa o emoji de olhos para indicar que está trabalhando no issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando no segundo pedido! O agente de nuvem funciona de maneira semelhante a uma pessoa desenvolvedora de software: você não precisa acompanhá-lo ativamente; basta revisar o resultado quando estiver concluído. Vamos nos concentrar em criar e usar agentes personalizados.

## Resumo e próximos passos

Esta lição apresentou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para que trabalhe de forma assíncrona. Você pode usá-lo para resolver dívida técnica, criar recursos ou ajudar na migração de código entre frameworks.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, você pode se concentrar em criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e arquivos de instruções personalizadas, explorados nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
