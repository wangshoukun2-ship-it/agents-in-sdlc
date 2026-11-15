---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。[指示ファイル][instruction-files]を使うと、Copilot にしてほしいことだけでなく、その方法も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot にどう伝わるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい規約を追加します。

> [!NOTE]
> VS Code や CLI の演習とは異なり、ここでは指示の更新前後に同じプロンプトを実行しません。Copilot cloud agent は GitHub Issue に基づいて非同期で作業するため、リアルタイムで並べて効果を示すのが難しいためです。この演習では、後のプルリクエストをレビューするときに指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発方法に関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには、必ず単体テストを用意します。
- UI はダークモードで、現代的な印象にします。
- TSDoc のドキュメントコメントをコードに追加します。
- ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発方針に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝えられます。規約、ライブラリ、記述したいコメントの種類などを指定することで、より適切な提案やコード生成を促せます。リポジトリ全体に適用する指示も、特定のファイルに適用する指示も作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md`: リポジトリに関するすべてのリクエストで Copilot に送られる単一の指示ファイルです。多くの Copilot Chat や CLI のリクエストに関係する、技術スタック、開発中のものの概要、ベストプラクティス、全般的なガイダンスなどを記載します。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイルの種類に指示を適用します。TypeScript や Astro などの言語別ガイドライン、UI コンポーネントや単体テストの作成手順などを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には適用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方をコード生成に使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準形式の指示ファイル][custom-instructions-support]にも対応しています。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく扱うのは、この演習の範囲外です。サンプルプロジェクトの例から、一般的な方法を確認しましょう。

- `copilot-instructions.md` には、構築中のもの、プロジェクト構成、全体的なコーディング規約など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルは、単体テスト、Astro コンポーネント、データレイヤーなどのファイルの種類、または特定のタスクに関する指示に使います。
- 自然な言葉で、明確なガイダンスを記載します。コードをどのように書くべきか、または書くべきでないかの例を含めます。

指示ファイルや AI の使い方に唯一の正解はありません。プロジェクトやシナリオに合わせ、試しながら見つけてください。

> [!TIP]
> GitHub Copilot を利用するプロジェクトでは、指示ファイルを十分に用意すると役立ちます。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用のファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。VS Code の **チャットを構成 → エージェントの指示を生成**や、Copilot CLI の `/init` など、環境によって方法が異なります。必要な場合は、利用中の環境に関するレッスンで説明します。
>
> テンプレートや出発点を探している場合は、指示ファイルやカスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルが用意されています。エディターまたは GitHub の Web UI で開きましょう。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指示もあります。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これはリポジトリのルートからの glob を指定します。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
5. このプロジェクトで単体テストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ファイルの末尾にある他の指示ファイル (例: `unit-tests.instructions.md`) やプロジェクト内のファイルへのリンクを確認します。大きな指示を小さく再利用可能なファイルに分け、Copilot がコード生成時に参考にできる例を示せます。ここに記載されたパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約がありますが、コード内のドキュメントに関する要件はまだありません。次の手順で TSDoc のドキュメントコメントとファイルヘッダーコメントの規約を追加します。

## リポジトリ全体の新しい規約を追加する

この演習で行う編集は1つです。コードに TSDoc のドキュメントコメントとファイルヘッダーを追加するルールを、プロジェクト全体に適用される指示として追加します。後の演習で Issue を割り当てると、cloud agent がこの指示を使います。

編集の前に、作業用のブランチを作成します。Cloud agent は Issue の対象となるブランチの指示を読み取りますが、変更をコミットするためにブランチを使います。

1. Codespace のターミナルで、次のコマンドを実行してブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前に説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を伝えます。コードの提案を改善するために、リポジトリの規約を追加しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27行目付近にある **Code formatting requirements** セクションを探します。現在はコーディング規約が記載されていますが、生成したヘルパーにドキュメントコメントがないように、コード内のドキュメント要件がありません。
3. 既存の規約の直後に、ファイルヘッダーと TSDoc のドキュメントコメントを追加するため、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、指示ファイルは `.github/copilot-instructions.md` のようにリポジトリ全体へ適用することも、言語やファイルの種類、タスクごとに `*.instructions.md` を作成して適用することもできます。ドキュメントコメント規約のようなプロジェクト全体の基準は、リポジトリレベルのファイルに記載します。

## 指示の更新をコミット、プッシュしてマージする

Cloud agent は、Issue が対象とするブランチの指示ファイルを読み取ります。次の演習で Issue を Copilot に割り当てると、`main` からブランチを作成します。そのため、cloud agent が更新した指示を読み取れるよう、指示の変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で Issue を割り当てると、cloud agent はこの指示を参照します。

> [!TIP]
> ワークショップで `main` に直接作業したい場合は、ブランチ作成を省いて、変更を `main` にコミットしても構いません。実際のプロジェクトでの手順に合わせるため、このレッスンではブランチを使います。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルを参照する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の規約を追加しました。このワークショップの後半で cloud agent が作成するプルリクエストをレビューするときに、指示の影響を確認します。

次は [Copilot cloud agent に Issue を初めて割り当てます][next-lesson]。

## 参考資料

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示作成のベストプラクティス][instructions-best-practices]
- [Copilot 用のより良いカスタム指示を書く5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
