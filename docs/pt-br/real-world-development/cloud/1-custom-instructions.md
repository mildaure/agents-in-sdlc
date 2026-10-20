---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser executada de uma maneira específica — ou se há informações de contexto que o Copilot deve conhecer —, você precisa garantir que ele possa acessá-las. Os [arquivos de instruções][instruction-files] permitem fornecer essas orientações, para que o Copilot entenda não apenas *o que* você quer, mas também *como* quer que o trabalho seja feito.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instruções com escopo por caminho.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos ambientes do VS Code e da CLI, aqui você não executará prompts *antes e depois* — o agente de nuvem do Copilot trabalha de forma assíncrona nos issues do GitHub, então é mais difícil demonstrar o impacto em tempo real. Você verá a influência das instruções mais adiante neste ambiente, ao revisar as pull requests produzidas pelo agente de nuvem.

## Arquivos de instruções

### Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você fornecerá ao Copilot as informações necessárias para seguir as práticas destacadas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Esse recurso permite orientar o Copilot sobre convenções de programação, bibliotecas e até mesmo os tipos de comentários que você prefere. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivo e tarefas.

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
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um padrão glob, relativo à raiz do repositório, que determina a quais arquivos as instruções se aplicam. Neste caso, qualquer arquivo de teste TypeScript, como um que corresponda a `**/*.test.ts`, será incluído.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes no projeto. Assim, você pode dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar exemplos para o Copilot seguir. (Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Adicionar um novo padrão para o repositório

Esta é a única parte deste exercício que envolve edição: adicionar uma regra geral do projeto para que a documentação fique no código como comentários TSDoc e um cabeçalho de comentários no nível do arquivo. O agente de nuvem usará essa regra ao trabalhar nos issues que você atribuir mais adiante neste ambiente.

Antes de editar, prepare uma branch (o agente de nuvem lerá as instruções da branch de destino do issue, mas você fará o commit das alterações de forma organizada):

1. No terminal do codespace, crie uma branch e alterne para ela:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como destacado anteriormente, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Vamos documentar os padrões de programação do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe como ela documenta os padrões do projeto, mas ainda não inclui uma regra para documentação no código. Por isso, o helper gerado não tinha comentários.
3. Adicione as seguintes linhas Markdown logo abaixo dos padrões existentes para orientar o Copilot a adicionar cabeçalhos de comentários nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem existir no nível do repositório (`.github/copilot-instructions.md`) para fornecer orientações gerais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo e tarefas específicos. O arquivo do repositório é o lugar certo para padrões gerais, como a regra de comentários de documentação adicionada.

## Fazer commit, enviar e mesclar a atualização das instruções

O agente de nuvem lê os arquivos de instruções da branch de destino do issue. Quando você atribuir um issue ao Copilot no próximo exercício, o agente criará uma branch a partir da `main`. Portanto, as alterações nas instruções precisam chegar à `main` para que o agente de nuvem as use.

1. Prepare e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. Em github.com, abra uma pull request de `custom-instructions-cloud` para `main` e faça a mesclagem. O agente de nuvem lerá essas instruções ao trabalhar nos issues atribuídos no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente na `main` neste workshop, você pode ignorar as etapas da branch e fazer commit diretamente nela. A branch é usada para que o workshop reflita o fluxo de trabalho de um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot usa o contexto dos arquivos de instruções do projeto e adicionou um novo padrão para todo o repositório em `.github/copilot-instructions.md`. Você verá esse padrão nas pull requests produzidas pelo agente de nuvem nas próximas lições.

Em seguida, você vai [atribuir seu primeiro issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
