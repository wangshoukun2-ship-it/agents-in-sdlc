---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルを使った自動チェックと、Playwright MCP を使ったブラウザー上のフィルター機能の確認を行いました。次は、明確に定義した QA プロセスを持つカスタムエージェントに、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用できる品質保証プロファイルを作成して調べます。
- QA エージェントを選択し、フィルター機能の Issue に照らして報告内容をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザー上の動作を一貫した方法で確認したいと考えています。カスタムエージェントを使うと、この QA プロセスをまとめて、再利用可能な報告を作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門エージェントです。プロファイルには目的、指示、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの規約を定義します。`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、要件を評価して結果を報告するために、それらの機能を Copilot がどう使うかを示します。QA プロファイルは、他の機能の代わりになったり、別の会話を要求したりするものではありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。プロファイルには、QA が行うチェックと守るべき境界の両方を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. デフォルトエージェントに、次のカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が QA のワークフローと境界を意図どおりに設定したかを確認します。これにより、確認だけが目的なのに、プロファイルが不完全だったり権限の範囲が広すぎたりするのを防げます。

1. `/diff` を実行し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は任意ですが、含めるとエージェントの表示名が明確になります。
3. プロファイルの指示を読み、QA が要件を起点とし、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に許可を求め、変更のコミットやプルリクエストの作成を行わないことを確認します。
5. これらの責務や制約が足りない場合は、続ける前にデフォルトエージェントへ修正を依頼します。

## Issue を基に QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイル作成後に同じフィルター機能の会話を再開し、QA を選ぶと、Issue と計画の内容を引き継いで確認できます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成した直後のエージェントは、利用可能なエージェントの一覧にまだ表示されない場合があります。その場合も、上記のコマンドで有効にできます。

2. 次のプロンプトを使い、QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業する様子を確認します。
4. 完了したら、報告を読みます。

## まとめと次のステップ

ワークフローを再利用できる専門的な役割としてまとめ、その結果を確認しました。このレッスンでは、次のことを行いました。

- カスタムエージェントと指示、スキル、MCP ツールとの連携を確認しました。
- 再利用可能な品質保証プロファイルを作成して調べました。
- QA エージェントを選択し、フィルター機能の Issue に照らして結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証結果がそろいました。次は[機能の PR を作成し、Agent Merge を使います][next-lesson]。

## 参考資料

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
