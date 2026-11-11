---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、指示、ベストプラクティス、コーディングパターン、専門知識を定義します。チームの知識を再利用できるエージェントとしてまとめることで、たとえば [WCAG][wcag] 準拠を確認するアクセシビリティエージェントや、安全なコーディングを確認するセキュリティエージェントを作成できます。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を記述した Markdown プロンプトが必要です。

### カスタムエージェントと agent skills の違い

カスタムエージェントと [agent skills][agent-skills-concept]には一部共通点があります。分かりやすく言うと、**カスタムエージェントは作業者、スキルはツール**です。カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルや他のエージェントを調整しながら作業します。この演習では、アクセシビリティエージェントがサイトをレビューし、ガイドラインに沿って更新します。

> [!NOTE]
> カスタムエージェントの作成方法は1つではありません。環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにしたいと考えています。文字色と背景色のコントラストが十分でないため、現在のダークテーマが読みにくいというフィードバックを受け、ユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

## アクセシビリティカスタムエージェントを確認する

既存のアクセシビリティエージェントがどのように Copilot をガイドするか確認します。

1. codespace に戻り、ターミナルを開いて `main` からアクセシビリティ用のブランチを作成します (演習 3 のフィルター PR とは分けて作業します)。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開き、`name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必要です。

3. アクセシブルな Web サイト向けの責任、ベストプラクティス、HTML/CSS/JavaScript のコード例、よくある問題を確認します。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェントのドロップダウンには、`.github/agents` に定義されたカスタムエージェントが表示されます。エージェントを選択すると、その指示とツールに合わせてチャットを設定できます。

1. codespace に戻り、Copilot Chat パネルで **New Chat** を選択して新しい会話を開始します。
2. Chat ビューのドロップダウンで **Agent** を選択します。

   ![Chat ビューでエージェントを選択するドロップダウンのスクリーンショット](../../../_images/shared-chat-mode-selector.png)

3. エージェントのドロップダウンを開き、一覧から **Accessibility agent** を選択します。
4. 次のプロンプトを送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案して適用する様子を確認します。各編集はファイルパスと差分とともにチャットに表示されます。
6. **Files changed** の表示で、エージェントが変更したファイルを確認します。

> [!NOTE]
> 実際のリポジトリに変更を加えるため、完了まで数分かかる場合があります。Astro コンポーネント、CSS、関連テストなどが更新されることがあります。次の演習では、作業中のセッションをレビューして追加の指示を送ります。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学び、Copilot Chat のエージェントモードで使用しました。次は、エージェントの作業を[監視して方向付けます][next-lesson]。変更を確認し、ライトモードの切り替えを追加するよう依頼します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
