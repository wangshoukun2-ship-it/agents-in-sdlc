---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プルリクエストの自動化を学びながら、Tailspin Toys に2つの変更を加えて検証し、リリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、ターミナルで GitHub Copilot をエージェント型コーディングアシスタントとして利用できます。コードベースを調べ、コードを生成し、コマンドを実行して、外部ツールに接続します。すべてコマンドラインで行えるため、グラフィカルエディターに切り替えずに作業を続けられます。

このワークショップでは、GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローに取り組みます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして使い始めます。
2. 星評価に絞った変更を加えてブラウザーで確認し、最初のプルリクエスト (PR) を手動でマージします。
3. フィルター機能の Issue から始めて、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューします。
4. リポジトリの指示を更新し、フィルター機能に適用します。
5. 既存の `quality-checks` スキルをカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルター機能を確認します。
7. 品質保証 (QA) カスタムエージェントを作成して、要件、テスト範囲、検証の証拠をレビューします。
8. フィルター機能の変更全体をレビューし、Agent Merge を使います。
9. コンテキスト、モデル、共有、クラウドへの任意の委任に役立つスラッシュコマンドを確認します。

内容に集中できるよう、PR は2つ作成します。星評価の変更と、指示・スキル・QA プロファイル・テストを含むフィルター機能の変更です。フィルター機能と品質ワークフローでは、1つの会話とブランチを使って、それまでの作業を活用します。

## レッスン

| レッスン | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、使い始めます |
| [2. 星評価の追加: まずは小さな改善][ex2] | 最初の変更 | 既存の評価と未評価時の表示を追加し、最初の PR をマージします |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | Issue から機能を計画し、Autopilot で構築して Interactive モードでレビューします |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を確認・更新し、フィルター機能に適用します |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し実行できるチェック | 既存のスキルとレポート形式を確認・変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を設定し、フィルターの動作を調べます |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門家のプロファイルを作成・選択して、最終検証の証拠を集めます |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使います |
| [9. GitHub Copilot CLI のスラッシュコマンド][ex9] | CLI の機能 | コンテキスト、モデル、共有、クラウドへの任意の委任を確認します |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、参考資料を振り返ります |
| [オプション: Foundry の組み込み][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づくエージェントをデプロイして Web サイトに接続します |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンドライン操作の基本的な知識

> [!TIP]
> 有料プランを利用していない場合、認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
