---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の向上や機能追加に関する複数のタスクを GitHub Copilot と進めてきました。コードへのドキュメント追加、デザインチーム向けの関連ゲーム機能の構築、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コード変更を確認し、必要に応じて Copilot にフィードバックを送り、作業を改善します。

### シナリオ

これまで何度も確認したように、生成 AI を使ってもソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チームのレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期かつ監督なしでタスクを実行するため、安全性を保つためのセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、コードを書き込めるのは作業用ブランチ**だけ**です。
- cloud agent は GitHub Actions 上に、作業用の独立した一時環境を作成して実行されます。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR) を確認します。これはコードへのドキュメント追加の PR です。GitHub.com の標準的な PR インターフェイスで作業します。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は MCP サーバーなどの外部リソースへのアクセスが既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** と表示されます。作業が完了するまで待ってください。数分かかることがあります。その間に休憩したり、これまでに学んだ内容を振り返ったりできます。

4. 準備ができたら **Files changed** タブを選択し、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく更新されたコードを確認します。TSDoc のドキュメントコメントなどが追加されています。変更内容は実行ごとに異なります。

   変更を読む際は、TSDoc のコメント、TypeScript の規約、ファイルコメントヘッダーを確認します。これらは演習 2 の最初に確認したカスタム指示ファイルに基づいています。

6. 更新内容を確認したら **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。正常に完了すると、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot とやりとりする方法は一方通行ではありません。チームメンバーと同じように、PR のコメントやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、修正するための別のセッションを開始します。結果は確定的ではないため、依頼文を一律には指定できません。たとえば、次のような更新を頼めます。

- 各コードファイルの先頭に、役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. ドキュメントの変更を依頼するコメントを追加し、ほかのユーザーと同様に **@copilot** をメンションします。上記の例、またはコードベースのドキュメントに関する別の依頼を使用してください。
2. **View Session** を選択して Copilot の作業を確認します。Copilot が更新のために新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更を確認します。

満足するまで反復して改善してください。完了したら、PR をドラフトからレビュー可能な状態に変更し、`main` ブランチにマージできます。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue について、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新を確認したら **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示があることを確認します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄にワークフローが追加されます。正常に完了すると、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。ほかの作業と同じように、cloud agent に更新を依頼して反復できます。たとえば、表示する関連ゲーム数を調整したり、コメントヘッダーや TSDoc のコメントを追加したりできます。この Issue を割り当てたのはカスタム指示を更新する**前**であることに注意してください。前と同じように **Conversation** タブでコメントを追加すると、Copilot が内容を確認し、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 のミッションコントロールで追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot が作業中の場合、PR に **[WIP]** と表示されます。作業が完了するまで待ってください。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認するときは、特に次の内容に注目してください。
   - 表示モードを切り替える UI コンポーネント
   - ユーザー設定を保存する Local Storage の使用
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モードの切り替えを管理する JavaScript/TypeScript コード

6. 更新を確認したら **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。正常に完了すると、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、`<branch-name>` を Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリケーションを起動し、ハイコントラストモードとライトモードの切り替えが動作し、ページを再読み込みしても設定が維持されることをブラウザーで確認します。

アクセシビリティ用カスタムエージェントが、ベストプラクティスに沿った機能の実装に役立ったことを確認してください。アクセシビリティ上の懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** に修正を依頼できます。

## 任意の演習 — 追加のタスクを委任する

cloud agent に実際のバックログ項目を任せ、自分は別の作業に取り組む方法を練習します。リポジトリに Issue を追加して Copilot に割り当てます。たとえば、次のような作業を依頼できます。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。これらのレッスンでは、次の作業を行いました。

- このリポジトリに含まれるカスタム指示ファイルを確認し、cloud agent の出力への影響を学びました。
- Issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期の実行を確認しました。
- アクセシビリティ用カスタムエージェントを作成して使用し、ハイコントラストモードとライトモードの切り替えを追加しました。
- Copilot agents ページをミッションコントロールとして使い、進行中のアクセシビリティセッションを監視して指示を出しました。
- cloud agent のプルリクエストをレビューして反復し、**@copilot** に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。ほかの環境でも学びたい場合は、次のハーネスを確認してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` 連携を使い、レビュー済みの変更をターミナルから2つ提出します。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けにカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

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
