---
title: "レッスン 2 - 星評価を追加する: まずは小さな改善"
description: "Copilot CLI でゲームカードに小さな変更を加え、転送したブラウザーで確認して、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、次はプロジェクトに初めて変更を加えます。変更は小さく保ちましょう。ゲームのデータには星評価が含まれていますが、ホームページのゲームカードには表示されていません。エージェントに評価を表示させ、変更を確認し、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot との会話を始めます。
- エージェントに小さな変更を依頼します。
- `/diff` で変更をレビューします。
- 転送したブラウザーでアプリを起動して変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。一方、ホームページのゲームカードには、タイトル、カテゴリー、パブリッシャー、説明しか表示されていません。最初の練習として、各カードに既存の評価を表示するようエージェントに依頼します。範囲が小さく完結しているため、最初のセッションに適しています。

## 会話の構成

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot app とは異なり、通常の CLI 会話では、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使います。保存した会話は後で再開できますが、ファイルとブランチはディスク上の通常の Git 状態のままです。

会話では、プロンプトとエージェントの応答、ファイルを調査・編集する際のツール操作、`/diff` で確認できる変更の3つを扱います。

## 会話を始めて変更を依頼する

新しい会話を始めて、機能の実装に取りかかります。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. まだ Copilot を起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュコマンド `/new` を実行して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームの詳細を表示するファイルを探して、必要なコードを作成します。これで Copilot CLI を使った新機能の追加は完了です。

## 差分をレビューする

小さな変更でも、AI が生成した変更はすべてマージ前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を実行し、変更されたファイルをすべて確認します。
2. 評価がある場合は数値が、`starRating` が `null` の場合は `No rating yet` がゲームカードに表示されることを確認します。
3. どちらの状態もテストされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> Copilot を含む生成 AI ツールは決定論的ではなく確率的に動作するため、生成されるコードは例と異なる場合があります。特定の実装ではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動くと決めつけてはいけません。Copilot に Web サイトを起動してもらい、Codespaces で転送されたブラウザーから更新された UI を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**ブラウザーで開く**を選択します。
3. ゲームカードに5段階の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を実装できました。次はプルリクエスト (PR) を作成して、変更をプロジェクトにマージします。

1. デフォルトエージェントに変更のコミットを依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を実行します。Copilot CLI は PR の作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI が表示した URL を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して開きます。
4. 変更されたファイルとチェックをレビューします。
5. 問題がなければ **プルリクエストをマージ**を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、初めての変更をリリースしました。具体的には、次のことを行いました。

- 機能ブランチで Copilot との会話を始めました。
- ゲームカードへの小さな変更をエージェントに依頼しました。
- `/diff` で変更をレビューしました。
- 転送したブラウザーで星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は[フィルタリングの Issue から始めて、Plan モードと Autopilot モードを使う][next-lesson]ことで、より大きな機能を構築します。

## 参考資料

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
