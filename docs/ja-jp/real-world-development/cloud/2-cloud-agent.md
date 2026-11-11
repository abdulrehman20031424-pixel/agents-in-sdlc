---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織では、未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要望など、技術的負債に取り組んでいます。GitHub Copilot cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行します。作業が終わると、人間がレビューできるドラフトの pull request (PR) を作成します。これにより、時間のかかる作業を任せ、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent の環境をカスタマイズし、安全な操作や適切な Issue の範囲設定を確認して、Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys では、古いコードにドキュメントがなく、エクスポートされたアプリケーションの関数に TSDoc コメントを追加したいと考えています。また、デザインチームはゲーム詳細ページに同じカテゴリの関連ゲームを表示し、UX の受け入れテストに使いたいと考えています。いずれも優先度の高い作業を妨げないよう、Copilot cloud agent に非同期で対応を依頼します。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) はバックグラウンドで作業します。[GitHub Issue を Copilot に割り当てる][assign-issue]方法などで依頼できます。割り当てると Copilot は進捗を記録するドラフト PR を作成し、環境を設定してタスクを開始します。作業中も完了後もセッションを確認できます。提案された解決策のレビュー準備ができると、PR で通知されます。

## 適切な範囲の指示の重要性

GitHub Copilot に魔法はありません。単純に見える操作でも、詳しく調べると複雑さがあります。[Copilot cloud agent に依頼するタスクの範囲を明確にしてください][cloud-agent-best-practices]。AI ペアプログラマーとして、段階的に進め、学び、試し、調整します。生成 AI を利用しても、ソフトウェア開発の基本は変わりません。

## リポジトリのカスタム指示

前の演習では、カスタム指示が Copilot をガイドする方法を確認しました。まだ確認していない場合は、`.github/copilot-instructions.md` の **Code standards** と TypeScript、TSDoc コメントに関する指示、および `.github/instructions/unit-tests.instructions.md` の `applyTo` が `**/*.test.ts` に適用されることを確認します。次の手順で `Code lacks documentation` Issue を cloud agent に割り当てたら、TSDoc コメント、TypeScript の規約、ファイルヘッダーが反映されているか PR で確認してください。

## Copilot cloud agent の開発環境を設定する

コードの作成では、必要な開発ツールや依存関係を整える環境設定が必要です。cloud agent も作業の前に GitHub Actions の環境を準備します。`.github/workflows/copilot-setup-steps.yml` のセットアップワークフローでツールや依存関係を用意できます。ラボ用の環境は事前に設定されており、Node.js、プロジェクト依存関係、エンドツーエンドテスト用ブラウザー、単一 Astro アプリ用にマイグレーションとシードを適用した SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 各ステップに必要な最小限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。クローンしない場合は、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストに必要なデータを用意するため、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

この GitHub Actions ワークフローには次の特徴があります。

- `copilot-setup-steps` という1つのジョブがあり、Copilot が PR 作業を開始する前に GitHub Actions で実行されます。
- `workflow_dispatch` により、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot を待たずに設定をテストできます。

## ドキュメントを追加する

ドキュメントの不足は、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。ドキュメントの追加は cloud agent に任せられる作業です。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

    ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルの新しい pull request (PR) を開きます。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が作成した todo リストを確認します。

> [!NOTE]
> Copilot は環境を構築し、プロジェクトを分析して、問題への最適な取り組み方を考えるため、PR に todo リストが表示されるまで数分かかることがあります。

13. todo リストと Copilot が実行する作業を確認します。
14. PR のタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が見えない場合は、ブラウザーを更新してください。

16. ライブセッションをスクロールし、Copilot がコードを調べて状態を理解し、計画を考えてコードを作成する様子を確認します。

Copilot がバックグラウンドで作業する間、別の作業に取り組めます。次にもう1つのタスクを割り当てます。

## ゲーム詳細ページに関連ゲームを追加する

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の要件を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことが分かります。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェアエンジニアのように作業するため、完了まで積極的に監視する必要はありません。完了後に結果をレビューします。次はカスタムエージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]に Issue を割り当て、技術的負債への対応、新機能の作成、フレームワーク間のコード移行などを非同期で依頼できることを学びました。次は、カスタムエージェントを作成して使用します。cloud agent では [MCP サーバー][cloud-agent-mcp]とカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
