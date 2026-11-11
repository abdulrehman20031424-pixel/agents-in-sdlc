---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリの変更では、複数のファイルを更新し、テストを実行する必要があります。関連ファイルを探し、変更し、テストを実行して問題を直す作業を、Copilot Agent Mode を使って進めます。エージェントモードはプロジェクトを調べ、必要なファイルを更新し、テストを実行し、見つかった問題を修正します。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途です。エージェントモードは IDE 内で動作し、すばやいフィードバックとやりとりを可能にします。cloud agent は Issue と pull request を介して、チームメンバーのように非同期で作業します。

この演習では、次の内容を学習します。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定して変更を調整します。
- UI とデータ層にまたがる新機能を実装します。
- コードベースにマージする前に、Copilot が作成した変更とテストをレビューします。

## シナリオ

ゲームの一覧をカテゴリやパブリッシャーで絞り込めるようにします。前の演習で追加した publishers ヘルパーを使い、Copilot Agent Mode で残りのデータ層、UI、テストを実装します。

## Tailspin Toys の Web サイトを実行する

Tailspin Toys は、開発者をテーマにしたボードゲームのクラウドファンディングサイトです。静的 HTML をビルド時に生成する単一の Astro アプリで、Drizzle ORM を使ってページの frontmatter からローカル SQLite データベースを直接参照します。別のバックエンド API やクライアント側 UI フレームワークはありません。データアクセスヘルパーは `src/lib/` に置き、インタラクティブな機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

1. 前の演習から `main` に戻っていることを確認します。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) でターミナルを開きます。
3. フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   サーバーの準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

5. ターミナルで <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら `http://localhost:4321` を選択してサイトを開きます。codespace では `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。

ゲームの一覧ページにはゲームのタイトル、画像、説明があります。ゲームを選択すると、詳細ページでタイトル、説明、パブリッシャー、カテゴリを確認できます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。手順で指定されている場合は、モードとモデルを選びます。

1. Chat ビューのドロップダウンで、ファイル変更を適用し、ターミナルコマンドやツールを実行できる **Agent** を選択します。

   ![Chat ビューでエージェントを選択するドロップダウンのスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成では Claude Sonnet 4.5 を使用しましたが、著者が特定のモデルを推奨するものではありません。LLM の動作は確率的であるため、例とは異なる提案が表示されることがあります。これは想定された動作です。

> [!NOTE]
> 確率的な動作のため、Copilot が異なる MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを送信して、バックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して Issue の一覧表示を実行します。
5. Copilot が生成した Issue の一覧と優先順位を確認します。

Copilot はユーザーに最も役立つと思われる項目を優先順位付けします。

## 指示ファイルを確認する

コードの生成を始める前に、Copilot にコンテキストを与える[ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を確認します。次のガイダンスを確認してください。

- アーキテクチャの概要
- コンポーネントの設計、テスト可能性、アクセシビリティの原則
- Astro、Tailwind CSS、Drizzle などのファイル形式ごとの指示へのリンク

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。相対パスを使うと、指示を再利用できる小さなファイルに分け、Copilot にプロジェクトの例を示せます。

## フィルター機能を実装する

フィルター機能では、少なくとも次の変更が必要です。

- `src/lib/` のフィルターロジックを追加または修正します。
- フィルターの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。
- マージ前にテストを実行し、成功することを確認します。

1. 現在の会話を続けるか、**New Chat** で新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、UI 指示ファイルを追加します。
3. **Agent** が選択されていることを確認します。
4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. 次のプロンプトで、バックログの関連 Issue に基づく実装を依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がデータ層のヘルパー、UI、テストを調べて更新し、テストを実行する様子を確認します。
7. Copilot に求められたら **Continue** を選択してテストを実行します。処理中にテストが一時的に失敗しても、Copilot はコード生成とテストを繰り返します。
8. 生成されたコードの問題を確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

9. ブラウザーでサイトに戻り、新しい機能を試します。
10. 動作とコードを確認したら、Copilot Chat で **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode で Tailspin Toys に機能を追加しました。新機能を UI とデータ層に実装し、プロジェクトを調べて関連ファイルを更新し、変更とテストをレビューしました。次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、pull request を作成します。

### ボーナス演習 — ページネーションを実装する

ゲームが増えたらページネーションが必要です。この演習のスキルを使い、Copilot にページネーションの実装を依頼してみてください。指示ファイルを活用し、ページサイズをユーザーが選ぶか固定するかを決め、必要なコンテキストを与えてください。必要に応じて Copilot に変更と追加情報を依頼しながら反復します。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
