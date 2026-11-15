---
slug: pt-br/real-world-development/cloud
title: "Agente na nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O **[agente na nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** permite que o GitHub Copilot trabalhe de forma assíncrona na nuvem. Você atribui o trabalho no GitHub, e o agente na nuvem o executa em segundo plano — explorando o repositório, fazendo mudanças e abrindo uma pull request — enquanto você pode se dedicar a outras tarefas.

Nestes exercícios, você adicionará instruções personalizadas para orientar o agente na nuvem e, em seguida, atribuirá uma issue do GitHub para que ele implemente o trabalho. Você revisará e usará agentes personalizados para definir a abordagem, monitorará e orientará as sessões no painel de agentes e, por fim, revisará as pull requests e iterará sobre os resultados.

## Exercícios

| Exercício | Tópico | Descrição |
|----------|-------|-------------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e Codespace |
| [1. Instruções personalizadas][ex1] | Contexto | Adicione instruções personalizadas para orientar o agente na nuvem |
| [2. Agente na nuvem][ex2] | Agente assíncrono | Atribua issues ao agente na nuvem do Copilot |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revise e use agentes personalizados |
| [4. Gerenciar agentes][ex4] | Monitoramento | Monitore e oriente as sessões do agente |
| [5. Iterar][ex5] | Revisão | Revise PRs, itere sobre o trabalho do Copilot e escolha os próximos passos |

## Pré-requisitos

Antes de participar deste workshop, confirme que você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] O agente na nuvem do Copilot habilitado para sua conta ou organização

> [!TIP]
> Não tem um plano pago? Estudantes com status verificado podem usar o GitHub Copilot gratuitamente pelo [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui o agente, MCP, revisão de código e os recursos do Copilot CLI usados neste workshop — assim, você pode concluir todos os roteiros com esse plano.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> O agente na nuvem está disponível nos planos **Copilot Student, Pro, Pro+, Business e Enterprise** — em todos, exceto no Copilot Free. No Copilot Business ou Enterprise, um administrador precisa habilitá-lo para a organização.

> [!NOTE]
> Este roteiro não aborda MCP. Para aprender a usar servidores MCP com o Copilot, consulte o [roteiro do CLI][cli-workshop] ou o [roteiro do VS Code][vscode-workshop].

## Começar

**[Comece pelo Exercício 0: Pré-requisitos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli-workshop]: ../cli/
[vscode-workshop]: ../vscode/
