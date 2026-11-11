---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を作成しました。pull request を開く前に、ブラウザーで正しく動作することを確認します。Playwright MCP サーバーを接続し、Copilot に実際のブラウザーを操作させて機能をテストしてから、ブランチを公開して PR を作成します。

この演習では、次の内容を学習します。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する方法を理解します。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーでフィルター機能をテストさせます。
- ブランチを公開し、フィルター機能の pull request を開きます。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にアクセスできると、さらに便利になります。Copilot は Model Context Protocol (MCP) を使って、外部ツールやサービスと通信します。

![エージェントモードの内部と、MCP サーバーや VS Code 拡張機能が提供するツールを含む、コンテキスト、LLM、ツールとのやりとりを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための標準です。MCP を利用すると、エージェントはリソースを使って最新情報にアクセスしたり、ツールを使ってユーザーの代わりに操作したりできます。

MCP サーバーは AI エージェントと外部ツールやサービスとの間を仲介します。GitHub MCP Server はリポジトリ、Issue、pull request を管理する API にアクセスします。Playwright MCP Server は、Web ページへの移動、フォームの入力、ボタンの選択など、Playwright によるブラウザー自動操作を提供します。GitHub が運営する [MCP registry](https://github.com/mcp) で、他のサーバーも確認できます。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーは他の依存関係と同様に扱ってください。使用前にソースコードを確認し、発行元を検証し、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクトテンプレートに含まれています。前の演習で Copilot がバックログの Issue を読み取れたのはそのためです。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使用するため、ローカルへのインストールは不要です。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できるブラウザーを提供します。`.vscode/mcp.json` の `github` と並べて `playwright` の設定を追加し、ファイルを保存します。

```json
{
  "servers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/"
    },
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest", "--headless"]
    }
  }
}
```

リモートの `github` サーバーとは異なり、`playwright` はローカルサーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` は、画面を表示できない codespace でブラウザーを実行するための指定です。

> [!NOTE]
> Tailspin Toys ではエンドツーエンドテストに Playwright を使うため、必要なブラウザーはすでにインストールされている場合があります。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行して再試行します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを必要に応じて起動します。初めてツールを使うときにサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` の上に **Start** が表示された場合は選択して起動し、設定が正しいことを確認します。
2. サーバーを信頼するか尋ねられた場合は、設定を確認して信頼し、起動を許可します。

手動で起動する必要はありません。次の手順で Copilot が初めて使うときに、サーバーの起動と信頼の確認が行われます。

## フィルター機能をテストする

Playwright MCP サーバーを接続し、アプリが実行中であることを確認したら、次のプロンプトを Copilot に送ります。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使います。構造化されたラベル付きのボタン、リンク、リスト項目をエージェントが確認するため、機能の簡易テストとアクセシビリティの基本確認を同時に行えます。Copilot の報告を Issue の受け入れ条件と照らし合わせます。問題があれば質問するか、PR を開く前にコード修正を依頼します。

> [!NOTE]
> テストするには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Playwright MCP を初めて使うときにブラウザーのダウンロードが必要な場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再試行します。

## ブランチを公開して pull request を作成する

機能を確認できたら、チームのレビューに向けて pull request (PR) を開きます。まず `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。

   ![Copilot が行った変更を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミットメッセージを生成します。
4. **Publish** を選択してブランチをリポジトリに push します。

## pull request を作成する

Copilot に PR を作成してもらい、関連する Issue と紐付けます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェントのドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報を収集して操作することを許可します。GitHub ツールを初めて使うときに認証を求められた場合は、案内に従います。
5. Copilot が Issue を検索し、適切な PR を作成することを確認します。
6. Copilot が生成したリンクを選択して PR をレビューします。まだマージしないでください。

## まとめと次のステップ

MCP サーバーの追加、ブラウザーでのフィルター機能のテスト、ブランチの公開、pull request の作成を行いました。MCP サーバーを使って機能をテストしました。次は、アクセシビリティレビューなどの作業を効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
