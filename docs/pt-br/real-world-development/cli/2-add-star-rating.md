---
title: "Lição 2 - Adicionar avaliações por estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena mudança nos cartões de jogos, revisar a diferença em um navegador encaminhado e mesclar sua primeira pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira mudança no projeto. Ela será pequena: os dados dos jogos já incluem uma avaliação por estrelas, mas os cartões da página inicial ainda não a mostram. Você pedirá ao agente que a exiba, revisará a mudança e a mesclará como sua primeira pull request.

Nesta lição, você vai:

- iniciar uma conversa focada em uma branch de recurso.
- pedir ao agente que faça uma pequena mudança no projeto.
- revisar a mudança com `/diff`.
- executar o aplicativo e confirmar a mudança em um navegador encaminhado.
- abrir e mesclar sua primeira pull request.

## Cenário

Cada jogo no Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. Os cartões de jogos na página inicial, porém, mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, você pedirá ao agente que exiba a avaliação existente em cada cartão — uma mudança pequena e independente, ideal para sua primeira sessão.

## Estrutura de uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal no CLI usa o repositório e a branch do Git atualmente selecionados no terminal, em vez de criar um worktree dedicado. As conversas salvas permitem retomar a mesma discussão, enquanto os arquivos e a branch continuam sendo o estado normal do Git no disco.

Em uma conversa, você verá três coisas: seus prompts e as respostas do agente, a atividade das ferramentas que o agente usa para explorar e editar arquivos e as mudanças que pode inspecionar com `/diff`.

## Inicie uma conversa e solicite a mudança

Vamos iniciar uma nova conversa para começar a implementar o recurso.

1. Volte ao Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver em execução, use este comando para iniciá-lo:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando slash `/new` e pressione <kbd>Enter</kbd>.
5. Use o prompt a seguir para solicitar a mudança:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explora o projeto, localiza os arquivos usados para exibir os detalhes dos jogos e cria o código necessário. Você acabou de adicionar um recurso com o Copilot CLI!

## Revise o diff

Todas as mudanças geradas por IA precisam ser revisadas antes da mesclagem, mesmo as pequenas. Vamos analisar as mudanças no próprio Copilot CLI.

1. Digite `/diff` e inspecione cada arquivo alterado.
2. Confirme que o cartão de jogo exibe a avaliação numérica quando ela existe e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes cobrem os dois estados.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija eventuais falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico, então o código exato pode variar. Revise o comportamento em vez de esperar uma implementação específica.

## Verifique as mudanças

Não basta ler o código e presumir que funciona. Vamos pedir ao Copilot que inicie o site para que possamos examinar a interface atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cartões dos jogos mostram as avaliações de zero a cinco.
4. Volte ao Copilot e peça que pare o servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abra e mescle sua primeira pull request

Você criou o recurso. Agora, abra uma pull request (PR) para mesclar o novo código ao projeto.

1. Peça ao agente padrão que faça commit da mudança:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. Ao criar a PR, o Copilot CLI envia o commit existente e mostra a URL da PR.
3. Abra a PR mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionada enquanto seleciona a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme a mesclagem.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira mudança usando o GitHub Copilot CLI. Você:

- iniciou uma conversa focada em uma branch de recurso.
- orientou o agente para fazer uma pequena mudança nos cartões de jogos.
- revisou a mudança com `/diff`.
- executou o aplicativo e confirmou a avaliação por estrelas em um navegador encaminhado.
- abriu e mesclou sua primeira pull request.

Em seguida, você começará pela [issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para desenvolver um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
