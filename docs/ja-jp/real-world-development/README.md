---
slug: ja-jp/real-world-development
title: "実践的な開発"
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更を繰り返し計画、実装、テストし、プルリクエストをレビューして、エージェントと作業を管理します。

## シナリオ

あなたは、開発者をテーマにしたボードゲームのクラウドファンディングを提供する架空の会社、Tailspin Toys に入社したばかりの開発者です。チームのバックログは GitHub Issue として登録済みで、すぐに着手できます。カテゴリーやページネーションによる絞り込みなどの機能開発に加えて、アクセシビリティやコーディング規約などの品質改善も含まれています。

## 環境を選ぶ

GitHub Copilot は、さまざまな作業環境で利用できます。開発したい環境を選んでください。各ワークショップは個別のセットアップから始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、便利なスラッシュコマンドを活用します。

### [GitHub Copilot app][app]

デスクトップアプリでエージェントセッションを並行して実行し、セッションモードを切り替え、キャンバスで共同作業を行い、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を案内して進捗を確認し、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
