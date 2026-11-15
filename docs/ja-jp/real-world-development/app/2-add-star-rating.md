---
title: "レッスン 2 - 星評価を追加する: 手早く成果を出す"
description: "GitHub Copilot app で最初のエージェントセッションを開始し、ゲームカードに小さな変更を加えて、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認し、クイックチャットを試しました。次は **エージェントセッション**を開始し、プロジェクトに最初の変更を加えます。変更は小さく保ちましょう。ゲームのデータには星評価がすでにありますが、ホームページのゲームカードには表示されていません。エージェントに評価を表示させ、変更をレビューし、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- エージェントセッションを開始し、その構成を学びます。
- エージェントに、小さく焦点を絞った変更を依頼します。
- ワークスペースの diff ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを開いてマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価を設定できます。評価はゲーム詳細ページにはすでに表示されていますが、ホームページのゲームカードにはタイトル、カテゴリ、パブリッシャー、説明しか表示されていません。ウォームアップとして、エージェントに既存の評価を各カードに表示させます。最初のセッションにぴったりな、小さく独立した変更です。

## セッションの構成

**セッション**とは、独立したワークスペースで実行されるエージェントとの会話です。各セッションには専用の git worktree とブランチが割り当てられます。そのため、あるセッションで機能を追加し、別のセッションでバグを修正するなど、変更を競合させずに複数のセッションを同時に実行できます。セッションはサイドバーでリポジトリごとにグループ化されています。選択すると、そのセッションに切り替わります。

セッションには、エージェントとの**会話**、ファイルを調査・編集するエージェントの**ツール操作**、変更ファイルとその diff の一覧が表示されます。

## セッションを開始して変更を依頼する

プロジェクトを確認し、機能を実装するために新しいセッションを開始しましょう。[アプリのセットアップ][prior-lesson]で、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリに新しいセッションを作成し、変更を依頼します。

1. GitHub Copilot app に戻るか、アプリを開きます。
2. サイドバー上部の **+** を選択してセッションを開始します。
3. リポジトリとして `tailspin-toys` を選択します。
4. **new working tree** を選び、プロンプトボックスの下で **Interactive** モードを選択します。次のプロンプトで変更を依頼します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> キーを押して、Copilot にプロンプトを送信します。

Copilot app はまず新しい worktree を作成し、プロジェクトの分離されたコピーを用意します。次にプロジェクトを調査し、新機能を追加するために変更が必要なファイルを探して、コードを作成します。これで Copilot app を使って新機能を追加できました。

## diff をレビューする

AI が生成した変更は、どれほど小さくてもマージ前にレビューします。Copilot app で変更を確認しましょう。

1. アプリ右上の **Toggle review panel** を選択します。開いたパネルで **Changes** を選択し、変更されたファイルとその diff を確認します。

    ![Create PR の右側にある Toggle review panel ボタンを指す矢印が表示された GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲームの詳細を表示する中心的なファイル `GameCard.astro` にコードが追加されていることを確認します。評価がある場合は表示し、`starRating` が `null` の場合は「No rating yet」と表示する小さなブロックになっているはずです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot を含む生成 AI ツールは確率的に動作するため、上記とまったく同じコードが生成されるとは限りません。ただし、コードはおおむね同様のものになります。

## 変更を確認する

ブラウザーを開く前に、エージェントが実行した自動チェックの結果を確認します。数値の `starRating` と `null` の場合の代替表示をテストで確認していることを確認してください。前提条件が不足していたり、チェックがスキップされたりしている場合、合格とはみなせません。インストールの要求があれば、承認する前に内容を確認してください。

コードを読むだけで動作すると決めつけず、Copilot にウェブサイトを起動させ、ブラウザーキャンバスで確認しましょう。

> [!TIP]
> キャンバスは、Copilot app 内で直接使えるインタラクティブなウィジェットです。この後のレッスンではカスタムキャンバスを調べ、自分でも作成します。ここでは組み込みのブラウザーキャンバスを使います。

1. 次のプロンプトを使い、Copilot にアプリの起動とブラウザーキャンバスでのページ表示を依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくするとアプリが起動し、GitHub Copilot app 内にブラウザーが開きます。
3. 評価が設定されたゲームカードに、5 段階評価が表示されることを確認します。
4. 確認が終わったら、次のプロンプトで、このセッションのために起動した開発サーバーを停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初のプルリクエストを開いてマージする

機能を追加できました。次は、既存のコードベースに変更を取り込むプルリクエスト (PR) を作成しましょう。

1. 右上の **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、画面の指示に従って認証します。
3. PR の作成に一致する Issue が必要であると Copilot が説明した場合は、その手順に従うことを確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択して、レビュー欄に PR を表示します。ここで必要に応じて内容をレビューできます。
6. 準備ができたら **Ready to merge** を選択します。
7. 表示されたダイアログで **Merge pull request** を選択し、PR をマージします。

## まとめと次のステップ

おめでとうございます。GitHub Copilot app を使って最初の変更をリリースしました。このレッスンでは、次のことを行いました。

- エージェントセッションを開始し、その構成を学びました。
- ゲームカードに小さく焦点を絞った変更を加えるよう、エージェントに指示しました。
- ワークスペースの diff ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- 最初の PR を開き、チェック結果をレビューして、明示的にマージしました。

次は[フィルター機能の Issue から開始し、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を構築します。

## 参考資料

- [GitHub Copilot app でエージェントセッションを操作する][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
