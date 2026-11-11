---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を利用する際は、コンテキストが重要です。作業方法に決まりがある場合や、Copilot が知っておくべき背景がある場合は、[指示ファイル][instruction-files]を使ってコード生成のガイダンスを伝えます。

この演習では、次の内容を学習します。

- リポジトリのカスタム指示とパスごとの指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント基準が Copilot に伝わる方法を確認します。
- 現在の指示で、フィルター機能用のデータスライス (publishers ヘルパー) を作成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する基準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい基準を採用する様子を確認します。
- 指示の更新とフィルタリングの作業を `main` にコミットし、次の演習で Copilot が最新のガイダンスを利用できるようにします。

> [!CAUTION]
> 生成コードが常にすべての基準に従うとは限りません。Copilot の動作は確率的です。この演習では、一字一句同じ結果ではなく、指示の更新後に動作がどう変わるかを確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には次の開発ガイドラインがあります。

- データ層には必ず単体テストを用意します。
- UI はダークモードで、現代的な印象にします。
- コードのドキュメントには TSDoc コメントを使います。
- 各ファイルの先頭に、内容を説明するコメントを追加します。

指示ファイルを使い、Copilot がこれらの慣行に沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルなどのコンテキストや希望を Copilot に伝えられます。リポジトリ全体に適用する指示と、特定の種類のファイルに適用する指示があります。

- `.github/copilot-instructions.md` は、リポジトリへの **すべて** のリクエストで Copilot に送信する指示です。技術スタック、開発中のもの、ベストプラクティスなどを記載します。
- `.github/instructions/*.instructions.md` は、TypeScript などの言語や UI コンポーネントの作成、単体テストなど、特定のファイルやタスクに関するガイダンスを記載します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。Copilot Chat、Copilot CLI、Copilot cloud agent はリポジトリレベルの指示と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルを使います。AGENTS.md や CLAUDE.md など、他の形式にも対応しています。

### 指示ファイルを管理するためのベストプラクティス

スタータープロジェクトの例から、指示ファイルの代表的な使い方を確認できます。

- `copilot-instructions.md` には、プロジェクトの説明、構成、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ層など、特定のファイル形式やタスクに関する指示を記載します。
- 自然な言葉で、明確なガイダンスと良い例・避けるべき例を示します。

指示ファイルの作成方法は1つではありません。プロジェクトで試しながら、適切な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、指示ファイルを十分に用意することをお勧めします。このプロジェクトには [UI 更新用の指示][ui-instructions]や [Astro 用の指示][astro-instructions]があります。Copilot に指示ファイルを生成してもらうこともできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、各環境で提供される機能を使います。テンプレートを探す場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot

## プロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトに含まれるファイルを確認します。

1. Codespace で `.github/copilot-instructions.md` を開き、プロジェクトの説明、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などを確認します。**Code standards** にある **GitHub Actions Workflows** のガイダンスも、Copilot とのやりとりに適用されます。
2. `.github/instructions` フォルダーを確認します。Astro、Drizzle、テストなどの指示があります。
3. `.github/instructions/unit-tests.instructions.md` を開きます。`applyTo` フィールドが、適用対象のファイルを示す glob (`**/*.test.ts` など) を定義しています。
4. 単体テスト作成のための指示を確認します。
5. `.github/instructions/drizzle.instructions.md` の末尾にある他の指示ファイルやプロジェクトファイルへのリンクを確認します。パスは指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準がありますが、コード内のドキュメントを求める規則はありません。次に TSDoc コメントとファイルヘッダーを追加します。

## 変更用ブランチを作成する

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) でターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace に戻り、Copilot Chat パネルで **New Chat** を選択して、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は手順で指定します。

1. 前の演習のタブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの場所を Copilot に伝えます。
3. Chat ビューのドロップダウンで **Agent** を選択します。
4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを確認し、ヘルパーとテストなどの変更を適用します。
6. Drizzle の指示により、`db` クライアントを最初の引数として受け取り、型付き配列を返す関数が生成されることを確認します。
7. まだ TSDoc コメントとファイルヘッダーがないことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントを追加する場合があります。その場合も問題ありません。ここでは指示更新後の一貫性を確認します。

## リポジトリ全体に適用する基準を追加する

1. `.github/copilot-instructions.md` を再度開きます。
2. **Code formatting requirements** セクションを探します (27 行目付近)。
3. 既存の基準の直後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

## 更新したガイダンスを再実行して確認する

1. Copilot Chat で **New Chat** を選択して会話をリセットします。
2. `src/lib/publishers.ts` を選択します。
3. 次の同じプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 生成ファイルに、たとえば次のファイルコメントが追加されたことを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディングプラットフォームの publisher データアクセスヘルパーです。
    * データベースから publisher 情報を取得する関数を提供します。
    */
   ```

5. 関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * すべての publisher の ID と名前の一覧を返します。
    *
    * @param db - Drizzle データベースクライアントです。
    * @returns publisher オブジェクトの配列に解決される Promise です。
    */
   ```

## 指示とブランチをコミットする

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) でターミナルを開きます。
2. `git status` を実行して指示とヘルパーの変更を確認します。
3. 指示ファイルと Copilot が作成したヘルパーの変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

GitHub ツールを使って、関連する Issue を探し、PR を作成してマージするよう Copilot に依頼します。

1. codespace で Copilot Chat を開き、エージェントのドロップダウンで **Agent** を選びます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) で Copilot Chat ビューを開きます。
3. 次の依頼をします。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. GitHub の操作を許可するよう求められたら、コマンドを確認して適切に **Allow** を選択します。
5. PR が作成されたら、次のプロンプトでマージしてローカルのブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub の操作やシェルコマンドを許可するよう求められたら、内容を確認して適切に **Allow** を選びます。

## まとめと次のステップ

指示ファイルが Copilot のコンテキストに与える影響を確認し、Copilot Chat で既存の指示を使った生成、リポジトリ全体に適用する標準の追加、再生成、指示とヘルパーの `main` へのコミットを行いました。次は、指示を活用する[エージェントモード][next-lesson]で機能を追加します。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示作成のベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
