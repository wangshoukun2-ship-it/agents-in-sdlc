---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウド上で非同期に作業できます。GitHub で作業を割り当てると、cloud agent がバックグラウンドでリポジトリを調べ、変更を行い、プルリクエストを作成します。その間、別の作業を続けられます。

この演習では、まず cloud agent が使うカスタム指示を追加し、GitHub Issue を割り当てて実装を依頼します。カスタムエージェントを確認して使い、エージェントのダッシュボードからセッションの進捗を監視して方向を示します。最後に、作成されたプルリクエストをレビューし、結果を反復して改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. カスタム指示][ex1] | コンテキスト | cloud agent が参照するカスタム指示を追加します |
| [2. Cloud agent][ex2] | 非同期エージェント | GitHub Issue を Copilot cloud agent に割り当てます |
| [3. カスタムエージェント][ex3] | 専門エージェント | カスタムエージェントを確認して使います |
| [4. エージェントの監視と管理][ex4] | 監視 | エージェントのセッションを監視して方向を示します |
| [5. 反復して改善する][ex5] | レビュー | PR をレビューし、Copilot の作業を改善して次の手順を選びます |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランを利用していない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、このプランですべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は **Copilot Free を除く Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Business または Copilot Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> この演習では MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ワークショップ](../cli/)または [VS Code ワークショップ](../vscode/)を参照してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
