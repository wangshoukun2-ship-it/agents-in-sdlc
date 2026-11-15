---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、参考資料を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローに取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、用意されたフィルター機能の Issue を見つけました。
- 星評価を追加し、転送したブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から始め、Plan モードで方針を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの検証結果を評価しました。
- フィルター機能の変更全体をレビューし、フィルター機能の PR で Agent Merge を許可しました。
- コンテキスト、モデル、共有、クラウドへの任意の委任に使うスラッシュコマンドを確認しました。

## 作成したもの

このワークショップには、更新済みの `main` からそれぞれ作成した、2つの PR の節目があります。

1. **星評価:** 既存の `starRating` と未評価状態をゲームカードに表示します。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、指示を更新してその指示を機能に適用します。`quality-checks` のレポートをカスタマイズし、QA プロファイルと関連テストを追加します。

フィルター機能の計画から PR の作成までは、同じ会話とブランチを使用しました。手順を簡潔にするため、この作業を1つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分自身によるブラウザーでの確認、MCP 経由の Copilot によるブラウザー操作など、いくつかの方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、指定した形式で結果を報告しました。QA はこれらの結果に要件とテスト範囲のレビューを加え、PR の前にまとめて確認しました。

追加したテストは、実際の不足を補うものである必要があります。QA の実行で新しいテストが不要と判断されることもあります。マージを許可する前にコードと検証結果を確認し、変更後は関係する検証結果を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整しましょう。指示は規約を定め、スキルは繰り返し実行できるタスクを記述し、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。**Plan** は実装前に方針を検討する場合、**Interactive** は小さな変更を自分で確認しながら進める場合、**Autopilot** は範囲が明確なタスクを任せる場合に使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

環境と同様に、コンテキストも重要です。何を、なぜ、どのように作りたいかを明確に伝えることで、結果が大きく変わります。

## さらに試す

主要なワークフローを学びました。Copilot CLI には、ほかにも次のような機能があります。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送信します。

## 次のステップ

どのツールも、使い続けることで上達します。実際のコード、趣味のコード、長年作ろうと思っていた小さなアプリなどで活用してください。チームと学んだことを共有し、互いの経験から学びましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys をさらに拡張する場合は、オプションの[Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## 参考資料

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
