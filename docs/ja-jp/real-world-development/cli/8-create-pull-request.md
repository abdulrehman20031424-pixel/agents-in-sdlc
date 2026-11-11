---
title: "レッスン 8 - 機能 PR の作成とマージ"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて1つのブランチに保存されています。まとめてレビューし、pull request (PR) を開きます。星評価の PR は手動でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、指示、スキル、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、フィルター機能と品質に関する作業を1つの会話とブランチで進め、この PR にまとめます。

このレッスンでは、次の内容を学習します。

- Agent Merge と、マージのライフサイクルを自動化する方法を理解します。
- 機能全体の変更と検証の根拠を確認します。
- フィルター機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能の作業を通じて、Copilot で機能を計画、実装、検証しました。次に Tailspin Toys では、マージの承認権限を開発者の管理下に置いたまま、残りの PR 作業を自動化します。

## Agent Merge について

**Agent Merge** は、pull request をマージするために必要な残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗を修正し、レビューコメントに対応し、必要に応じてリベースするなど、PR を妨げている問題に取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

Agent Merge を有効にする前に、作業をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードを作成したら、レビューして PR を作成し、残りの作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` で Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視し、すべて成功するとマージします。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが届いたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は報告内容を確認し、リポジトリで許可されている場合はレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、pull request の処理など、開発プロセスの一部を自動化しました。次は、コンテキスト、モデル、共有、cloud agent への任意の委任に使う[便利な Copilot CLI スラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI で pull request を管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
