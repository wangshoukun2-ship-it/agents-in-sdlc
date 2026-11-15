---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことだけが開発ではありません。手動でコードの動作を確認し、指示ファイルで規約に従っていることも確かめました。では、テストや lint、継続的インテグレーション (CI) の他の作業はどうでしょうか。

このようなタスクには、**エージェントスキル**が適しています。スキルを使うと、このような処理を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys は、単体テスト、lint、型チェックに `quality-checks` スキルを使っています。チームでは、結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルは、エージェントが必要に応じて読み込む、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめます。基本的には、スキル名が付いたフォルダーと、その中の Markdown ファイル `SKILL.md` で構成されます。Markdown には、スキルの内容を定義する `name` と `description` のフロントマター、機能の概要、呼び出すタイミングのガイダンスを記載します。スキルが使用するスクリプトや他のリソースをサブフォルダーに含めることもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトに既存のコマンドを使うため、`SKILL.md` のみで構成されています。

スキルをプロジェクトの `.github/skills` に配置すると、チームで共有・再利用できるリポジトリ資産になります。また、ユーザーのスキル用フォルダー `~/.copilot/skills` に配置することもできます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェックの実行用に作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイル先頭にある `name` と `description` を読みます。description によって Copilot がスキルを呼び出すタイミングを判断することを確認します。
3. 指示を読み、テストと lint の手順をどう案内するか確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

Copilot CLI から直接、または自然言語でスキルを呼び出せます。3種類のチェックを実行するよう Copilot に依頼します。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行したチェック、成功したかどうか、ツールから報告された内容を示す、より分かりやすいレポートに変更します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイル末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更したスキルを実行してみましょう。Copilot CLI は会話を再起動せずに、編集されたスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 変更前と**同じプロンプト**を使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されたレポートを読み、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使いました。このレッスンでは、次のことを行いました。

- 単体テスト、lint、型チェックを実行する `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、機能の PR に含めます。次は [Playwright MCP サーバーを使って][next-lesson]、Copilot がサイトを直接操作できるようにします。

## ほかのスキルの例

コミュニティが公開している次の例は参考資料です。追加の演習ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と同梱のエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
