---
title: "Exercício 3 - Testar o recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo de agente. Antes de abrir uma pull request, confirme que ela funciona no navegador. Em vez de navegar pelo aplicativo manualmente, você conectará o **servidor MCP do Playwright** e deixará o Copilot controlar um navegador real para testar o recurso. Em seguida, publicará sua branch e abrirá a PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas disponíveis para o Copilot.
- adicionar o servidor MCP do Playwright ao workspace.
- pedir ao Copilot que use o servidor para testar manualmente o recurso de filtragem em um navegador.
- publicar sua branch e abrir uma pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente se torna muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) permite que o Copilot faça isso: é um padrão para o agente se comunicar com ferramentas e serviços externos.

![Diagrama que mostra o funcionamento interno do modo de agente e como ele interage com o contexto, o LLM e as ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) oferece aos agentes de IA uma maneira de se comunicar com ferramentas e serviços externos. Com o MCP, os agentes de IA podem se comunicar com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas (por meio de recursos) e executar ações em seu nome (por meio de ferramentas).

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e os serviços externos. O servidor MCP gerencia a comunicação entre o agente e as ferramentas externas, como APIs existentes ou ferramentas locais, por exemplo, pacotes NPM. Cada servidor MCP oferece um conjunto diferente de ferramentas e recursos ao agente.

Alguns servidores MCP populares:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: fornece acesso a um conjunto de APIs para gerenciar repositórios do GitHub. O agente de IA pode, por exemplo, criar e atualizar repositórios e gerenciar issues e pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: oferece recursos de automação do navegador com o Playwright. O agente de IA pode, por exemplo, acessar páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP que fornecem acesso a diferentes ferramentas e recursos. O GitHub mantém um [registro MCP](https://github.com/mcp) para facilitar a descoberta e as contribuições para o ecossistema.

> [!CAUTION]
> Em termos de segurança, trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise cuidadosamente o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores MCP confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Você verá um servidor `github` já configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

Essa entrada `github` vem com o modelo do projeto e permitiu que o Copilot lesse o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é necessário instalar nada localmente. O VS Code se conecta por HTTP e você se autentica no GitHub na primeira vez que o Copilot usa uma das ferramentas.

## Adicionar o servidor MCP do Playwright

Agora, adicione um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, exatamente o que é necessário para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, de modo que o arquivo fique assim:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

Diferentemente do servidor `github` remoto, `playwright` é um servidor **local**: o VS Code o inicia em sua máquina executando `npx @playwright/mcp@latest`. O sinalizador `--headless` orienta o Playwright a executar o navegador sem uma janela visível. Isso é necessário em um codespace, onde não há uma área de trabalho para exibi-la.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta; por isso, o navegador necessário geralmente já está instalado. Se o Copilot informar depois que falta um navegador, peça a ele que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia os servidores MCP **sob demanda** — na primeira vez que o Copilot precisar das ferramentas de um servidor, o VS Code o iniciará e perguntará se você confia nele.

1. Em `.vscode/mcp.json`, aparecerá uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar que a configuração está correta.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e escolha confiar nele para que o servidor possa iniciar.

Iniciá-lo manualmente é opcional. Se você não fizer isso, o VS Code iniciará o servidor e pedirá que você confie nele na primeira vez que o Copilot precisar de suas ferramentas, na próxima etapa.

## Testar o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador real que ele pode controlar. Em vez de você navegar pelo aplicativo para verificar o trabalho, o agente pode abrir uma página, navegar, aplicar filtros e apresentar os resultados — além de resumir o que observou. É uma maneira rápida de confirmar que o recurso funciona como esperado, sem sair da conversa.

Internamente, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. Assim, o agente raciocina sobre elementos estruturados e rotulados, como botões, links e itens de lista, da mesma maneira que as tecnologias assistivas. Portanto, uma verificação funcional rápida também serve como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste o recurso de filtragem recém-desenvolvido:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, executará cada etapa e apresentará os resultados. Compare o resumo com os critérios de aceitação do issue. Se algo parecer errado, faça perguntas complementares ou peça ao Copilot que corrija o código antes de abrir a pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para este teste. Se você tiver parado o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira utilização do servidor MCP do Playwright, talvez o Copilot precise baixar um navegador. Se ele informar que falta um, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar uma pull request

Agora que você confirmou que o recurso funciona, abra uma pull request (PR) para que a equipe possa revisá-lo. Primeiro, publique a branch `filtering-vscode`.

1. Acesse o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control mostrando as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar a pull request

Há várias maneiras de criar uma pull request, inclusive por github.com e pela interface de linha de comando (CLI) do GitHub. Como você já está trabalhando com o GitHub Copilot, vamos pedir que ele crie a PR. Ele pode encontrar o issue correspondente e associá-lo à PR.

1. Acesse o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Confirme que **Agent** está selecionado na lista suspensa de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie a PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Quando necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute as operações. Na primeira vez que usar uma ferramenta do GitHub, talvez seja necessário autenticar-se — siga as instruções para permitir o acesso.
5. Observe como o Copilot pesquisa os issues, encontra o correto e cria a PR.
6. Selecione o link fornecido pelo Copilot para revisar a pull request, mas **não a mescle ainda**.

## Resumo e próximos passos

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas disponíveis para o Copilot.
- adicionou o servidor MCP do Playwright ao workspace.
- usou o servidor para testar manualmente o recurso de filtragem em um navegador antes de publicá-lo.
- publicou a branch e abriu uma pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso, mas o MCP é apenas uma das maneiras de ampliar o Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para simplificar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
