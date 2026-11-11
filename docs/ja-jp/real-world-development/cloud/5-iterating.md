---
title: "演習 5: GitHub Copilot の作業を改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

このラボでは、ユーザー体験を改善して機能を追加する複数のタスクで GitHub Copilot と作業しました。コードへのドキュメント追加、デザインチームが試せる関連ゲーム機能の作成、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コード変更を確認し、必要に応じて Copilot にフィードバックを送ります。

## 作業をレビューする

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに従います。Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能を確認してから、チーム全体にレビューを依頼します。

### シナリオ

これまでに何度も確認したとおり、生成 AI を導入してもソフトウェア設計の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスを進めます。チーム全体のレビューを有効にする前に、Copilot がドキュメント、関連ゲーム機能、アクセシビリティ機能について提案した内容を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は監督者なしで非同期にタスクを実行するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスと、コードを置くブランチへの書き込みアクセスのみを持ちます。
- cloud agent は GitHub Actions 内で、作業用の一時的な環境を作成します。
- GitHub Actions のワークフローは、実行前に人間の承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは既定で制限][agent-firewall]されています。

## 生成されたドキュメントをレビューする

最初に、コードへのドキュメント追加を依頼した pull request (PR) を GitHub.com の標準的な PR インターフェイスで確認します。

> [!NOTE]
> PR を開くと、GitHub Copilot がファイアウォールでブロックされているという警告が表示されることがあります。Copilot の外部リソース (外部 MCP サーバーへの呼び出しなど) へのアクセスは既定で制限されているため、これは想定された動作です。必要であれば、[Copilot cloud agent のファイアウォールを設定または無効化][agent-firewall]できます。

1. 新しいブラウザータブで github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。作業が完了するまで待ちます。数分かかることがあるので、休憩を取るか、学習した内容を振り返ってください。

4. **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 追加された TSDoc コメントやその他のドキュメントを確認します。演習 2 で確認したカスタム指示ファイルによって、TSDoc、TypeScript の規約、ファイルヘッダーが反映されています。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す通知を確認します。
8. 承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェックセクションにワークフローが追加されることを確認します。単一の Astro アプリのプロジェクトチェックが成功するまで数分かかることがあります。

## GitHub Copilot に変更を依頼する

PR 上のコメントやコードへのインラインコメントで、チームメンバーと同じように **@copilot** をメンションして変更を依頼できます。Copilot はコメントを確認し、更新のために新しいセッションを開始します。結果は確率的に変わるため、指定の文面はありません。次のような変更を依頼できます。

- 各コードファイルの先頭に、内容を説明するコメントヘッダーを追加します。
- TypeScript と Astro ファイルに TSDoc コメントを追加します。
- Astro アプリの構成を説明する README を作成します。

1. 上記の案や、コードベースに必要な変更を選び、**@copilot** をメンションしてコメントを追加します。
2. **View Session** を選択し、Copilot の作業を確認します。
3. **Back to pull request** を選択して PR に戻ります。

    ![pull request に戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了したら、PR に新しいコミットがあることを確認します。
5. **Files changed** タブを選択して変更をレビューします。

満足するまで改善を繰り返せます。問題がなければ、下書き PR をレビュー可能な状態にして `main` にマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページへの関連ゲーム表示を依頼した PR をレビューします。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちになっていることを示す通知を確認します。
7. 承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. PR のチェックにワークフローが追加され、単一の Astro アプリのプロジェクトチェックが成功することを確認します。完了まで数分かかることがあります。
9. **省略可能:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。ターミナルを開き、Copilot が作成した `<branch-name>` (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot は関連ゲーム機能を作成しました。必要に応じて、表示する関連ゲームの数やコメントヘッダー、TSDoc コメントについて、PR の **Conversation** タブに新しいコメントを追加して更新を依頼できます。関連ゲーム機能はカスタム指示を追加する前に割り当てたため、指示が反映されていない場合があります。

## アクセシビリティ機能をレビューする

アクセシビリティカスタムエージェントで実装した機能を確認します。PR には、演習 3 で依頼したハイコントラストモードと、演習 4 で追加依頼したライトモードが含まれます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。完了まで待ちます。

4. **Files changed** タブを選択して変更をレビューします。
5. 次の項目に注意して実装を確認します。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を保持する local storage の使用
   - ハイコントラストモードとライトモードの CSS またはスタイル
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript コード
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す通知を確認します。
8. 承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェックにワークフローが追加され、単一の Astro アプリのプロジェクトチェックが成功することを確認します。
10. **省略可能:** Copilot のブランチを Codespace でチェックアウトして、アクセシビリティ機能を手動でテストできます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

アプリを起動し、ハイコントラストモードとライトモードを切り替えて、再読み込み後も設定が保持されることを確認します。カスタムアクセシビリティエージェントがアクセシビリティのベストプラクティスに沿って実装したかを確認します。問題や改善点があれば、PR のコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 — 委任を続ける

cloud agent にはバックログの実際のタスクを任せ、自分は他の作業に取り組めます。次のアイデアで Issue を作成し、Copilot に割り当ててみてください。

- ゲーム詳細ページに支援者向け問い合わせフォームを作成します。
- ゲーム一覧ページにページネーションを実装します。
- データアクセスヘルパーに入力検証とエラー処理を追加します。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンでは、次のことを行いました。

- リポジトリに用意されたカスタム指示ファイルを確認し、cloud agent の出力に反映される様子を学びました。
- Issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期の作業を確認しました。
- アクセシビリティ向けのカスタムエージェントを作成して使い、ハイコントラストとライトモードの切り替えを追加しました。
- Copilot agents ページを mission control として使い、アクセシビリティセッションを監視して方向付けました。
- Copilot の pull request をレビューし、`@copilot` に変更を依頼して、ワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。他の環境のハーネスでは、ここで学んだことを別の方法で実践できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、agent skills、QA エージェント、Playwright MCP、Agent Merge、任意の `/delegate` を使い、ターミナルからレビュー済みの変更を2つ作成します。

自分のリポジトリでは、次のことも試してください。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てます。
- 別の分野のカスタムエージェントを作成します。
- 別のスタック用に `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォール設定][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
