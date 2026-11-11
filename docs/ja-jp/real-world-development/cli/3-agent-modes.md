---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で実装方針を決め、Autopilot で Issue に基づくフィルター機能を構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

プロジェクトに小さな機能を追加しました。より大きな変更には、さらにしっかりとしたプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作し、適切な方法で必要な機能を構築できるよう支援します。このレッスンから、Issue を起点に新機能を作成し、コードを検証し、機能の動作を確認して、プロジェクトにマージするまでのエージェント主導の開発プロセスを学びます。

> [!NOTE]
> 機能開発を続ける間、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中するため手順を簡略化します。

このレッスンでは、次の内容を学習します。

- GitHub Issue から Copilot との会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えているため、訪問者がゲームをカテゴリやパブリッシャーで絞り込めるようにします。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法などは、コーディング前に合意する必要があります。Plan モードで方針を決めてから、範囲を明確にした実装を Autopilot に任せます。

## 背景

AI コーディングエージェントを開発フローに導入しても、ソフトウェア開発の基本は変わりません。一般的な流れは次のとおりです。

1. 必要な作業が記載された Issue から始めます。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で確認します。
6. pull request (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、ほとんどの場合は上記の流れに沿っています。

標準的な手順に沿うことで、AI が生成したコードが要件を満たし、手書きのコードと同じレビューと検証を受けるようにします。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> でモードを切り替えます。

- **Interactive**: ユーザーとエージェントが協力して作業します。エージェントは変更を提案し、入力を待ってから進めます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

最初に Plan モードで計画をレビューしてから、Autopilot で実装します。

## Issue から開始する

フィルター機能の作業を始める前に、Codespace とターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新して、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選びます。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の Issue や pull request (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てることで、Copilot に実装を依頼する前に、機能の実装方法やタスクの進め方を決められます。複雑な作業では、事前に計画することをお勧めします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替え、プロンプトの下に **Plan** と表示されていることを確認します。
2. 前の手順で追加した Issue の後ろに、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、計画を作成します。

3. フィルターの動作について質問されたら、希望する動作を回答します。正解は1つではありません。
4. 計画ができたら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画を確認します。
6. 合意した内容と異なる部分があれば、変更を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使うと、Copilot は完了したと判断するまで作業を反復します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンで同様のラベルの選択肢を選びます。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が合意済みの計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まることがあります。必ず計画を先にレビューしてください。依存関係が不足している、またはポートが競合していると報告された場合は、チェック完了と判断する前に環境の問題を解決してください。

## 実装をレビューして検証する

生成されたコードは、他のコードと同じようにマージ前にレビューします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** や **Autopilot** がないことを確認します。
2. `/diff` を入力し、フィルター機能の実装とテストを確認します。
3. Issue と、計画時に決めた内容に照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分ビューを終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば修正を依頼します。

## 新しい機能を試す

Codespaces で転送されたポートからサイトを開き、アプリを実行します。

1. 次のプロンプトで Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリとパブリッシャー、およびその組み合わせで絞り込みます。
4. リセットと該当なしの場合の動作が、Issue と決定内容に合っていることを確認します。
5. Codespace に戻り、Copilot に開発サーバーの停止を依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を構築し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から Copilot との会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は、カスタム指示を使ってコードが規約に従うようにします。[カスタム指示を使う][next-lesson]に進みます。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
