---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、cloud agent への任意の委任に使うコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には対話に使えるスラッシュコマンドが多数あります。高度な機能、内部情報、追加の設定オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、他の便利なコマンドを確認します。

このレッスンでは、次の内容を学習します。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

基本的な CLI ワークフローを完了しました。次は、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュコマンドを使って確認したり、自分で圧縮したりできます。

1. Codespace に戻り、まだ起動していない場合はリポジトリのルートから Copilot CLI を起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように配分されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合は、大きな違いが見られないことがあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的に会話を圧縮します。タイミングを選びたい場合は `/compact` を使います。関係のない別タスクに移るときは、`/clear` または `/new` で新しい会話を開始します。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では利用可能なモデルを一覧表示し、選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> で一覧を閉じます。

## セッションを共有する

チームで協力して知見を共有すると、AI ツールの活用を改善できます。`/share` コマンドを使って、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. 共有する場合は、ローカルの Markdown へのエクスポートに使う `/share file` など、適切な出力先を選びます。
3. エクスポートした内容を、送信または公開する前に確認します。エクスポートにはプロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容を公開しないでください。

## 省略可能: 作業の拡大や委任

基本的なワークショップは完了しました。Copilot CLI には、大きなタスクに対応する次のコマンドもあります。

- `/fleet` は独立したサブタスクをサブエージェントに分割し、並行して実行します。
- `/worktree` は別のタスク用に分離された Git worktree を作成します。
- `/delegate` は、非同期に作業して pull request を作成する場合がある Copilot cloud agent にタスクを委任します。

これらのコマンドを使うと、追加の worktree やリモート作業が発生する場合があるため、使用は任意です。試すときは、新しい範囲の明確なタスクから始め、通常の手順で結果をレビューしてください。非同期エージェントについて詳しく知るには、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定、セッションの共有、内部動作の確認ができます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` でコンテキスト管理を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI には、他にも多くのスラッシュコマンドがあります。[学んだ内容と次のステップ][next-lesson]を確認して、ワークショップを締めくくりましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
