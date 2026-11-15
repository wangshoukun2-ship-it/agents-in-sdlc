---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメントの規約を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。作業の方法に決まりがある場合、そのガイダンスを Copilot が利用できるようにしましょう。[指示ファイル][instruction-files]には、求めるコードだけでなく、その構成方法も記述できます。フィルター機能を構築したので、Copilot が参照した指示を確認し、ドキュメントの規約を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスに応じた指示ファイルがエージェントにどう適用されるかを確認します。
- コーディング規約が守られるよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発方法に関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントはコードの内容を繰り返すのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数は、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。該当する場合は、注入可能な `db` 引数も記載します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連コードが変わったらコメントも更新します。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの開発方針に沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝えられます。規約、ライブラリ、記述したいコメントの種類などを指定することで、より適切な提案やコード生成を促せます。リポジトリ全体に適用する指示も、特定のファイルに適用する指示も作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md`: リポジトリに関するすべてのリクエストで Copilot に送られる単一の指示ファイルです。多くのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイルの種類、タスクに関するガイダンスを記載します。

> [!NOTE]
> その他の指示形式への対応状況は、環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポート状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには指示ファイルが用意されています。変更前に内容を確認して、影響を見てみましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドは、指示が適用されるファイルの glob を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存プロジェクトファイルへのリンクを確認します。これにより大きな指示を小さく再利用可能なファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは出発点として十分ですが、TypeScript に新しく追加するコードへの TSDoc コメントが求められていません。`copilot-instructions.md` を更新しましょう。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクションを探します。およそ35行目にあります。
2. セクションの最後に、次の箇条書きを追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を反映するため、会話を終了して再開します。これにより、機能のコンテキストは維持されます。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して終了します。会話は自動保存されます。
2. リポジトリのルートで `game-filters-cli` ブランチをチェックアウトした状態で、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示された場合はフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を実行して、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、内容がコードを正しく説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示を参照する方法を確認し、新しい規約を機能に適用しました。次は、[再利用できる quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実施します。

## 参考資料

- [Copilot CLI 用のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート状況][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
