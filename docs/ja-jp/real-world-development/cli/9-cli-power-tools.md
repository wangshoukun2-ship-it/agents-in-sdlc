---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウドへの委任に使えるスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には操作に使えるスラッシュコマンドが多数あります。高度な機能、内部情報、追加の設定オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、ほかの便利なコマンドをいくつか確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションを書き出したり共有したりする方法を学びます。
- 並行作業、worktree、クラウドエージェントへの委任に使える任意のコマンドを確認します。

## シナリオ

CLI の主要なワークフローを終えました。ここでは、コンテキスト管理、モデルの切り替え、セッション共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組んでいると、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使って手動で確認や圧縮を行うこともできます。

1. Codespace に戻り、Copilot CLI が起動していなければリポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように割り当てられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を実行して結果を比較します。会話がすでに短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。タイミングを選びたい場合は `/compact` を使います。関連のないタスクに切り替えて新しい会話を始めたい場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者の好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示して、使用するモデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学びを共有すると、AI ツールをより効果的に活用できます。`/share` コマンドを使うと、セッションを Markdown ファイルや HTML ファイルに書き出したり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を実行し、インストール済みのバージョンで利用できる `/share` のオプションを確認します。
2. セッションを共有する場合は、用途に合った出力先を選びます。たとえば `/share file` はローカルに Markdown として書き出します。
3. 書き出した内容を誰かに送信したり公開したりする前に確認します。セッションの書き出しには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容を公開しないでください。

## オプション: 作業を拡張または委任する

ワークショップの主要な部分は完了しました。Copilot CLI には、大きなタスクに使えるコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分配して並行実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、タスクを Copilot cloud agent に送信します。エージェントは非同期で作業し、プルリクエストを開くことがあります。

これらのコマンドは追加の worktree やリモート作業を作成する場合があるため、任意の機能です。試すときは、範囲を明確にした新しいタスクから始め、通常の手順で結果をレビューしてください。非同期エージェントの作業を詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定を変更したり、共有したり、内部の状況を確認したりできます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使ってコンテキスト管理を確認しました。
- `/model` を使って利用可能なモデルを確認しました。
- `/share` でセッションを書き出したり共有したりする方法を学びました。
- 並行作業、worktree、クラウドエージェントへの委任に使える任意のコマンドを確認しました。

ほかにも多くのスラッシュコマンドがあります。次は、これまでに学んだことと、学習を続けるための方法を[振り返ります][next-lesson]。

## 参考資料

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
