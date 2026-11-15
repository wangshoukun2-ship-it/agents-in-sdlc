---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、GitHub Copilot とともにユーザー体験の改善や機能追加に取り組みました。コードにドキュメントを追加し、デザインチームが検討する関連ゲーム機能を構築し、ハイコントラストモードとライトモードを含むアクセシビリティ機能を実装するよう Copilot に依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体のコードレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関して GitHub Copilot が提案した内容を確認します。

## GitHub Copilot cloud agent とセキュリティ

Copilot cloud agent は非同期に、監督者なしでタスクを実行するため、安全性を保つための制約があります。

- Copilot はリポジトリに対する読み取りアクセスと、作業用ブランチに対する書き込みアクセスのみを持ちます。
- Cloud agent は GitHub Actions 内で動作し、作業用に分離された一時的な環境を作成します。
- GitHub Actions のワークフローを実行するには、人間による承認が必要です。
- 既定では、[外部リソースへのアクセス][agent-firewall]は制限されています。MCP サーバーへのアクセスも制限の対象です。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加をレビューします。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するときに、GitHub Copilot がファイアウォールでブロックされているという警告が表示される場合があります。Copilot は MCP サーバーへのアクセスなど、既定では外部リソースへのアクセスが制限されているため、**想定された動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot が作業中の場合、プルリクエストには **[WIP]** が付いています。その場合は Copilot が完了するまで待ちます。数分かかることがあるため、その間に休憩したり、学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選び、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメントコメントやその他のドキュメントが追加されたコードを確認します。具体的な変更内容は、実行によって異なります。

   変更を確認するときは、TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーに注目してください。これらは演習 2 の最初に確認したカスタム指示ファイルによるものです。

6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが登録されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot とのプルリクエスト作業は一方通行ではありません。チームメンバーと同様に、プルリクエストのコメントやコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確率的に変わるため、依頼する文章を厳密に指定する必要はありません。次のような変更を提案できます。

- 各コードファイルの先頭に、役割を説明するコメントヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. **@copilot** をメンションし、生成されたドキュメントへの変更を依頼します。上記の例から選ぶか、コードベースに追加してほしいドキュメントについて別の提案をします。
2. **View Session** を選択して、Copilot が作業する様子を確認します。Copilot が更新用の新しいセッションを開始します。
3. プルリクエストに戻るには **Back to pull request** を選択します。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の変更が完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更をレビューします。

納得できるまで反復して改善できます。問題がなければ、PR をドラフトからレビュー可能な状態に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態に変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちになっていることを示す表示を探します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローが登録されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動テストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に `<branch-name>` を置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。前の PR と同様に、Copilot cloud agent にコメントを送り、反復して改善できます。たとえば、表示する関連ゲームの数を調整するよう依頼できます。また、この Issue を割り当てた時点ではカスタム指示をまだ更新していなかったため、コメントヘッダーと TSDoc のドキュメントコメントを追加するよう依頼することもできます。**Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントを使って実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 の Mission control から追加を依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が付いています。その場合は Copilot が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認するときは、特に次の点に注目してください。
   - モード切り替え用の UI コンポーネント。
   - 利用者の設定を保持する local storage の使用。
   - ハイコントラストモードとライトモードの CSS やスタイル変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが登録されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名を `<branch-name>` に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリを起動し、ブラウザーでハイコントラストモードとライトモードを切り替えて、ページを再読み込みした後も設定が保持されることを確認します。

アクセシビリティエージェントが、アクセシビリティのベストプラクティスに沿って Copilot の機能実装をガイドしたことを確認できます。アクセシビリティに関する懸念や改善点があれば、前の PR と同じように **@copilot** をコメントでメンションして更新を依頼できます。

## 任意の演習 — 委任を続ける

Cloud agent は、実際のバックログ項目を任せて別の作業に取り組むと効果的です。リポジトリにいくつか Issue を登録し、Copilot に割り当ててみましょう。たとえば、次のようなタスクがあります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent の演習を完了しました。これまでのレッスンで、次のことを行いました。

- **このリポジトリに用意されたカスタム指示ファイルを確認し**、後で cloud agent の出力に反映される様子を学びました。
- **Issue を Copilot cloud agent に割り当て**、環境の準備、計画、非同期の実行を確認しました。
- **カスタムエージェントを作成して使い**、ハイコントラストモードとライトモードを実装しました。
- **Copilot agents ページを Mission control として使い**、アクセシビリティセッションを監視して作業中に方向を示しました。
- **cloud agent のプルリクエストをレビューして反復し**、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent の演習は完了です。ほかの環境で、今回学んだことをさらに試せます。

- 🖥️ **[VS Code ワークショップ](../../vscode/)** — IDE 内で Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI ワークショップ](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` を使い、レビュー済みの変更を2つターミナルからリリースします。

自分のリポジトリでは、次のアイデアも試せます。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てます。
- 別の分野向けのカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## 参考資料

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
