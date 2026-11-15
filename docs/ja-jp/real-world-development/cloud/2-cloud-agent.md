---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[| ← 前のレッスン: カスタム指示][previous-lesson] |

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装の時間がなくバックログに残った機能などがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフトのプルリクエスト (PR) を作成します。面倒な作業を委任して開発を加速し、より大きな課題に集中できます。

このレッスンでは、Copilot cloud agent を使って次のことを行います。

- コード生成に使う環境をカスタマイズします。
- 操作が安全に行われるようにします。
- 範囲が明確な Issue の重要性を確認します。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。サイトの初期バージョンを作成した開発者がドキュメントを十分に整備しておらず、ほとんど存在しません。まず、アプリケーションのエクスポート関数すべてに TSDoc のドキュメントコメントを追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリーの他のゲームを表示し、利用者が閲覧を続けられるようにします。洗練された実装はまだ必要ありません。UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、優先度の高い他の Issue があります。

どちらも、優先順位が下がりやすいタスクです。Copilot cloud agent に割り当てると、非同期で作業させることができます。開発者は別のタスクに集中し、後で Copilot の作業をレビューして期待どおりか確認できます。

## GitHub Copilot cloud agent とは

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様に、バックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]方法など、いくつかの方法で依頼できます。割り当てると Copilot は進捗を追跡するドラフトのプルリクエストを作成し、環境をセットアップしてタスクに取りかかります。セッションは進行中でも完了後でも確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。単純に見える操作にも複雑さが隠れています。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に作業し、学び、試し、適応させます。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot をガイドする方法を確認しました。カスタム指示の演習をすでに行っていれば、`.github/copilot-instructions.md` の動作を確認したはずです。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリにある指示ファイルを読み取り専用で確認し、後でその効果を確かめましょう。

GitHub の Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` に一致するファイルに適用されます。

次の演習では、`Code lacks documentation` という Issue を cloud agent に割り当てます。生成されたプルリクエストでは、これらの指示に基づく TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーを確認してください。後の演習で PR をレビューするときにも、改めて確認します。

## Copilot cloud agent の開発環境をセットアップする

開発者がコードを作成するときと同じく、タスクを実行するエージェントにも、必要な環境とセットアップが必要です。Copilot にタスクを割り当てる場合も、ソフトウェア開発エンジニアのように作業するための環境が必要です。

Cloud agent は、作業時の環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業前に実行するツールや依存関係をカスタマイズできます。このワークショップでは、学習に集中できるよう、あらかじめ設定されています。単一 Astro アプリ用に Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシードを適用した SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # セットアップ手順に必要な最小限の権限を設定します。Copilot は操作用の独自トークンを受け取ります。
    permissions:
      # 依存関係のインストールなどでセットアップ手順内からリポジトリをクローンする場合は、`contents: read` 権限が必要です。クローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
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

      # ローカルの SQLite データベースをマイグレーションし、データをシードします。ビルドとテストでデータを利用できます。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローファイルと同じですが、重要な点がいくつかあります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。これを使うと、Copilot の実行を待たずにリポジトリの **Actions** タブから手動でワークフローを実行し、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、古い情報しかなかったり、まったく整備されていなかったりするプロジェクトは少なくありません。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しいメンバーの参加を難しくします。Copilot はドキュメント作成に役立つため、cloud agent に依頼するのに適したタスクです。バックグラウンドでドキュメントを生成してもらい、後の演習で結果を確認します。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue 作成ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルのプルリクエスト (PR) を開きます。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後に、Copilot が To Do リストを作成したことを確認します。

> [!NOTE]
> Copilot の To Do リストが PR に表示されるまで数分かかることがあります。Copilot は先ほど説明したワークフローを実行して環境を作成し、プロジェクトを分析して、タスクの進め方を決めています。

13. リストを確認し、実行するタスクを調べます。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示を更新しないと、状態の更新が表示されない場合があります。

16. ライブセッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調べて状態を理解し、方針を考えて、コードを作成するまでの作業を確認できます。

作業には数分かかることがあります。Copilot cloud agent の主な目的は、非同期にタスクを実行して他の作業に集中できるようにすることです。次はその特徴を活用し、別のタスクを cloud agent に割り当ててから、アプリケーションに機能を追加する作業を進めます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の1つは作業を分けられることです。デザインチームが求める関連ゲームの表示は、それほど時間がかからないかもしれませんが、その時間を別の作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue 作成ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を始めたことを示す 👀 が Issue の最初のコメントに表示されます。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組み始めました。Copilot cloud agent はソフトウェア開発エンジニアのように作業するため、常に監視する必要はありません。完了後にレビューできます。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]を確認しました。cloud agent に Issue を割り当て、非同期で作業を実行できます。技術的負債の解消、新機能の作成、別のフレームワークからのコード移行などを依頼できます。

このレッスンでは、次のことを行いました。

- コード生成に使う環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- 範囲を明確にした Issue の重要性を学びました。
- Issue を Copilot に割り当てました。

Cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使います。[Copilot cloud agent は MCP サーバー][cloud-agent-mcp]も使用できます。また、前のモジュールで扱ったカスタム指示も利用できます。

## 参考資料

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
