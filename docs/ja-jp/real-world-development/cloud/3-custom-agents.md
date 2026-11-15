---
title: "演習 3 - カスタムエージェント"
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
- Issue をカスタムエージェントに割り当てます。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。内容を確認して、Copilot への指示を理解しましょう。

Codespace に戻り、アクセシビリティ用エージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` を含むフロントマターは、カスタムエージェントに必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主な責務。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Issue を作成して割り当てる

Mission control は、環境内のエージェントを操作する中心的な場所です。Copilot cloud agent にタスクを割り当て、監視し、方向を変えるよう追加のガイダンスを与えられます。まず、ハイコントラストモードを追加するタスクを Copilot に割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue 作成ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

   ![カスタムエージェントの割り当てで Accessibility を選択する画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot はバックグラウンドでタスクを開始します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントは、特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの専門知識や規約を再利用可能なエージェントに記述し、Copilot の作業を導けます。

このレッスンでは、次のことを行いました。

- カスタムエージェントの定義方法を確認しました。
- エージェントにタスクを割り当てました。

Copilot がハイコントラストモードの実装を進める間に、次は [Mission control からエージェントのセッションを監視して方向付けます][next-lesson]。

## 参考資料

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使う準備をする][org-custom-agents]
- [Enterprise でカスタムエージェントを使う準備をする][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
