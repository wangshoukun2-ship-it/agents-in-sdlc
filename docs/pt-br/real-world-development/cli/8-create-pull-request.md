---
title: "Lição 8 - Criar e mesclar a PR do recurso"
description: "Revise a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie uma PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora é hora de revisar tudo em conjunto e abrir uma pull request. Você mesclou a pull request (PR) das avaliações por estrelas manualmente; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções, a atualização da skill e o agente de QA em algumas PRs separadas. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma conversa e branch, e enviaremos todo esse trabalho nesta PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionar toda a mudança do recurso e as evidências de verificação.
- criar a PR de filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar a mesclagem da PR.

## Cenário

Durante o fluxo de trabalho da filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora a Tailspin Toys quer automatizar o trabalho restante da PR, mantendo a autorização para mesclar sob o controle da pessoa desenvolvedora.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante necessário para concluir uma pull request. Quando você o habilita, o Copilot resolve os impedimentos da PR — corrige verificações de integração contínua (CI) que falharam, responde aos comentários de revisão e faz rebase quando necessário — e, em seguida, habilita a mesclagem automática do GitHub se o repositório permitir.

Até aqui, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que ele está pronto antes de habilitar o Agent Merge.

## Use o Agent Merge para gerenciar a PR

Com todo o código pronto, vamos revisar as mudanças em conjunto, criar a PR e deixar o Agent Merge gerenciar as próximas etapas.

1. Volte ao Codespace.
2. Abra a caixa de diálogo de agentes digitando `/agent`.
3. Na lista de opções, selecione **Default** e pressione <kbd>Enter</kbd>.
4. Crie uma PR com o comando `/pr create`.
5. Ative o Agent Merge com o comando `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua na PR. Quando todas as verificações forem aprovadas, ele fará a mesclagem.
7. Confirme se o Copilot informa algo semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de mesclagem, configurações do repositório nem permissões ausentes. Se houver um bloqueio, leia o motivo informado e conclua manualmente a mesclagem revisada quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento: geração de código, testes e validação e, agora, o processo da pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionou toda a mudança do recurso e as evidências de verificação.
- criou a PR de filtragem.
- habilitou o Agent Merge somente depois da revisão e confirmou a mesclagem da PR.

Em seguida, você [explorará outros comandos slash úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
