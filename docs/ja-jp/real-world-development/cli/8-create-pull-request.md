---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて1つのブランチに保存されています。まとめてレビューし、プルリクエストを開きましょう。星評価のプルリクエスト (PR) は手動でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は、機能、指示、スキル、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、フィルター機能と品質ワークフロー全体を1つの会話とブランチにまとめ、この PR に含めています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージまでの作業を自動化する方法を学びます。
- 機能全体の変更と検証結果を確認します。
- フィルター機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフロー全体で、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージの許可を開発者が管理しながら、PR の残りの作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージ可能にするために残っている作業を自動化します。有効にすると、Copilot は PR をブロックしている項目を確認し、失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースなどを行います。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **プルリクエストをマージ**していました。Agent Merge に任せることもできます。Agent Merge を有効にする前に、変更をレビューして問題がないことを確認してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、まとめてレビューし、PR を作成して、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 一覧から **Default** を選択して <kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを実行して PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションの状況を監視します。すべて成功すると、マージを実行します。
7. Copilot から「PR #14 was squash-merged successfully.」などのメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可されていれば、レビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成やテスト、検証に加えて、プルリクエストのプロセスも自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージまでの作業を自動化する方法を学びました。
- 機能全体の変更と検証結果を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使える[便利な Copilot CLI のスラッシュコマンド][next-lesson]を確認します。

## 参考資料

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
