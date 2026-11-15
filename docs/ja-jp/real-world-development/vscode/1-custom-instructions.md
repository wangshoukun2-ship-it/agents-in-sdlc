---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。Copilot にコード生成の指針を伝える主な方法の1つが、[指示ファイル][instruction-files]です。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot にどう伝わるかを確認します。
- 現在の指示を適用した状態で、フィルター機能の最初のデータ部分 (パブリッシャーヘルパー) を生成します。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の規約を追加します。
- 同じプロンプトを再実行し、生成コードが新しい規約に従う様子を確認します。
- 指示の更新とフィルター機能の作業を `main` にコミットし、次の演習で Copilot が更新したガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが、設定した規約の一部に従わないことがあります。Copilot の動作は確率的です。この演習では、更新前後のコードが文字単位で一致するかではなく、指示を更新した後に動作がどのように変化するかを確認します。

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

## 変更用のブランチを作成する

ベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**新しいチャット**を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを適用した状態でプロンプトを送信します。その後、指示を更新し、同じプロンプトをもう一度送信して違いを見ます。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを変更します。多くの場合、ヘルパーファイルとテストの両方が対象になります。
6. 生成されたヘルパーが、型付きの関数として `db` クライアントを最初の引数に取り、パブリッシャーの型付き配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 生成されたコードには、TSDoc のドキュメントコメントとファイルヘッダーコメントが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は確率的なので、指示がなくてもドキュメントコメントが追加される場合があります。その場合も問題ありません。この演習では、指示を更新した後に一貫性が向上することを確認します。

## 新しいリポジトリ規約を追加する

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
> 前のレッスンで説明したように、指示ファイルは `.github/copilot-instructions.md` のようにリポジトリ全体へ適用することも、言語やファイルの種類、タスクごとに `*.instructions.md` を作成して適用することもできます。ここで追加したドキュメントコメントの規約のような、プロジェクト全体の基準はリポジトリレベルのファイルに記載します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**新しいチャット**を選択して会話をクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot の対象ファイルを明確にします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 生成されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 生成された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト規約に従うよう Copilot を導き、次の演習で利用する実際のコードに適用しました。

## 指示とブランチをコミットしてプッシュする

指示ファイルも、リポジトリ内のほかのファイルと同じようにバージョン管理します。変更をコミットしてプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次を実行し、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基礎として Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加ファイルが表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 次のコマンドでブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント規約の Issue に関連付けてプルリクエストを作成します。GitHub Copilot に Issue を検索し、その Issue を完了する PR を作成してマージするよう依頼します。

> [!NOTE]
> Copilot が GitHub 上で操作するのは、この演習が初めてです。プロジェクトのテンプレートには GitHub のツールがあらかじめ設定されているため、追加のセットアップは不要です。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインを求められることがあります。プロンプトに従って認証してください。この接続がどのように動作するかは、後の演習で Model Context Protocol を使って学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントの選択で **Agent** が選ばれていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成して、指示の更新とパブリッシャーヘルパーの基礎の両方を記載するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を検索して PR を作成します。
5. Copilot が GitHub の操作を許可するよう求めたら、コマンドを確認して、適切な場合に **許可**を選択します。
6. PR が作成されたら、次のプロンプトを使ってマージし、ローカルのブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作とシェルコマンドの実行を許可するよう求めたら、コマンドを確認して、適切な場合に **許可**を選択します。

GitHub Copilot を使ってプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルを Copilot が参照する方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- 既存の指示を適用してコード生成プロンプトを送り、Copilot が生成する内容を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の規約を追加しました。
- 同じプロンプトを再実行し、生成コードが新しい規約に従う様子を確認しました。
- 指示とヘルパーの基礎を `main` にコミットし、次の演習で利用できるようにしました。

次は、[エージェントモード][next-lesson]で指示を適用し、Copilot にコードベース全体にまたがる機能を追加してもらいます。

## 参考資料

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示作成のベストプラクティス][instructions-best-practices]
- [Copilot 用のより良いカスタム指示を書く5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
