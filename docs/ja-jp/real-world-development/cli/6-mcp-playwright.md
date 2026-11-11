---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーからフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスとの連携、Copilot で利用できる自動化など、開発には多くの作業が必要です。MCP サーバーを使うと、Copilot の CLI に組み込まれていないツールやサービスを利用できます。

このレッスンでは、次の内容を学習します。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を理解します。
- まだ利用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルター機能を試します。

## シナリオ

単体テストやエンドツーエンドテストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。ユーザーと同じように Web サイトを使えるように Copilot を拡張し、変更が期待どおりに動作するという確信を高めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を利用すると、エージェントは最新の情報にアクセスし、ユーザーの代わりに操作できます。

MCP サーバーは、AI エージェントと外部ツールやサービスの間を仲介します。それぞれの MCP サーバーは、エージェントがアクセスできるツールとリソースを提供します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、pull request を管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザーの自動操作機能を提供します。

他にも多くの MCP サーバーがあります。GitHub が運営する [MCP registry][mcp-registry] で、エコシステム内のサーバーを見つけたり、貢献したりできます。

> [!CAUTION]
> MCP サーバーは、他の依存関係と同様に扱ってください。使用前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Copilot CLI から Web サイトを操作できるよう、Playwright MCP サーバーがまだ利用できない場合は追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、セットアップを省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd> を押してから <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) で、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

Copilot が利用できる Playwright MCP サーバーで、実際に機能を検証します。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を確認するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、明示的に指定しなくても Copilot は使用する MCP サーバーを判断します。ただし、使用するツールが分かっている場合は、指定すると結果の一貫性を高め、トークンを節約できます。

2. Copilot がブラウザーで行う操作を確認します。
3. 結果を読み、期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright でサイトを操作し、サーバーを停止して結果を報告します。

## まとめと次のステップ

Copilot CLI から実際のブラウザーで機能を確認できました。次は、スキルとブラウザーツールを専門的な役割として組み合わせる[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
