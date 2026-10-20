---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser executada de uma maneira específica — ou se há informações de contexto que o Copilot deve conhecer —, você precisa garantir que ele possa acessá-las. Há várias maneiras de compartilhar um contexto específico com o Copilot. Entre as principais estão os [arquivos de instruções][instruction-files], que permitem orientar a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instruções com escopo por caminho.
- gerar a primeira parte da camada de dados da filtragem (um helper de editoras) usando as instruções atuais.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.
- executar o mesmo prompt novamente e observar o código gerado adotar o novo padrão.
- fazer commit da atualização das instruções e da parte da filtragem na `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir alguns padrões definidos. O Copilot é não determinístico. O objetivo deste exercício é observar a tendência de mudança no comportamento após atualizar as instruções, não reproduzir exatamente cada caractere da saída.

## Arquivos de instruções

### Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você fornecerá ao Copilot as informações necessárias para seguir as práticas destacadas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Esse recurso permite orientar o Copilot sobre convenções de programação, bibliotecas e até mesmo os tipos de comentários que você prefere incluir. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivo e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo de instruções enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter informações do projeto relevantes para a maioria das solicitações de chat ou CLI, como a pilha de tecnologias, uma visão geral do que está sendo desenvolvido, práticas recomendadas e outras orientações gerais.
- Os arquivos `.github/instructions/*.instructions.md` podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para fornecer diretrizes para linguagens como TypeScript ou Astro ou para tarefas como criar um componente de UI ou um novo conjunto de testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados somente para gerar código no Copilot Chat — não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam os arquivos de instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre como criar arquivos de instruções está além do escopo deste workshop. No entanto, os exemplos do projeto demonstram uma abordagem representativa. Em linhas gerais:

- Mantenha o arquivo `copilot-instructions.md` focado nas orientações do projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões gerais de programação.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo (testes unitários, componentes Astro e camada de dados, por exemplo) ou de tarefas específicas.
- Use linguagem natural. Mantenha as orientações claras e inclua exemplos de como o código deve ou não deve ser escrito.

Não existe uma única maneira correta de criar arquivos de instruções, assim como não existe uma única maneira de usar IA. A experimentação ajudará você a descobrir o que funciona melhor para seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá instruções para várias tarefas, incluindo [atualizações de UI][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece esse recurso de maneira diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI) — a lição da interface usada indicará quando isso for relevante.
>
> Procurando modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório repleto de arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas do projeto

Reserve um momento para ler os arquivos de instruções incluídos neste repositório — há um arquivo principal `copilot-instructions.md` e um conjunto de arquivos `*.instructions.md` para várias tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas de **GitHub Actions Workflows**. Elas se aplicam a qualquer interação com o Copilot neste repositório.
3. Abra a pasta `.github/instructions` e explore os arquivos. Observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um padrão glob, relativo à raiz do repositório, que determina a quais arquivos as instruções se aplicam. Neste caso, qualquer arquivo de teste TypeScript (por exemplo, um que corresponda a `**/*.test.ts`) será incluído.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes no projeto. Assim, você pode dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar exemplos para o Copilot seguir. (Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Criar uma branch para as alterações

Vamos seguir algumas práticas recomendadas e criar uma branch para nossas alterações.

1. Volte ao codespace usado no exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma nova branch e alterne para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. A escolha do modo e do modelo varia em cada exercício — as etapas indicarão o que importa.

Para observar o impacto das instruções personalizadas, comece enviando um prompt com o arquivo de instruções atual. Mais tarde, atualize-o e envie o mesmo prompt novamente para comparar o resultado.

1. Feche as guias abertas de exercícios anteriores para que o Copilot receba somente o contexto desejado.
2. Abra `src/lib/publishers.ts` para indicar ao Copilot onde o helper deve ficar.
3. Na lista suspensa de agentes da visualização Chat, selecione **Agent** para permitir que o Copilot aplique alterações nos arquivos.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o seguinte prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explora o projeto e aplica as atualizações no código, geralmente incluindo o arquivo helper e os testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras — isso segue as convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, aplicável a `src/lib/*.ts`.
7. Observe que o código proposto **não tem** comentários TSDoc nem um cabeçalho de comentários no nível do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários de documentação mesmo sem receber essa orientação. Se isso acontecer, tudo bem — o objetivo continua sendo observar a melhoria da *consistência* depois da atualização das instruções.

## Adicionar um novo padrão para o repositório

Como destacado anteriormente, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Vamos documentar os padrões de programação do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe como ela documenta os padrões de programação do projeto, mas ainda não contém uma regra para documentação no código. Por isso, o helper gerado não tinha comentários.
3. Adicione as seguintes linhas Markdown logo abaixo dos padrões existentes para orientar o Copilot a adicionar cabeçalhos de comentários nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem existir no nível do repositório (`.github/copilot-instructions.md`) para fornecer orientações gerais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo e tarefas específicos. O arquivo do repositório é o lugar certo para padrões gerais, como a regra de comentários de documentação adicionada.

## Executar o prompt novamente e observar a diferença

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto da conversa.
2. Selecione novamente `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie o **mesmo prompt** de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão do projeto e aplicá-lo a código real que servirá de base para o próximo exercício.

## Fazer commit das instruções e enviar a branch

Os arquivos de instruções são recursos do repositório e devem ser gerenciados pelo controle de versão, como qualquer outro arquivo. Vamos fazer commit e enviar a branch ao repositório.

1. Abra uma nova janela do terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme que as atualizações das instruções e do helper estão presentes:

   ```bash
   git status
   ```

3. No terminal, prepare e faça commit da atualização das instruções e de todos os arquivos alterados pelo Copilot para a base do helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e mesclar uma pull request

Com a branch enviada, crie uma pull request e associe-a ao issue de padrões de documentação do backlog. O Copilot pode fazer isso por você usando as ferramentas do GitHub já conectadas ao projeto. Vamos pedir ao Copilot que encontre o issue, crie uma PR para fechá-lo e, em seguida, faça a mesclagem.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executará ações no GitHub em seu nome. O modelo do projeto já configura as ferramentas do GitHub no workspace, então não há nada para configurar — na primeira vez que o Copilot usar uma delas, o VS Code poderá pedir que você entre no GitHub. Siga as instruções para permitir o acesso. Em um exercício posterior, você aprenderá como essa conexão funciona com o Model Context Protocol.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme que **Agent** está selecionado no seletor de agentes.
3. Peça ao Copilot que encontre o issue sobre a atualização dos arquivos de instruções e crie uma PR a partir da branch atual, descrevendo as atualizações das instruções e a nova base do helper de editoras:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Se o Copilot solicitar permissão para executar ações no GitHub em seu nome, revise o comando e selecione **Allow** quando apropriado.
5. Depois que a PR for criada, peça ao Copilot que a mescle e retorne sua branch para `main` com o seguinte prompt:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Se o Copilot solicitar permissão para executar ações no GitHub e comandos de shell em seu nome, revise o comando e selecione **Allow** quando apropriado.

Você acabou de criar e mesclar uma pull request com a ajuda do GitHub Copilot!

## Resumo e próximos passos

Você explorou como o Copilot usa o contexto dos arquivos de instruções do projeto e, em seguida, usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado usando as instruções *existentes*.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.
- enviar novamente o mesmo prompt e observar o código proposto adotar o novo padrão.
- fazer commit das atualizações das instruções e da base do helper na `main` para que o próximo exercício possa aproveitar esse trabalho.

Em seguida, você usará essas instruções no [modo de agente][next-lesson] enquanto o Copilot adiciona um novo recurso à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções personalizadas pessoais para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
