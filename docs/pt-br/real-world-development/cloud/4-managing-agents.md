---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente na nuvem do Copilot que realizasse três tarefas diferentes para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente na nuvem trabalhe de forma assíncrona e autônoma, ainda é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente na nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse centro de controle, você vê todas as tarefas do agente que têm pull requests (PRs) abertas. Também pode explorar as operações realizadas e orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para monitorar as tarefas do agente na nuvem.
- orientar uma sessão em andamento e solicitar uma funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação de um modo de alto contraste, a equipe percebeu que seria um bom momento para adicionar também um modo claro. Como o agente já estava atualizando o estilo do site e adicionando opções de alternância, pareceu natural incluir esse recurso. Você orientará o agente para que ele adicione tanto o modo claro quanto o alto contraste.

## Revise as tarefas do agente na nuvem do Copilot

Vamos verificar o estado atual de todas as tarefas atribuídas ao agente na nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe as tarefas no painel principal e no painel esquerdo. A lista deve incluir as tarefas atribuídas ao Copilot, como:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em execução. Revise as operações realizadas pelo Copilot, que podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho do Copilot e a abordagem usada.

4. Observe também o painel de pull request que aparece à direita. Ele permite acompanhar a PR e os arquivos alterados.

## Oriente o agente na nuvem

Agora que você viu as tarefas em andamento, vamos pedir ao Copilot que inclua uma opção de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente ao modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado atual do trabalho.

   ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração terminou e o trabalho começou. Você verá mensagens semelhantes aos exemplos.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o prompt a seguir:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarefa do agente na nuvem na página de agentes com a caixa de diálogo para orientar a sessão destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento quando você enviou a mensagem.

Como o processo pode levar alguns minutos, aproveite para fazer uma pausa e refletir sobre o que aprendeu até aqui.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, o centro de controle para monitorar e orientar tarefas do agente na nuvem do GitHub Copilot. Nela, você acompanha tarefas ativas e concluídas, revisa o trabalho em andamento e redireciona sessões para ajustar o escopo ou fornecer outras orientações.

Você explorou estes conceitos:

- usar o centro de controle e a página de agentes para monitorar as tarefas do agente na nuvem.
- redirecionar uma sessão em andamento e solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar sobre as pull requests criadas pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
