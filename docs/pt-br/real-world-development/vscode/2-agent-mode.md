---
title: "Exercício 2 - Adicionar funcionalidade com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Mesmo as atualizações mais simples de um aplicativo costumam exigir alterações em vários arquivos e tarefas como executar testes. Normalmente, o fluxo de trabalho de desenvolvimento envolve localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou de fora, atualizá-lo e assim por diante. A lista continua.

É para isso que serve o modo de agente do Copilot.

O modo de agente do Copilot foi desenvolvido para atuar com mais autonomia no IDE. Ele se comporta de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo de agente para adicionar funcionalidade ao site.

> [!NOTE]
> Apesar dos nomes semelhantes, o modo de agente e o agente de nuvem foram criados para experiências diferentes. O modo de agente executa tarefas no IDE, permitindo ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o modo de agente do GitHub Copilot pode implementar novos recursos na interface e na camada de dados.
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. Você já adicionou um helper de editoras no exercício anterior e agora concluirá com o modo de agente do Copilot o trabalho restante na camada de dados, na interface e nos testes.

## Executar o site Tailspin Toys

Antes de fazer alterações, vamos explorar o site Tailspin Toys para entender sua funcionalidade atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com tema de programação. Ele permite listar jogos e exibir informações sobre eles. O site é um aplicativo Astro único que gera as páginas como HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter por meio do Drizzle ORM — não há API de back-end separada nem framework de interface do lado do cliente. Helpers de acesso a dados reutilizáveis ficam em `src/lib/`, e qualquer interatividade é adicionada com um pequeno `<script>` do Astro, com escopo definido e usando as APIs padrão do DOM.

### Iniciar o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento Astro. Você pode executá-la no GitHub Codespace:

1. Volte ao codespace. Você deve estar na `main` depois do exercício anterior.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma nova branch para o trabalho de filtragem e alterne para ela:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o comando a seguir para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá uma mensagem com o URL, semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo perguntando se deseja abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar um link para o URL localhost no terminal do Codespaces redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para seu codespace, que agora está hospedando o servidor Web!

### Explorar o site

Depois de iniciar o site, você poderá explorar suas funcionalidades principais:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com seus títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você acessa uma página de detalhes com mais informações, incluindo título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. A escolha do modo e do modelo varia em cada exercício — as etapas indicarão o que importa.

A implementação inicial do site funciona, mas queremos adicionar recursos. Vamos começar revisando o backlog. Quando você configurou o projeto com base no modelo, um backlog de issues foi criado automaticamente — peça ao GitHub Copilot que mostre esses itens.

1. Selecione **Agent** na lista suspensa de agentes da visualização Chat. O agente **Agent** planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos no terminal e invoca ferramentas.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão indicando preferência por um modelo específico. Durante o desenvolvimento, usamos o Claude Sonnet 4.5 e, por isso, incluímos esse modelo nas instruções. Esperamos que as sugestões de código sejam relativamente consistentes e proporcionem uma boa experiência. No entanto, como os LLMs são probabilísticos, as sugestões podem ser diferentes das indicadas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas ainda deverá conseguir concluir a tarefa.

3. Envie ao Copilot o seguinte prompt para perguntar sobre o backlog de issues:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todos os issues.
5. Revise a lista gerada.

Observe que o Copilot até priorizou os itens com base nos que considera mais úteis para as pessoas usuárias.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere o código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que contém orientações para adicionar funcionalidades ao site.

1. No codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais sobre como adicionar funcionalidades. Elas incluem:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para instruções específicas de tipos de arquivo, incluindo:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Isso permite reutilizar orientações, dividir conjuntos complexos em arquivos menores e mais gerenciáveis e fornecer exemplos e modelos.

## Implementar a funcionalidade de filtragem

Para concluir a filtragem, será necessário fazer pelo menos três atualizações distintas no aplicativo:

- adicionar ou aprimorar a lógica restante de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes para o comportamento da filtragem.
- atualizar a página de lista de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes de mesclar tudo à base de código. O modo de agente do Copilot pode realizar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela de exemplo da seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado na lista suspensa de agentes da visualização Chat.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade de acordo com o issue correspondente no backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe enquanto o Copilot explora o projeto e localiza os arquivos relacionados à funcionalidade. Ele encontrará os helpers da camada de dados, a interface e os testes. Em seguida, começará a modificar os arquivos e executar os testes.

   ![Captura de tela do Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, talvez apareçam os botões **Keep** e **Undo** no editor. Quando o Copilot terminar, você verá um botão **Keep** ou **Undo** para cada alteração. Não é preciso selecioná-los enquanto o trabalho ainda está em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel Copilot Chat pedindo confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez haja pausas e alguns testes falhem durante o processo. Não há problema! O Copilot alterna entre geração de código e testes até concluir a tarefa sem detectar erros.

   ![Captura de tela de uma sessão completa do modo de agente do Copilot Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado e procure possíveis problemas.

> [!CAUTION]
> Lembre-se de que é sempre importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, exploramos como usar o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Aprendemos como:

- o modo de agente do GitHub Copilot pode implementar novos recursos na interface e na camada de dados.
- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

Agora, vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir uma pull request.

### Exercício bônus de exploração – Implementar paginação

À medida que a lista de jogos cresce, será necessário implementar paginação. Use as habilidades aprendidas neste exercício e peça ao Copilot que atualize o site. Algumas considerações:

- siga as práticas recomendadas existentes, incluindo o uso dos arquivos de instruções do projeto.
- decida como implementar a paginação: permitir que a pessoa usuária selecione o tamanho da página ou defini-lo no código.
- ao criar o prompt, forneça ao Copilot as orientações necessárias para implementar o que você deseja.
- talvez seja necessário iterar com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot!

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usá-los][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
