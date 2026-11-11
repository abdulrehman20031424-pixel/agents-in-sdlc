---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つの pull request、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローに取り組みました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、用意されたフィルターの Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初の pull request (PR) を手動でマージしました。
- フィルターの Issue から開始し、Plan モードで方針を決め、Autopilot モードで構築し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの根拠を評価しました。
- フィルター機能全体をレビューし、フィルターの PR に Agent Merge を承認しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュコマンドを確認しました。

## 作成したもの

このワークショップでは、更新済みの `main` からそれぞれ独立したブランチを作り、2つの PR を作成しました。

1. **星評価:** 既存の `starRating` と未評価の状態をゲームカードに表示します。
2. **フィルターと品質ワークフロー:** フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルターの計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、それらの作業を1つの PR にまとめています。

## さまざまな検証方法

自動テスト、ブラウザーでの手動確認、MCP による Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果に要件とテストカバレッジの確認を加えました。

追加するテストは実際の不足を解決する必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを承認する前にコードと根拠を確認し、変更後は関連する根拠を再取得してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが作業の内容に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整できます。指示は規約を定め、スキルは繰り返し実行できるタスクを記述し、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った **モードとモデル** を選びます。構築前に方針を検討するには **Plan**、範囲の限定された変更を自分で確認しながら進めるには **Interactive**、明確なタスクを任せるには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

環境と同様に、コンテキストも重要です。*何を*、*なぜ*、*どのように*作るのかを明確に伝えると、結果が大きく変わります。

## さらに試す

基本的なワークフローを完了しました。Copilot CLI の他の機能も試してください。

- `/review` でコードレビューエージェントに変更を分析させます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを委任します。

## 次のステップ

ツールの使い方を上達させるには、使い続けるのが一番です。製品コード、趣味のコード、作りたいと思っていた小さなアプリなどで活用してください。チームと知見を共有し、互いに学びましょう。ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張するには、省略可能な [Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境での体験と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI で pull request を管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
