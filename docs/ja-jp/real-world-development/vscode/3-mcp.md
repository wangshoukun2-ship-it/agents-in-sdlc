---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動くことを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストしてもらいましょう。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot に新しいツールを提供する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーでフィルター機能を手動テストしてもらいます。
- ブランチを公開して、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部に接続できると、さらに便利になります。Copilot が外部ツールやサービスと通信するための標準的な方法が、Model Context Protocol (MCP) です。

![エージェントモードの構成要素と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信する仕組みです。MCP を使うと、AI エージェントはリソースを使って最新情報にアクセスしたり、ツールを使って利用者に代わって操作を実行したりできます。

ツールやリソースには、AI エージェントと外部サービスの橋渡しをする MCP サーバーを通じてアクセスします。MCP サーバーは既存の API や npm パッケージなどのローカルツールとの通信を管理します。各 MCP サーバーは、エージェントが利用できるツールとリソースのセットを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザーの自動操作機能を提供します。Web ページを開き、フォームに入力し、ボタンを選択するなどの操作をエージェントが実行できます。

ほかにもさまざまなツールやリソースを利用できる MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、貢献できるようにするため、[MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクト内の他の依存関係と同様に扱ってください。利用前にソースコードをよく確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密情報や重要な操作へのアクセスを許可する際は注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されています。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクトのテンプレートに含まれています。そのため、前の演習では Copilot が Issue のバックログを読み取れました。これは[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルにインストールする必要はなく、VS Code が HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを利用できるため、機能のテストに適しています。

1. `.vscode/mcp.json` を編集し、`github` の隣に `playwright` エントリを追加します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行してサーバーを起動します。`--headless` フラグは、表示用のデスクトップ環境がない Codespace でも実行できるよう、Playwright がブラウザーウィンドウを表示しない設定にします。

> [!NOTE]
> Tailspin Toys ではエンドツーエンドテストにも Playwright を使っているため、必要なブラウザーはすでにインストールされていることが多いです。後で Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot がサーバーのツールを初めて必要としたときに、VS Code がサーバーを起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するかどうか尋ねられた場合は、設定を確認して、サーバーを起動できるよう信頼を選択します。

手動で起動する必要はありません。次の手順で Copilot がツールを初めて必要としたときに、VS Code がサーバーを起動して信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作して変更を確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して、その結果を読み取って要約します。会話を続けながら、機能が期待どおりに動作するかをすばやく確認できます。

Playwright MCP サーバーは、スクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使って操作します。ボタン、リンク、リスト項目など、ラベル付きの構造化された要素を基にエージェントが判断します。支援技術と同じように情報を扱うため、機能の確認がアクセシビリティの簡易チェックにもなります。

サーバーを接続し、アプリを起動した状態で、次のプロンプトを Copilot に送信してフィルター機能を確認します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。報告と Issue の受け入れ条件を照らし合わせます。問題がある場合は、追加の質問をするか、プルリクエストを開く前にコードの修正を依頼します。

> [!NOTE]
> このテストには、`http://localhost:4321` でアプリを起動しておく必要があります。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動してください。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーのダウンロードが必要な場合があります。ブラウザーがないと報告された場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームのレビュー用にプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **ソース管理**パネルを開き、Copilot が変更したファイルを確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![Copilot が変更したファイルを表示するソース管理パネル](../../../_images/ex3-source-control-changes.png)

4. **公開**を選択し、リポジトリにブランチをプッシュします。

## プルリクエストを作成する

プルリクエストを作成する方法には、github.com や GitHub コマンドラインインターフェイス (CLI) を使う方法などがあります。ここでは GitHub Copilot に作成を依頼します。関連する Issue を探し、関連付けた PR を作成できます。

1. Copilot Chat パネルを開き、**新しいチャット**を選択して新しいセッションを始めます。
2. GitHub のツールを使えるよう、エージェントのドロップダウンから **Agent** が選ばれていることを確認します。
3. 次のプロンプトを使って PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 情報の収集や操作の実行を Copilot に許可するため、必要に応じて **続行**を選択します。GitHub ツールを初めて使うときは、GitHub への認証を求められることがあります。表示される手順に従って認証します。
5. Copilot が Issue を検索し、適切なものを見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択してプルリクエストをレビューします。**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot に新しいツールを提供する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- 出荷前に、ブラウザーでフィルター機能を手動テストしました。
- ブランチを公開して、フィルター機能のプルリクエストを作成しました。

MCP サーバーを使って機能を**テスト**しました。MCP は Copilot を拡張する方法の1つです。次は、アクセシビリティのレビューなど、対象を絞ったタスクを効率化する[カスタムエージェントを作成します][next-lesson]。

## 参考資料

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP レジストリ][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
