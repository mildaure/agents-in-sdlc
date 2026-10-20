---
title: "Lição 8 - Criar e mesclar a PR do recurso"
description: "Revise a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; em seguida, crie uma PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora, revise tudo em conjunto e abra uma pull request. Você mesmo mesclou a PR de classificação por estrelas; desta vez, deixe o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções, a atualização da skill e o agente de QA em algumas PRs separadas. Para simplificar o workshop, você manteve todo o fluxo de filtragem e qualidade em uma conversa e branch, e todo o trabalho será incluído nesta PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionar a alteração completa do recurso e as evidências de verificação.
- criar a PR de filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar que a PR foi mesclada.

## Cenário

Durante o fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante da PR, mantendo a autorização da mesclagem sob o controle do desenvolvedor.

## Apresentação do Agent Merge

O **Agent Merge** automatiza o trabalho restante necessário para concluir uma pull request. Quando você o habilita, o Copilot resolve os bloqueios da PR — corrigindo verificações de integração contínua (CI) com falha, respondendo a comentários de revisão e fazendo rebase quando necessário — e, em seguida, habilita a mesclagem automática do GitHub quando o repositório permite.

Até agora, você selecionou **Merge pull request** por conta própria. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que está pronto antes de habilitar o Agent Merge.

## Usar o Agent Merge para gerenciar a PR

Com todo o código pronto, vamos revisá-lo, criar a PR e deixar que o Agent Merge gerencie as etapas seguintes.

1. Volte ao Codespace.
2. Abra a caixa de diálogo do agente digitando `/agent`.
3. Selecione **Default** na lista de opções e pressione <kbd>Enter</kbd>.
4. Crie uma PR usando o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua da PR. Quando tudo for aprovado, ele fará a mesclagem.
7. Confirme que aparece uma mensagem do Copilot semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de mesclagem, configurações do repositório nem permissões ausentes. Se o processo for bloqueado, leia o motivo informado e conclua manualmente a mesclagem revisada quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias partes do processo de desenvolvimento, incluindo a geração, os testes e a validação do código e, agora, o processo da pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionou a alteração completa do recurso e as evidências de verificação.
- criou a PR de filtragem.
- habilitou o Agent Merge somente depois da revisão e confirmou a mesclagem da PR.

Em seguida, você vai [explorar outros comandos de barra úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
