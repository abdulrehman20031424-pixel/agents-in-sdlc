---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント作成の基準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を利用する際は、コンテキストが重要です。作業方法に決まりがある場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]では、必要なコードだけでなく、その構成方法も説明できます。フィルター機能を構築したので、Copilot が参照した指示を確認し、ドキュメント作成の基準を追加してコードに適用します。

このレッスンでは、次の内容を学習します。

- リポジトリの指示とパスごとの指示ファイルがエージェントに適用される方法を確認します。
- コーディング規約に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発作業に関するガイドラインがあります。

- コメントでは、コードの繰り返しではなく、意図や自明でない決定を説明します。
- `db/` と `src/lib/` のエクスポート関数は、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。`db` 引数を注入する場合は、その引数についても記述します。
- 再利用可能な Astro コンポーネントは `Props` の契約を説明します。関連コードが変わったらコメントも更新します。
- 既存の書式設定と lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの開発慣行に沿って作業できるようにします。

## 指示ファイル

カスタム指示では、コーディングスタイルなどのコンテキストや希望を Copilot に伝えます。規約、ライブラリ、コードに含めるコメントなどを指定できます。リポジトリ全体、または特定の種類のファイルに対する指示を作成できます。

- `.github/copilot-instructions.md` は、リポジトリに対する **すべての** リクエストで Copilot に送信されます。多くのリクエストで役立つ、プロジェクト全体のコンテキストを記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイドラインを記述します。

> [!NOTE]
> その他の指示ファイル形式やサポート状況は環境によって異なります。形式を利用する前に、[カスタム指示のサポート][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、すでに指示ファイルが含まれています。変更を加える前に確認します。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。ここに記述された指示は、このリポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを決める glob です。
6. `.github/instructions/drizzle.instructions.md` にある、他の指示ファイルやプロジェクトファイルへのリンクを確認します。指示を再利用できる小さなファイルに分け、Copilot に例を示せます。

## チームのガイダンスに合わせて指示を更新する

既存のファイルには、追加すべきガイダンスが残っています。`copilot-instructions.md` を変更し、新しく生成する TypeScript に TSDoc コメントを含めるようにします。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを見つけます (35 行目付近)。
2. 最後の箇条書きとして、次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存したら、会話を終了して再開します。これにより、機能に関するコンテキストを維持したまま、新しいガイダンスを読み込めます。

1. フィルターの会話が開いている場合は、`/exit` を入力します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルターの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルのコンテキストを利用する方法を確認し、新しい基準を機能に適用しました。次は、再利用可能な `quality-checks` スキルをカスタマイズして実行し、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
