---
title: "Lição 3 - Modos do agente: Plan e Autopilot"
description: "Use Plan para definir uma abordagem, Autopilot para implementar a filtragem com base em um issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto. Alterações maiores, porém, exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi desenvolvido para trabalhar com o fluxo existente da organização e ajudar a construir o que é preciso, da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento com agentes: começará com um issue para gerar um novo recurso, verificará se o código é válido e se o recurso funciona como esperado e, por fim, fará a mesclagem bem-sucedida ao projeto.

> [!NOTE]
> Você usará a mesma conversa e branch ao longo do fluxo de trabalho do recurso. Normalmente, seriam usadas branches ou PRs diferentes para os vários tipos de arquivo, mas vamos simplificar para manter o foco nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de um issue do GitHub.
- definir os requisitos no modo Plan.
- implementar o novo recurso usando o modo Autopilot.
- revisar o código.
- validar manualmente o recurso em um navegador encaminhado.

Ao continuar o desenvolvimento desse recurso, você atualizará as instruções do repositório, personalizará a skill `quality-checks` existente, adicionará a validação MCP, criará um agente de QA e abrirá a PR do recurso.

## Cenário

O catálogo do Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. O issue do backlog descreve o recurso, mas detalhes como a combinação de categorias precisam ser definidos antes da programação. Você usará o modo Plan para decidir esses pontos e, em seguida, autorizará uma implementação com escopo definido usando o Autopilot.

## Contexto

Introduzir agentes de programação com IA no fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles se tornam ainda mais importantes! A maioria dos desenvolvedores segue um fluxo semelhante a este:

1. Começar com um issue registrado que descreva o trabalho necessário.
2. Criar um plano do que precisa ser desenvolvido.
3. Desenvolver e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar uma pull request (PR).
7. Depois da revisão do código e da aprovação do processo de integração contínua, mesclar o código.

> [!NOTE]
> Os detalhes variam de acordo com a equipe e a organização, mas a maioria dos fluxos segue alguma variação desse processo.

Seguir essa abordagem padrão ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de validação que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham juntos. O agente sugere alterações e aguarda sua entrada antes de continuar.
- **Plan**: o agente cria um plano primeiro e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma — escreve código, executa testes e repete o processo até concluir a tarefa.

Comece no modo Plan, revise o plano e, em seguida, use o Autopilot para implementá-lo.

## Começar pelo issue

Antes de iniciar o trabalho de filtragem, volte ao Codespace e confira se o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme que a PR de classificação por estrelas foi mesclada.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todos os issues.
8. Use as teclas de seta para destacar o issue intitulado **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar o issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` ou um número semelhante. O caractere `#` permite adicionar um issue ou uma pull request (PR) do GitHub ao contexto.

## Planejar o recurso de filtragem

O planejamento permite definir a abordagem de implementação de um recurso ou de execução de uma tarefa antes de delegá-la ao Copilot. É recomendável dedicar algum tempo ao planejamento de qualquer trabalho complexo. Vamos alternar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar para o modo Plan. Confirme que o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência ao issue adicionada na etapa anterior, insira o seguinte prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começa a elaborar o plano. Primeiro, ele explora o projeto e, em seguida, determina a melhor abordagem.

3. Durante o processo, o Copilot pode fazer perguntas sobre o funcionamento da filtragem. Responda de acordo com suas preferências. Não há respostas erradas!
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Role a tela para cima e para baixo para revisar o plano.
6. Peça ao Copilot que revise qualquer parte que não corresponda às suas decisões.

## Aprovar o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo! Vamos deixar o Copilot trabalhar usando o Autopilot.

O Autopilot permite que o Copilot repita o processo até considerar que a tarefa está concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção equivalente na versão instalada.
2. Confirme que o indicador abaixo do prompt mostra **Autopilot**.
3. Observe enquanto o Copilot segue o plano definido, gera o código e executa os testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente; por isso, revise o plano antes. Se o Copilot informar que faltam dependências ou que há um conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revisar e verificar a implementação

Depois de gerar o código, revise-o antes de mesclá-lo, como faria com qualquer outro código. Vamos revisar o código e executar o site para conferir se tudo está funcionando.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não mostra mais **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com o issue e as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização de diferenças.
5. Revise os resultados das verificações do projeto e peça ao Copilot que resolva eventuais falhas.

## Explorar a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste a filtragem por categoria, por editora e pelas combinações definidas no plano.
4. Confirme que o comportamento de redefinição e de resultados vazios corresponde ao issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que pare o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para desenvolver e revisar um recurso. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de um issue do GitHub.
- definiu os requisitos no modo Plan.
- implementou o novo recurso usando o modo Autopilot.
- revisou o código.
- validou manualmente o recurso em um navegador encaminhado.

Em seguida, vamos explorar como garantir que o código siga as práticas documentadas, [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
