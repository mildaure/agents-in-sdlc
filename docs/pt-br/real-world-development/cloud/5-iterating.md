---
title: "Exercício 5: Iterar no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que adicionasse documentação ao código, desenvolvesse um recurso de jogos relacionados para a equipe de design avaliar e implementasse recursos de acessibilidade, incluindo seletores de modo de alto contraste e modo claro. Vamos explorar as alterações no código e, se necessário, fornecer feedback ao Copilot para aprimorar o trabalho. Em seguida, habilitaremos a revisão para o restante da equipe.

### Cenário

Como já destacamos várias vezes, os fundamentos do design de software e do DevOps não mudam com a chegada da IA generativa. Sempre queremos revisar o código gerado e seguir nosso processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para criar a documentação, o recurso de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, existem restrições de segurança para manter o ambiente protegido. Entre elas:

- O Copilot tem acesso somente de leitura ao repositório e acesso de gravação **somente** à branch usada para o trabalho.
- O agente de nuvem é executado no GitHub Actions, que cria um ambiente separado e temporário para o trabalho.
- Todos os fluxos de trabalho do GitHub Actions precisam da aprovação de uma pessoa antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), inclusive o acesso a servidores MCP.

## Revisar a documentação gerada

Vamos começar explorando a primeira pull request (PR) gerada pelo agente de nuvem do GitHub Copilot: a PR que adiciona documentação ao código. Você fará isso usando a interface padrão de PR do GitHub.com.

> [!NOTE]
> Ao explorar a PR, talvez você veja um aviso de que o Copilot está bloqueado por um firewall. Isso é **esperado**, pois o Copilot tem acesso limitado a recursos externos por padrão, incluindo chamadas a servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao repositório em github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra a pull request intitulada algo como **Add missing documentation** ou um título mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, a pull request terá o sinalizador **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos; aproveite para fazer uma pausa ou refletir sobre o que aprendeu.

4. Quando a pull request estiver pronta, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui comentários TSDoc e outras documentações recém-criados. As alterações exatas variam.

   Ao analisar as alterações, procure comentários TSDoc, convenções TypeScript e cabeçalhos de comentários. Eles vêm dos arquivos de instruções personalizadas examinados no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role para baixo.
7. Você verá um indicador de que alguns fluxos de trabalho aguardam aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho aparecerão na seção de verificações da pull request. Se tudo der certo, as verificações do projeto serão aprovadas para o aplicativo Astro único. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Trabalhar com o Copilot em uma pull request não é uma via de mão única. Você pode mencionar o Copilot em comentários na PR, como faria com outras pessoas da equipe, ou em comentários em linha no código. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados não são determinísticos, não há um texto único que você precise usar. Algumas ideias de solicitações:

- Adicionar ao início de cada arquivo de código um cabeçalho de comentários que descreva brevemente sua finalidade.
- Adicionar comentários TSDoc a arquivos TypeScript e Astro.
- Criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário pedindo uma alteração à documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das ideias acima ou outra sugestão de documentação que queira ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe como ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar à PR.

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, um novo commit aparecerá na pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Quando estiver tudo certo, você poderá marcar a PR como pronta para revisão e mesclá-la à branch principal.

![Marcar PR como pronta para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Agora, volte à PR gerada pelo Copilot para resolver o issue sobre a exibição de jogos relacionados na página de detalhes.

1. Volte ao repositório em GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione a PR com um título semelhante a **Show related games on the game details page** ou um título mais descritivo.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role para baixo.
6. Você verá um indicador de que alguns fluxos de trabalho aguardam aprovação.
7. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Os fluxos de trabalho aparecerão na seção de verificações da pull request. Se tudo der certo, as verificações do projeto serão aprovadas para o aplicativo Astro único. Isso pode levar alguns minutos.
9. **Opcional:** você também pode alternar seu Codespace para essa branch e testar manualmente o recurso de jogos relacionados. Acesse o Codespace, abra o terminal e execute os comandos a seguir. Substitua `<branch-name>` pelo nome da branch criada pelo Copilot, como **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou o recurso de jogos relacionados! Como antes, você pode iterar com o agente de nuvem do Copilot e solicitar atualizações. Por exemplo, peça que ajuste o número de jogos relacionados exibidos ou adicione cabeçalhos de comentários e comentários TSDoc. Lembre-se de que esta tarefa foi atribuída **antes** da atualização das instruções personalizadas. Assim como antes, você pode fazer esses pedidos em um novo comentário na guia **Conversation**; o Copilot o verá e iniciará uma nova sessão.

## Revisar os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados com o agente personalizado de acessibilidade. Esta PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no painel de controle do Exercício 4.

1. Volte ao repositório em GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione a PR com um título semelhante a **Add high contrast mode to website** ou um título mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, a pull request terá o sinalizador **[WIP]**. Nesse caso, aguarde a conclusão do trabalho, que pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - componentes de UI dos seletores de modo.
   - uso do armazenamento local para manter as preferências da pessoa usuária.
   - alterações de CSS ou estilo dos modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - código JavaScript/TypeScript que gerencia a troca de modos.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role para baixo.
7. Você verá um indicador de que alguns fluxos de trabalho aguardam aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho aparecerão na seção de verificações da pull request. Se tudo der certo, as verificações do projeto serão aprovadas para o aplicativo Astro único. Isso pode levar alguns minutos.
10. **Opcional:** alterne seu Codespace para essa branch para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute os comandos a seguir. Substitua `<branch-name>` pelo nome da branch criada pelo Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Em seguida, inicie o aplicativo e teste os seletores dos modos de alto contraste e claro no navegador. Confirme que funcionam e que as preferências são mantidas após recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar esses recursos seguindo as práticas recomendadas de acessibilidade. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário para solicitar atualizações, como fez nas PRs anteriores.

## Exercício opcional — continuar delegando

O agente de nuvem é mais útil quando você pode atribuir itens reais do backlog e se concentrar em outras tarefas. Para praticar, registre mais alguns issues no repositório e atribua-os ao Copilot. Algumas ideias:

- criar um formulário para demonstrar interesse em um jogo na página de detalhes.
- implementar paginação na página de lista de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o ambiente prático do agente de nuvem. Ao longo destas lições, você:

- **Inspecionou os arquivos de instruções personalizados incluídos no repositório** para observar a influência deles nos resultados do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando seletores dos modos de alto contraste e claro.
- **Usou a página Copilot agents como centro de controle** para acompanhar e orientar a sessão de acessibilidade.
- **Revisou e iterou nas pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando fluxos de trabalho.

## Revisão e próximos passos

Você concluiu o ambiente prático do agente de nuvem. Se quiser continuar explorando, os outros ambientes complementam o que você praticou:

- 🖥️ **[Ambiente do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração MCP diretamente no IDE.
- 💻 **[Ambiente da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal usando os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, MCP do Playwright, Agent Merge e uma integração opcional `/delegate` com o agente de nuvem.

No seu próprio repositório, experimente estas ideias:

- atribuir ao agente de nuvem um issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
