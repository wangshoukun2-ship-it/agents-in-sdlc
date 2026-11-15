---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI の Playwright MCP を確認または設定して、ブラウザー上でフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

すでに説明したように、コードを書くことだけが開発ではありません。データや外部サービスを使い、Copilot が利用できる自動化機能を追加することもあります。そこで MCP サーバーが役立ちます。MCP サーバーを使うと、Copilot CLI に組み込まれている機能を超えて、ツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と、Copilot CLI での使い方を理解します。
- Playwright MCP サーバーが利用できない場合は追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

単体テストやエンドツーエンドテストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot が利用者と同じように作業中の Web サイトを使えるようにし、期待どおりの変更かを確認する作業を自動化します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部サービスとリアルタイムにやり取りし、最新情報へのアクセスや操作を行えます。

ツールやリソースには、AI エージェントと外部サービスの橋渡しをする MCP サーバーを通じてアクセスします。各 MCP サーバーは、エージェントが利用できるツールとリソースのセットを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ってブラウザーを自動操作できます。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくするため、[MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクトの他の依存関係と同様に扱ってください。利用前にソースコードを確認し、公開元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は追加し、Copilot が利用者と同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI に Playwright MCP がすでに設定されている場合は、セットアップを飛ばして次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力してから <kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認して <kbd>Enter</kbd> を押し、続けて <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Copilot に Playwright で機能を確認してもらう

前のレッスンでは、機能が期待どおり動作することを手動で確認しました。今度は Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot に MCP サーバーを使うよう伝えなくても、自動的に選択します。ただし利用するサーバーが分かっている場合は、明示しても構いません。結果の一貫性を高め、トークンを少し節約できます。

2. ブラウザーで機能を確認する手順が Copilot によって実行される様子を確認します。
3. 報告を読み、すべてが期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認しました。次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びました。
- 必要に応じて Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認しました。

次は、スキルとブラウザーツールを専門家の役割にまとめるため、[QA カスタムエージェントを作成します][next-lesson]。

## 参考資料

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
