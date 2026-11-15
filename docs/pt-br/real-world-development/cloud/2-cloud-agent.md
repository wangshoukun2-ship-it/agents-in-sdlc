---
title: "Exercício 2 - Agente na nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações não tenham problemas com dívida técnica. Pode ser uma questão de segurança não resolvida, código legado que precisa de atualização ou solicitações de recursos que ficaram no backlog por falta de tempo para implementá-las. O agente na nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele gera uma PR de rascunho para uma pessoa desenvolvedora revisar. Assim, é possível delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em questões mais importantes.

Neste exercício, você explorará como usar o agente na nuvem do Copilot para:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas questões de dívida técnica para resolver. As pessoas contratadas inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado: ela está completamente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. A página de detalhes de cada jogo deve mostrar jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem explorando o catálogo. A equipe ainda não precisa de uma implementação refinada; quer algo que possa usar para testes de aceitação da experiência. Esse trabalho está bloqueado, mas há outras issues de prioridade maior no momento.

Esses são exemplos de tarefas que podem ser adiadas com facilidade e são boas candidatas para atribuir ao agente na nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona enquanto a pessoa desenvolvedora se dedica a outras tarefas e, depois, voltar para revisar o trabalho e confirmar se está tudo certo.

## Conheça o agente na nuvem do GitHub Copilot

O [agente na nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano de forma semelhante a uma pessoa desenvolvedora. Assim como no trabalho em equipe, há várias formas de atribuir uma tarefa, inclusive [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois que recebe uma issue, o Copilot cria uma pull request de rascunho para acompanhar o progresso, prepara um ambiente e começa a trabalhar. Você pode examinar a sessão enquanto o trabalho está em andamento ou depois que terminar. Quando a solução estiver pronta para revisão, o agente mencionará você na pull request.

## A importância de instruções com escopo bem definido

Não há mágica no GitHub Copilot: não é possível deixar de pensar no problema. Mesmo operações que parecem simples podem ser complexas quando examinadas de perto. Por isso, [defina com cuidado o escopo das tarefas para o agente na nuvem][cloud-agent-best-practices]. Trate-o como um par de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a chegada da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já concluiu o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação. Se não, este é um bom momento para dar uma olhada rápida. Antes de atribuir trabalho ao agente na nuvem, leia os arquivos de instruções incluídos no repositório para observar o impacto deles mais adiante.

Abra os arquivos a seguir na interface Web do GitHub ou em um Codespace que já esteja em execução:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as expectativas sobre convenções TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente na nuvem na próxima seção, observe se a pull request contém comentários TSDoc, segue as convenções TypeScript e inclui cabeçalhos de comentário. Essas instruções vêm dos arquivos que você explorou. Voltaremos ao assunto ao revisar PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente na nuvem do Copilot

Criar código, independentemente de quem esteja trabalhando, geralmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de forma semelhante a uma pessoa desenvolvedora de software.

O agente na nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo criando um [fluxo de trabalho de configuração especial][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, que será executado antes de o agente começar. Assim, ele terá acesso às ferramentas de desenvolvimento e dependências necessárias. Esse fluxo já foi configurado para facilitar o laboratório e a experiência de aprendizagem. Ele disponibiliza Node.js, as dependências do projeto, os navegadores do Playwright para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para estas etapas. O Copilot receberá um token próprio para suas operações.
    permissions:
      # Para clonar o repositório durante as etapas de configuração, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se o repositório não for clonado nessas etapas, o Copilot fará isso automaticamente depois que elas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo: Node.js (o aplicativo agora é todo Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo se parece com qualquer fluxo de trabalho do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar na pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o fluxo manualmente na guia **Actions** do repositório. Isso é útil para confirmar que o fluxo funciona sem precisar esperar que o Copilot o execute.

## Adicione documentação

Todo mundo entende a importância da documentação, mas muitos projetos têm informações desatualizadas ou não têm documentação. Esse tipo de dívida técnica costuma não ser resolvido, reduzindo a produtividade e dificultando a manutenção da base de código e a integração de novas pessoas à equipe. Felizmente, o Copilot é muito útil para criar documentação, então esta é uma issue ideal para atribuir ao agente na nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse o repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina o **Title** como `Code lacks documentation`.
6. Defina a **Description** como:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

   ![Detalhes da atribuição da issue ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra a pull request (PR) criada recentemente, cujo título será semelhante a `[WIP]: Code lacks documentation`. Se a PR ainda não aparecer na lista, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer na PR. O Copilot está preparando o ambiente (executando o fluxo de trabalho mencionado anteriormente), analisando o projeto e definindo a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que serão executadas.
14. Percorra a linha do tempo da pull request. Você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a página para ver o indicador atualizado.

16. Percorra a sessão em tempo real para acompanhar como o Copilot resolve o problema. Ele explora o código e entende o estado do projeto, pausa para pensar e decidir a abordagem e cria código.

Esse processo provavelmente levará alguns minutos. Um dos objetivos principais do agente na nuvem do Copilot é executar tarefas de forma assíncrona, deixando você livre para trabalhar em outras coisas. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, em seguida, voltando nossa atenção à criação de funcionalidades para o aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Uma das principais vantagens do agente na nuvem do GitHub Copilot é poder dividir o trabalho: enquanto você se concentra em um conjunto de tarefas, o agente trabalha em outro. Talvez a seção de jogos relacionados solicitada pela equipe de design não leve muito tempo para implementar, mas você poderia usar esse tempo em outras tarefas. Vamos atribuí-la ao agente na nuvem.

1. Volte ao repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o template em branco.
5. Defina o **Title** como `Show related games on the game details page`.
6. Defina a **Description** como:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O emoji de olhos indica que o Copilot está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando na segunda solicitação. O agente na nuvem funciona de forma semelhante a uma pessoa desenvolvedora de software; por isso, não é necessário monitorá-lo continuamente. Você pode revisar o resultado quando ele terminar. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente na nuvem do GitHub Copilot][copilot-agents]. Com o agente na nuvem, é possível atribuir issues ao Copilot para que ele trabalhe de forma assíncrona. Você pode usá-lo para resolver dívida técnica, criar recursos ou ajudar na migração de código entre frameworks.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

Enquanto o agente na nuvem trabalha em segundo plano, vamos criar e usar agentes personalizados. O [agente na nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso às instruções personalizadas que exploramos nos módulos anteriores.

## Recursos

- [Sobre o agente na nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de trabalho de configuração do agente na nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
