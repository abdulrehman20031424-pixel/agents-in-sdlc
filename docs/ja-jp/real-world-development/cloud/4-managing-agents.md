---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習で、Copilot cloud agent にユーザー体験の改善と機能追加に関する3つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも大切です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェントページ][agents-page]などで管理できます。この mission control では、open pull request (PR) があるエージェントタスクを確認できます。実行内容を調べ、進行中のセッションに指示を追加することもできます。

このレッスンでは、エージェントページでタスクを監視し、実行中のセッションに追加機能を依頼します。

## シナリオ

ハイコントラストモードを作成するようエージェントに依頼した後、チームはライトモードも追加したいと考えました。サイトのスタイルと切り替え機能をすでに更新しているため、ハイコントラストモードに加え、ライトモードも実装するようエージェントに指示します。

## Copilot cloud agent のタスクを確認する

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインのタスクリストを確認します。コードドキュメントの追加、ゲームを変更する API の生成、Web サイトへのハイコントラストモードの追加など、割り当てたタスクが表示されます。
3. 実行中のタスクを選択します。Copilot が実行したリポジトリのチェックアウト、環境作成、MCP サーバーのセットアップ、その他の操作を確認します。

> [!NOTE]
> 表示されるステップは、Copilot の作業状況や進め方によって異なります。

4. 右側に表示される PR ペインで、PR と変更されたファイルを確認します。

## cloud agent に追加の指示を送る

進行中のタスクを確認したので、ハイコントラストモードの作業にライトモードも追加するよう依頼します。

1. ハイコントラストモードに関するセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

    ![mission control の Accessibility セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まるまで数分待ちます。
3. **Steer active session while Copilot is working** ダイアログで、次のプロンプトを追加します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

    ![Copilot が作業中のセッションに指示を追加するダイアログのスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が指示を受け取り、作業に反映したことを確認します。

## Copilot に作業を任せる

Copilot は、メッセージを送信した時点で進めていたステップが終わると、新しい依頼を作業に反映します。完了まで数分かかることがあります。その間に、これまでに学習した内容を振り返りましょう。

## まとめと次のステップ

Copilot agents ページを使ってタスクを監視し、実行中のセッションに追加機能を依頼しました。次は、Copilot が作成した pull request を[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
