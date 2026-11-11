---
title: "レッスン 2 - 星評価の追加: すぐにできる改善"
description: "Copilot CLI でゲームカードを小さく変更し、転送されたブラウザーで確認して、最初の pull request をマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次は、プロジェクトに最初の変更を加えます。変更は小さくしましょう。ゲームのデータには星評価がすでにありますが、ホームページのゲームカードには表示されていません。エージェントに表示を依頼し、変更をレビューして、最初の pull request としてマージします。

このレッスンでは、次の内容を学習します。

- 機能ブランチで Copilot との会話を開始します。
- エージェントに小さな変更を依頼します。
- `/diff` で変更をレビューします。
- 転送されたブラウザーでアプリを実行して確認します。
- 最初の pull request を作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。しかし、ホームページのゲームカードにはタイトル、カテゴリ、パブリッシャー、説明しかありません。ウォームアップとして、各カードに既存の評価を表示するようエージェントに依頼します。最初のセッションに適した、範囲の小さな変更です。

## 会話の構成

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot app と異なり、通常の CLI 会話では、専用の worktree ではなく、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使います。保存した会話には後から戻れますが、ファイルとブランチはディスク上の通常の Git 状態のままです。

会話では、プロンプトとエージェントの応答、ファイルを調べて編集するエージェントのツール操作、`/diff` で確認する変更の3つを扱います。

## 会話を開始して変更を依頼する

1. Codespace に戻ります。
2. まだターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. まだ起動していない場合は、次のコマンドを実行して Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. スラッシュコマンド `/new` を入力して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームの詳細を表示するファイルを見つけて、必要なコードを作成します。Copilot CLI で新機能を追加できました。

## 差分をレビューする

小さな変更でも、マージする前に AI が生成した変更をレビューします。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. ゲームカードに評価がある場合は数値が、`starRating` が `null` の場合は `No rating yet` が表示されることを確認します。
3. どちらの状態もテストされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を終了します。

> [!NOTE]
> Copilot を含む生成 AI ツールは決定論的ではなく確率的に動作するため、コードは例と異なる場合があります。特定の実装に固執せず、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると判断せず、Copilot に Web サイトを起動してもらい、Codespaces で転送されたブラウザーから UI を確認します。

1. 次のプロンプトで Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. ゲームカードに 5 段階の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初の pull request を作成してマージする

機能を作成できました。次に、pull request (PR) を作成し、コードをプロジェクトにマージします。

1. 既定のエージェントに、変更のコミットを依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存のコミットを push し、PR の URL を表示します。
3. Copilot CLI が表示した URL を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して PR を開きます。
4. 変更されたファイルとチェックをレビューします。
5. 準備ができたら **Merge pull request** を選び、マージを確認します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、最初の変更をリリースできました。このレッスンでは、次のことを行いました。

- 機能ブランチで Copilot との会話を開始しました。
- エージェントにゲームカードの小さな変更を依頼しました。
- `/diff` で変更をレビューしました。
- 転送されたブラウザーでアプリを実行し、星評価を確認しました。
- 最初の pull request を作成してマージしました。

次は、[フィルター機能の Issue から開始し、Plan モードと Autopilot モードを使って][next-lesson]大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
