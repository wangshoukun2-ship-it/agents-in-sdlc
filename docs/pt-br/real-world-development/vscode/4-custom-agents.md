---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados em tarefas ou domínios específicos do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você pode fornecer instruções, práticas recomendadas, padrões de código e conhecimento de domínio específicos para orientar o Copilot. As equipes podem registrar seu conhecimento em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica as diretrizes [WCAG][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem um frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e ensinam a IA a executar tarefas. A maneira mais simples de diferenciá-los é: um **agente personalizado** é quem executa o trabalho; as **skills** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e podem orquestrar skills (e até outros agentes) durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade. Para fazer isso, ele poderia usar skills como uma skill de fluxo de trabalho de pull requests ou uma que execute e gerencie testes.

> [!NOTE]
> Não existe uma única maneira certa de criar um agente personalizado. Como em qualquer uso de IA, teste e itere para descobrir o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer garantir que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é essencial, queremos implementar esse recurso o mais rápido possível. Para isso, usaremos um agente personalizado.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar o modo de alto contraste.

## Revise o agente personalizado de acessibilidade

Já existe um agente personalizado para acessibilidade. Vamos revisar o arquivo para entender como ele orientará o Copilot.

Volte ao Codespace e, no terminal, mude para uma branch nova criada a partir de `main` para o trabalho de acessibilidade. Assim, a PR de filtragem do Exercício 3 permanecerá separada:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Leia as seções seguintes, que destacam:
   - As responsabilidades principais ao gerar código para um site acessível.
   - As práticas recomendadas de acessibilidade.
   - Exemplos de código HTML, CSS e JavaScript.
   - Armadilhas e erros comuns.

## Use o agente personalizado no Copilot Chat

O VS Code mostra todos os agentes personalizados definidos em `.github/agents` na lista de agentes na parte inferior da visualização Copilot Chat. Você pode selecionar um agente personalizado para aplicar as instruções e ferramentas dele à sessão de chat.

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios a seguir, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa informa o que selecionar quando necessário.

1. Na lista de agentes da visualização Chat, selecione **Agent** se ele ainda não estiver selecionado.

   ![Captura de tela mostrando o seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione a lista de agentes na parte inferior da visualização de chat. Ela mostra o agente ativo; por padrão, **default**.
3. Na lista, selecione **Accessibility agent**.
4. Envie o seguinte prompt ao agente de acessibilidade:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. O Copilot Chat começará a trabalhar: explorará a base de código, proporá mudanças e aplicará edições aos arquivos do projeto. Cada edição aparecerá no chat com o caminho do arquivo e um diff para revisão.
6. Conforme as edições forem aplicadas, o indicador **Files changed** será atualizado com os arquivos alterados pelo agente.

> [!NOTE]
> O processo provavelmente levará alguns minutos. O Copilot fará mudanças reais no repositório e editará arquivos existentes, como componentes Astro, CSS e testes relacionados.

Você revisará e orientará o trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents], assistentes de IA especializados em tarefas e domínios específicos. Com agentes personalizados, é possível registrar o conhecimento e os padrões da equipe em agentes reutilizáveis que orientam o Copilot.

Você explorou estes conceitos:

- como os agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [monitorará e orientará o trabalho do agente][next-lesson], revisando as mudanças à medida que são feitas e adicionando uma opção de modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar o recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
