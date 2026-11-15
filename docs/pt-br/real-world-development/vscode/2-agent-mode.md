---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo as atualizações mais simples de um aplicativo normalmente exigem mudanças em vários arquivos e operações como executar testes. Em geral, a pessoa desenvolvedora precisa localizar todos os arquivos necessários, fazer as mudanças, executar testes, depurar, descobrir qual arquivo ficou de fora, fazer outra atualização e assim por diante.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para trabalhar com mais autonomia no IDE. Ele funciona de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo de agente para adicionar funcionalidades ao site.

> [!NOTE]
> Embora tenham nomes semelhantes, o modo de agente e o agente na nuvem foram criados para experiências diferentes. O modo de agente executa tarefas no IDE, permite ciclos rápidos de feedback e interação. O agente na nuvem trabalha de forma assíncrona, como uma pessoa da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar mudanças.
- o modo de agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados.
- revisar mudanças e testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem os jogos por categoria e editora. No exercício anterior, você adicionou um helper de editoras. Agora, concluirá o trabalho restante na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Execute o site Tailspin Toys

Antes de fazer mudanças, vamos explorar o site Tailspin Toys para entender o que ele oferece atualmente.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de programação. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro, que renderiza as páginas como HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, por meio do Drizzle ORM. Não há uma API de back-end separada nem um framework de interface do lado do cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com um pequeno `<script>` do Astro, com escopo limitado, que usa APIs padrão do DOM.

### Inicie o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento Astro. Siga estas etapas para executá-la no GitHub Codespace:

1. Volte ao Codespace. Você deve estar na branch `main`, depois de concluir o exercício anterior.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie uma branch e mude para ela para trabalhar na filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute este comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, você verá um aviso com a URL, semelhante ao exemplo:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo perguntar se você quer abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando o **x**.

5. Abra o site mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionada enquanto seleciona o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um Codespace, ao selecionar o link local no terminal, você será redirecionado automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o Codespace, que agora hospeda o servidor Web.

### Explore o site

Com o site em execução, explore as funcionalidades. Os principais recursos incluem:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você abre uma página com mais informações, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios a seguir, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa informa o que selecionar quando necessário.

A implementação inicial do site funciona, mas queremos adicionar novos recursos. Vamos começar revisando o backlog. A configuração do projeto a partir do template criou automaticamente um conjunto de issues. Peça ao GitHub Copilot que as mostre.

1. Na lista de agentes da visualização Chat, selecione **Agent**. O agente planeja e implementa mudanças em vários arquivos de forma autônoma, executa comandos de terminal e chama ferramentas.

   ![Captura de tela mostrando o seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> As pessoas que criaram este workshop não estão recomendando um modelo específico. Durante a criação do workshop, usamos Claude Sonnet 4.5 e o incluímos nas instruções para tornar as sugestões de código mais consistentes e a experiência mais previsível. Como os LLMs são probabilísticos, as sugestões podem ser diferentes das descritas no workshop. Isso é normal e esperado.

> [!NOTE]
> Como os LLMs são probabilísticos, o Copilot pode usar um comando MCP diferente, mas ainda deverá conseguir concluir a tarefa.

3. Peça ao Copilot que mostre o backlog de issues com este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para permitir que o comando liste todas as issues.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere o código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [instruções da interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No Codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - Uma visão geral da arquitetura.
   - Princípios de design de componentes, testabilidade e acessibilidade.
   - Links para instruções específicas para vários tipos de arquivo, incluindo:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Assim, é possível reutilizar e dividir conjuntos complexos de instruções e incluir exemplos e templates para o Copilot.

## Implemente a filtragem

Para concluir a filtragem, são necessárias pelo menos três atualizações diferentes no aplicativo:

- Adicionar ou aprimorar a lógica restante de filtragem na camada de dados (`src/lib/`).
- Adicionar ou atualizar testes do comportamento de filtragem.
- Atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes de mesclar as mudanças à base de código. O modo de agente do Copilot pode executar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue na conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** para adicionar o arquivo de instruções.

   ![Captura de tela mostrando como selecionar o arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Na lista de agentes da visualização Chat, confirme se **Agent** continua selecionado.

   ![Captura de tela mostrando o seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** continua selecionado como modelo.
5. Use este prompt para pedir ao Copilot que implemente a funcionalidade com base na issue relacionada do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relacionados à funcionalidade, tanto na camada de dados quanto na interface e nos testes. Em seguida, ele alterará os arquivos e executará os testes.

   ![Captura de tela mostrando o Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e rodar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, talvez apareçam os botões **Keep** e **Undo** no editor. Quando o Copilot terminar, você verá uma opção **Keep** ou **Undo** para cada mudança; não é necessário selecioná-las enquanto o trabalho está em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela da solicitação para executar testes no painel Copilot Chat](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e até falhas de teste durante o processo. Não há problema: o Copilot alterna entre geração de código e testes até concluir a tarefa sem detectar erros.

   ![Captura de tela de uma sessão completa do modo de agente no Copilot Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador em que o site está em execução e explore o novo recurso.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, você explorou como usar o modo de agente do GitHub Copilot para adicionar funcionalidades ao site Tailspin Toys. Você aprendeu como:

- o modo de agente do GitHub Copilot pode implementar recursos na interface e na camada de dados.
- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar mudanças.
- revisar mudanças e testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

Agora, vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir uma pull request.

### Exercício extra de exploração — implementar paginação

À medida que a lista de jogos crescer, será necessário adicionar paginação. Use o que aprendeu neste exercício e peça ao Copilot para atualizar o site. Considere:

- seguir as práticas recomendadas existentes, inclusive as instruções dos arquivos do projeto.
- decidir como implementar a paginação: permitir que o usuário escolha o tamanho da página ou defini-lo no código.
- orientar o Copilot com um prompt que contenha as informações necessárias para a implementação desejada.
- iterar com o GitHub Copilot, solicitando mudanças e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
