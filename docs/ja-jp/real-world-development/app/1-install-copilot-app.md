---
title: "レッスン 1 - GitHub Copilot app をインストールする"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使うデスクトップアプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されています。リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。作業をすべて自分で行うのではなく、それぞれ分離された独自のワークスペースで複数のエージェントを並列に動かし、繰り返し作業を自動化するワークフロー向けに設計されています。Node.js をインストールし、プロジェクトのコピーを用意できたので、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートによって用意されたバックログを含め、ワークスペースを確認します。
- クイックチャットを試して、アプリについて学びます。

## シナリオ

チームは、増え続けるバックログに対応するために AI エージェントを導入しています。Copilot app では、Issue の選択、エージェントの実行、変更のレビュー、プルリクエストのマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise を使用している場合、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使用するには、まずアプリをインストールします。Windows、macOS、Linux 向けのバージョンが用意されています。アプリをインストールして認証し、Tailspin Toys リポジトリを追加しましょう。

1. ブラウザーで [GitHub Copilot app のランディングページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ランディングページに記載された手順でインストールします。
3. インストールが完了したら、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って GitHub.com または GHE.com のアカウントでサインインします。データ所在地に対応した GitHub Enterprise Cloud を使用している場合は **GitHub Enterprise Cloud** を選択し、求められたら `*.ghe.com` のホスト名を入力します。

   GitHub Enterprise Server を使用している場合は、まずクラウドアカウントでオンボーディングを完了します。その後、**Settings** を開き、**Accounts**、**Add account**、**GitHub Enterprise Server** の順に選択します。[Enterprise Server のサインインガイド][ghes-sign-in]に従い、ブラウザーでアプリを承認します。

5. オンボーディング中にリポジトリの選択画面が表示されたら、テンプレートから作成した Tailspin Toys リポジトリを選択します。リポジトリ選択をスキップして、オンボーディング後に追加することもできます。
6. テーマを求められたら選択し、オンボーディングを完了します。
7. サイドバーの **Sessions** でリポジトリを探します。すでに表示されていれば、再度追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで記録した所有者とリポジトリ名を使用します。
9. リポジトリがサイドバーに表示されることを確認します。ここで使用するレイアウトでは、`tailspin-toys` などのリポジトリフォルダーが **Sessions** の下にグループ化され、個々のセッションは各リポジトリの下に表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、アプリの使い方を確認しましょう。サイドバーには主に次の領域があります。

- **New** - 新しいチャットセッションを開始します。
- **Pull requests** - アプリの GitHub ネイティブ統合を通じてプルリクエストを表示します。アプリを離れずに PR の参照や絞り込み、CI 状態の確認、レビューを行えます。
- **Issues** - Issue を表示します。Issue の参照や絞り込み、Issue からのセッション開始を行えます。
- **Automations** — スケジュールまたはオンデマンドで実行する、保存済みのエージェントタスクです。To Do リストの管理、定期的なプロジェクトの保守、その他の面倒な作業を任せるのに役立ちます。まとめでは次のステップとして紹介します。このワークショップの演習には含まれません。
- **Customize** - MCP サーバー、プラグイン、スキルなどのコンポーネントを使って、Copilot app に機能を追加します。Playwright MCP の設定で使用します。
- **Chats** — ブランチや専用ワークスペースを必要としない、質問やブレインストーミングのための軽量な会話です。最初のチャットを開始すると表示されます。表示されない場合は、**Sessions** の横にある **+** を選択して最初のチャットを始めます。
- **Sessions** — リポジトリフォルダーと、その中のエージェントセッションをまとめます。`tailspin-toys` などのリポジトリ名を選択するとリポジトリのビューが開き、その下のセッションを選択すると会話に戻ります。セッションごとに分離されたワークスペースがあるため、変更が競合することなく複数のセッションを同時に実行できます。最初のセッションは星評価を追加する際に開始します。

ワークショップを進めながら、ワークスペースを確認していきます。

> [!TIP]
> 迷ったときは Copilot に質問しましょう。操作方法が分からない場合や、できることを知りたい場合は、Copilot に尋ねると案内してくれます。

### 用意されたバックログを確認する

バックログのないプロジェクトはほとんどなく、Tailspin Toys も例外ではありません。テンプレートからリポジトリを作成したときに用意されたバックログを確認します。

1. サイドバーの **Sessions** で、個別のセッションではなく Tailspin Toys のリポジトリフォルダーを選択します。リポジトリ上部のナビゲーションで **Issues** を選択します。
2. Issue 番号を決めつけず、次のタイトルを探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Issue を選択して詳細を読みます。各 Issue はエージェントセッションを開始する場所にもなります。小規模な最初の変更が終わったら、フィルター機能の Issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧には、Copilot app に追加したリポジトリの項目だけが自動的に表示されます。他のリポジトリの作業項目を確認するには、そのリポジトリもアプリに追加してください。

## クイックチャットを試す

アプリの使い方を学ぶには、アプリ自体について質問するのが効果的です。その用途には **クイックチャット**が適しています。クイックチャットでは、ブランチや worktree を作らずに質問やブレインストーミングができます。セッションを必要としない、すぐに終わる質問に最適です。

1. サイドバーの **Chats** の横にある **+** を選択し、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始します。チャットを開始すると **Chats** が表示されます。
2. アプリでセッションがどのように動作するかを尋ねます。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションが分離された独自の git worktree で実行されるため、変更が競合せずに複数のエージェントを並列実行できることが分かります。会話は続けることも、新しいチャットをいつでも開始することもできます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。次のことを学びました。

- アプリをインストールして GitHub にサインインしました。
- GitHub リポジトリからプロジェクトを追加しました。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つけました。
- クイックチャットで、すぐに答えが欲しい質問をしました。

次は[最初のエージェントセッションを開始し][next-lesson]、ゲームカードに星評価を表示します。

## 参考資料

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の使用を開始する][getting-started]
- [GitHub Copilot app でエージェントセッションを操作する][agent-sessions]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
