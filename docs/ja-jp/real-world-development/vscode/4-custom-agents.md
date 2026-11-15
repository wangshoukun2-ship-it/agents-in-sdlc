---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、対象を絞った指示、ベストプラクティス、コーディングパターン、専門分野の知識を記述することで、特定の作業を効果的に実行するよう Copilot を導けます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、セキュアなコーディングを促すセキュリティエージェント、一貫したテストパターンを守るテストエージェントなど、チームの専門知識を再利用可能なエージェントにできます。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはユーザー全体に適用する `~/.copilot/agents` の Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割が重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業の方法を伝えます。分かりやすく言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキル (場合によっては他のエージェント) を調整して作業します。この演習のアクセシビリティ用カスタムエージェントは、アクセシビリティガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエストワークフロー用や、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントを作成する唯一の正解はありません。AI を使うほかの作業と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディングプラットフォームを使えるようにしたいと考えています。最近、現在のダークテーマはテキストと背景色のコントラストが不十分で、読みにくいという意見が寄せられました。このアクセシビリティの問題に対処するため、デザインチームは利用者が切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、カスタムエージェントを使って機能をすばやく作成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ用カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。内容を確認して、Copilot への指示を理解しましょう。

Codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用のブランチを作成します。演習 3 のフィルター機能の PR とは分けて作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` を含むフロントマターは、カスタムエージェントに必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主な責務。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェントのドロップダウンには、`.github/agents` に定義したすべてのカスタムエージェントが表示されます。カスタムエージェントを選ぶと、そのエージェントの指示とツールに合わせてチャットセッションが設定されます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**新しいチャット**を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

1. まだ選択されていない場合は、Chat ビューのエージェントのドロップダウンから **Agent** を選択します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部にあるエージェントのドロップダウンを選択します。現在のエージェント名が表示されます。既定の名前は **default** です。
3. 利用可能なエージェント一覧から **Accessibility agent** を選択します。
4. アクセシビリティエージェントに次のプロンプトを送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat はコードベースを調べて変更を提案し、プロジェクトファイルに適用します。変更はファイルパスと差分とともにチャットに表示されます。
6. エージェントが変更したファイルの一覧は **Files changed** の表示に反映されます。

> [!NOTE]
> この作業には数分かかることがあります。Copilot はリポジトリ内のファイルを実際に変更します。作業中に Astro コンポーネント、CSS、関連するテストなどが更新される場合があります。

次の演習で、進行中の作業をレビューして必要な方向を示します。

## まとめと次のステップ

このレッスンでは、[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントは、特定のタスクや分野に合わせて専門化した AI アシスタントです。チームの専門知識や規約を再利用可能なエージェントに記述し、Copilot の作業を導けます。

次のことを学びました。

- カスタムエージェントの定義方法。
- Copilot Chat のエージェントモードでカスタムエージェントを使う方法。

次は[エージェントの作業を監視して方向付け][next-lesson]、変更を確認しながら、同じセッションにライトモードの切り替えを追加します。

## 参考資料

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
