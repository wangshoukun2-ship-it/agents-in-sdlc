---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの単純な更新でも、複数のファイルを変更し、テストを実行するなどの作業が必要になることがあります。開発者は通常、必要なファイルを探し、変更し、テストを実行して、デバッグし、更新し忘れたファイルを特定してから再度変更します。作業は次々に増えていきます。

ここで役立つのが Copilot エージェントモードです。

Copilot エージェントモードは、IDE 内でより自律的に作業します。開発者と同様に、既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正します。エージェントモードを使って、Web サイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途の機能です。エージェントモードは IDE 内で動作するため、すばやくフィードバックを得て対話しながら進められます。Cloud agent はチームメンバーのように非同期で作業し、Issue やプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot エージェントモードがプロジェクトを調べ、関連ファイルを見つけ、複数ファイルにまたがる変更を行う方法。
- GitHub Copilot エージェントモードが UI とデータレイヤーにまたがる新機能を実装する方法。
- Copilot エージェントモードが生成した変更とテストを、コードベースにマージする前にレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、利用者がカテゴリーとパブリッシャーで絞り込めるようにしたいと考えています。前の演習ではパブリッシャーヘルパーを追加しました。次は Copilot エージェントモードで、残りのデータレイヤー、UI、テストを完成させます。

## Tailspin Toys の Web サイトを起動する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を理解します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームを一覧表示し、詳細を表示できます。このアプリは単一の Astro アプリケーションで、ビルド時に静的 HTML を生成します。ページは Drizzle ORM を使ってフロントマターからローカルの SQLite データベースを直接クエリします。別のバックエンド API やクライアントサイドの UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、必要な操作は標準 DOM API を使う小さな Astro `<script>` で実装します。

### Web サイトを起動する

`npm run dev` タスクを実行すると、単一の Astro 開発サーバーが起動します。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後、`main` にいることを確認します。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Codespace 上の Web サーバーに接続するプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。

- **ホームページ**: タイトル、画像、説明とともにボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選ぶと、タイトル、説明、パブリッシャー、カテゴリーなどの情報を表示する詳細ページが開きます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**新しいチャット**を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

Web サイトの最初の実装は動作しますが、さらに機能を追加したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを作成すると、Issue のバックログも自動的に作成されます。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。エージェントモードでは、自律的に計画し、複数ファイルを変更し、ターミナルコマンドを実行してツールを呼び出せます。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高め、円滑に進めることが目的です。ただし、大規模言語モデルの動作は確率的なので、手順とは異なる提案が生成されることがあります。これは想定された動作です。

> [!NOTE]
> 大規模言語モデルの性質上、Copilot は別の MCP コマンドを使う場合がありますが、タスク自体は完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログの Issue を確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **続行**を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot は利用者にとって役立つと考える項目を優先順位付けしています。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に伝える指示ファイルを確認しましょう。Web サイトに機能を追加する際の考え方を記載した[ユーザーインターフェイス (UI) ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の進め方に関するガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などのファイル別指示へのリンク。

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルを基準とした相対パスです。指示を再利用し、大きく複雑な内容を分割し、例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の3種類の変更が必要です。

- データレイヤー (`src/lib/`) に残りの絞り込み処理を追加または調整します。
- フィルターの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

コードベースにマージする前に、テストを実行して成功することも必要です。Copilot エージェントモードに作業を依頼しましょう。

1. Copilot との現在の会話を続けるか、**新しいチャット**を選択して新しい会話を始めます。
2. **コンテキストの追加**、**指示**、**ui** の順に選択して指示ファイルを追加します。

   ![UI 指示ファイルを選択する画面の例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンから **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、目的の機能に関連するファイルを探す様子を確認します。データレイヤーのヘルパー、UI、テストを特定して変更を加え、テストを実行します。

   ![Copilot がプロジェクトファイルを調べている画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによっては、数分かかることがあります。作業中、エディターに **保持**と**元に戻す**が表示される場合があります。作業が完了すると、すべての変更に対して操作を選べるようになるため、進行中に選択する必要はありません。

7. Copilot に確認されたら、**続行**を選択してテストを実行します。

   ![テスト実行の確認を求める Copilot Chat のダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はタスクを完了してエラーがないと判断するまで、コード生成とテストを繰り返します。

   ![Copilot エージェントモードの Chat セッションが完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動したブラウザーに戻り、新機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **保持**を選択します。

## まとめと次のステップ

GitHub Copilot エージェントモードを使い、Tailspin Toys の Web サイトに機能を追加しました。この演習では、次のことを学びました。

- GitHub Copilot エージェントモードが UI とデータレイヤーにまたがる機能を実装する方法。
- Copilot エージェントモードがプロジェクトを調べ、関連ファイルを特定し、複数ファイルを変更する方法。
- Copilot エージェントモードが生成した変更とテストを、コードベースにマージする前にレビューする方法。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習: ページネーションを実装する

ゲーム一覧が増えていくと、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを実装するよう依頼します。次の点を検討してください。

- 既存の指示ファイルなど、プロジェクトのベストプラクティスに従います。
- ページサイズを利用者が選択できるようにするか、固定にするかなど、ページネーションの動作を検討します。
- 希望する実装に必要なガイダンスを Copilot に伝えるプロンプトを作成します。
- Copilot に変更やコンテキストを伝え、反復的に調整する必要がある場合があります。これは Copilot を使う通常のワークフローです。

## 参考資料

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
