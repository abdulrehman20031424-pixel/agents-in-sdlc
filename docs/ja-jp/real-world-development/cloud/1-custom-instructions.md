---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を利用する際は、コンテキストが重要です。作業方法に決まりがある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを利用できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に作業内容だけでなく方法も伝えられます。

この演習では、次の内容を学習します。

- リポジトリのカスタム指示とパスごとの指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント基準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する基準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは指示の追加前後でプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期に作業するため、リアルタイムで影響を比較するのが難しいためです。後の演習で cloud agent が作成した pull request をレビューする際に、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には次の開発ガイドラインがあります。

- データ層には必ず単体テストを用意します。
- UI はダークモードで、現代的な印象にします。
- コードのドキュメントには TSDoc コメントを使います。
- 各ファイルの先頭に、内容を説明するコメントを追加します。

指示ファイルを使い、Copilot がこれらの慣行に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルなどのコンテキストや希望を Copilot に伝えられます。リポジトリ全体に適用する指示と、特定のファイルやタスクに適用する指示を作成できます。

- `.github/copilot-instructions.md` は、リポジトリへの **すべて** のリクエストで Copilot に送信する指示ファイルです。技術スタック、開発中のもの、ベストプラクティスなど、プロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、TypeScript などの言語、UI コンポーネントの作成、単体テストなどに固有のガイダンスを記載するファイルです。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルを使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、他の形式の指示ファイルにも対応しています。

### 指示ファイルを管理するためのベストプラクティス

- `copilot-instructions.md` には、プロジェクトの説明、構成、全体に適用するコーディング標準などを記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ層など、特定のファイルやタスクに関する指示を記載します。
- 自然な言葉で、明確なガイダンスと良い例・避けるべき例を示します。

指示ファイルの作成方法は1つではありません。プロジェクトで試しながら、適切な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、指示ファイルを十分に用意することをお勧めします。このプロジェクトには [UI][ui-instructions]や [Astro][astro-instructions] などの指示があります。Copilot に指示ファイルを生成してもらうこともできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、使用する環境に応じた機能を使います。テンプレートを探す場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタム指示ファイルを確認する

このリポジトリには、`copilot-instructions.md` とタスクごとの `*.instructions.md` が用意されています。エディターまたは GitHub の Web UI で次のファイルを確認します。

1. `.github/copilot-instructions.md` を開き、プロジェクトの説明、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などを確認します。**Code standards** の **GitHub Actions Workflows** のガイダンスも、Copilot とのやりとりに適用されます。
2. `.github/instructions` フォルダーを確認します。Astro、Drizzle、テストなどの指示があります。
3. `.github/instructions/unit-tests.instructions.md` を開きます。`applyTo` フィールドが、リポジトリルートを基準とした glob (例: `**/*.test.ts`) で適用対象を決めます。
4. 単体テストの作成に関する指示を確認します。
5. `.github/instructions/drizzle.instructions.md` の末尾にある他の指示ファイルやプロジェクトファイルへのリンクを確認します。パスは指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準がありますが、コード内のドキュメントはまだ求めていません。次の手順で TSDoc コメントとファイルヘッダーを追加します。

## リポジトリ全体に適用する基準を追加する

1. codespace のターミナルで、作業用のブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開き、**Code formatting requirements** セクションを探します (27 行目付近)。
3. 既存の基準の直後に、次の2行を追加して、ファイルヘッダーと TSDoc コメントを依頼します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリ全体の標準には `.github/copilot-instructions.md`、特定の言語、ファイル、タスクには `*.instructions.md` を使います。TSDoc コメントのようなプロジェクト共通の基準には、リポジトリレベルのファイルが適しています。

## 指示の更新をコミット、push、マージする

次の演習では cloud agent が `main` からブランチを作成するため、指示を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` への pull request を作成してマージします。次の演習で cloud agent が Issue に取り組むとき、更新した指示を読み込みます。

> [!TIP]
> ワークショップで `main` に直接作業する場合は、ブランチの手順を省略して `main` にコミットしてもかまいません。ここでは実際のプロジェクトに近い手順を紹介しています。

## まとめと次のステップ

指示ファイルを確認し、cloud agent が作業中に使うリポジトリ全体の基準を追加しました。次は、最初の Issue を Copilot cloud agent に[割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示作成のベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
