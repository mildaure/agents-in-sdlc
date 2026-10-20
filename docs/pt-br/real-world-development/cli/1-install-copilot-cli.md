---
title: "Lição 1 - Instalar o GitHub Copilot CLI"
description: "Instale e autentique o Copilot CLI no Codespace, conheça o ambiente e localize o issue de filtragem criado automaticamente."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O [GitHub Copilot CLI][about-copilot-cli] é um poderoso assistente de programação baseado em agentes que funciona no terminal, permitindo explorar bases de código, gerar código, executar comandos e interagir com ferramentas externas — tudo pela linha de comando. Ele permite delegar tarefas, solicitar alterações e manter o foco. Como você pode imaginar, o primeiro passo é instalar a ferramenta. Felizmente, isso pode ser feito com ferramentas que você já conhece.

Nesta lição, você vai:

- instalar o GitHub Copilot CLI usando npm.
- autenticar-se com sua conta do GitHub.
- confiar no repositório do workshop e iniciar uma conversa rápida.
- localizar o issue de filtragem usando o servidor MCP integrado do GitHub.

## Cenário

Sua equipe está começando a usar agentes de IA para trabalhar em um backlog crescente. O Copilot CLI leva essa capacidade ao terminal, onde muitas pessoas desenvolvedoras já trabalham. Nesta lição, você fará a instalação, autenticação e configuração inicial para usá-lo no restante do workshop.

## Instalar o Copilot CLI

Você pode instalar o Copilot CLI por [npm][install-cli], WinGet ou Homebrew. Como o GitHub Codespaces já inclui o Node.js, você usará o npm.

1. Volte ao Codespace e abra um terminal.
2. Confirme que o Node.js está instalado e atende ao requisito de versão:

   ```bash
   node --version
   ```

   A versão exibida deve ser 24 ou superior.

3. Instale o Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Confirme a instalação:

   ```bash
   copilot --version
   ```

   Você deverá ver o número da versão.

## Autenticar-se com o GitHub

Na primeira inicialização, o Copilot CLI solicitará que você autentique sua conta do GitHub.

1. Inicie o Copilot CLI:

   ```bash
   copilot
   ```

2. Se solicitado, siga as instruções do código de dispositivo para autenticar-se e autorizar o Copilot CLI.
3. O Copilot CLI exibirá o seguinte prompt:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Confirme que o caminho corresponde ao repositório Tailspin Toys e responda sim selecionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> Em um Codespace, talvez você já esteja autenticado pela sua sessão do GitHub. Se o Copilot CLI iniciar sem solicitar autenticação, está tudo certo.

## Conhecer o ambiente

Os comandos no prompt normal do shell são executados diretamente no Codespace. Depois de iniciar o Copilot CLI, a linguagem natural é enviada ao agente e os comandos de barra controlam a conversa.

1. Digite `/model`, use as teclas de seta para selecionar **Auto**, pressione <kbd>Enter</kbd> e pressione <kbd>Enter</kbd> novamente para confirmar.
2. Digite `/help` para ver os comandos disponíveis na versão instalada e pressione <kbd>Esc</kbd> para fechar a tela de ajuda.
3. Faça uma pergunta simples ao Copilot para confirmar que tudo está funcionando:

   ```plaintext
   What are the key files in this project?
   ```

4. Leia a resposta e observe como o Copilot explora o repositório antes de responder.
5. Digite `/mcp list` e confirme que o servidor MCP integrado do GitHub está disponível.
6. Peça ao Copilot que encontre o issue de filtragem:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abra o URL e leia o issue. Você o usará depois de concluir uma primeira alteração rápida.

> [!TIP]
> Uma sessão normal do Copilot CLI usa a branch atualmente selecionada no terminal; ela não cria automaticamente um worktree. Você criará uma branch de recurso antes de cada alteração.

## Usar o atalho do workshop

Normalmente, o Copilot CLI pede permissão antes de usar ferramentas além das permissões já estabelecidas. Neste workshop, você o reiniciará com `--yolo`, um atalho aprovado por você que remove essas solicitações de aprovação no Codespace para que possa se concentrar nos exercícios.

> [!CAUTION]
> `--yolo` habilita permissões automáticas completas (`--allow-all-tools`, `--allow-all-paths` e `--allow-all-urls`). Use-o somente em um ambiente isolado, como um Codespace ou uma máquina virtual, e nunca o use como alias padrão no desenvolvimento diário. Consulte [Allowing and denying tool use][allow-all-warning] para obter mais informações.

Neste workshop, `--enable-all-github-mcp-tools` habilita as ferramentas MCP de leitura e gravação do GitHub usadas nas lições seguintes para trabalhar com issues e pull requests. O Codespace limita o acesso ao seu computador local, mas os recursos autenticados do GitHub continuam sendo reais. Revise as alterações antes de publicá-las ou mesclá-las.

1. Saia do Copilot CLI com `/exit`.
2. Reinicie-o na raiz do repositório:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Faça outra pergunta rápida sobre o projeto para confirmar que a conversa está funcionando e saia com `/exit`.

O Copilot salva as conversas automaticamente. Mais adiante, depois de atualizar instruções ou adicionar um agente, use `copilot --resume` para retomar a mesma conversa e branch de recurso.

## Resumo e próximos passos

Parabéns! Nesta lição, você:

- instalou o GitHub Copilot CLI usando npm.
- autenticou-se com sua conta do GitHub.
- confiou no repositório do workshop e iniciou uma conversa rápida.
- localizou o issue de filtragem usando o servidor MCP integrado do GitHub.

Em seguida, você vai [começar sua primeira alteração focada][next-lesson] e usar o Copilot CLI para exibir uma classificação por estrelas nos cartões dos jogos.

## Recursos

- [Instalar o GitHub Copilot CLI][install-cli]
- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
