---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、Copilot cloud agent にユーザー体験の改善と機能追加に関する3つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェントページ][agents-page]など、さまざまなツールで管理できます。この Mission control では、オープンなプルリクエスト (PR) を持つすべてのエージェントタスクを確認できます。実行内容を調べ、進行中のセッションに追加のガイダンスを送ることもできます。

このレッスンでは、次のことを行います。

- エージェントページを開き、cloud agent のタスクを監視します。
- 進行中のセッションに指示を送って、機能を追加します。

## シナリオ

チームは、エージェントにハイコントラストモードを追加するよう依頼した後、ライトモードも必要だと気付きました。サイトのスタイルとモード切り替えをすでに変更しているため、同じ作業に含めるのが合理的です。エージェントにライトモードも追加してもらいましょう。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインの両方にあるタスク一覧を確認します。次のような、Copilot に割り当てたタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲームを変更する API を生成する。
   - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを1つ選び、Copilot が実行した作業を確認します。たとえば、次のものがあります。
   - リポジトリのコードをチェックアウトする。
   - Copilot が作業する環境を作成する。
   - MCP サーバーをセットアップする。
   - タスクを完了するための各種手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や実施した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここでは、PR と変更されたファイルを確認できます。

## Cloud agent に指示を送る

実行中のタスクを確認したので、ハイコントラストモードを実装している Copilot にライトモードも追加するよう依頼します。

1. ハイコントラストモードを追加しているセッションを選択します。Copilot が付けた名前や作業状況によって、タイトルは異なります。

   ![Mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まるまで、セッションを数分間確認します。以下のようなメッセージが表示されたら作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![エージェントページで、Copilot 作業中のセッションへの追加指示ダイアログを示す画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを認識して、作業に含める様子を確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行中だった手順が終わると、追加の依頼をワークフローに反映します。

今回も数分かかります。これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot cloud agent の監視とガイダンスの中心となる、Copilot agents ページを確認しました。Mission control では、実行中または完了済みのタスクを追跡し、実行内容を確認し、進行中のタスクに指示を加えて範囲を調整できます。

このレッスンでは、次のことを行いました。

- Mission control とエージェントページを使って、cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装する間に、次は Copilot が作成した[プルリクエストを反復して改善します][next-lesson]。

## 参考資料

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
