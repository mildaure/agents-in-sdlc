---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加といった作業を自律的に行えます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を委任して開発を加速し、開発者がより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次の内容を確認します。

- コード生成の環境をカスタマイズする。
- 操作を安全に行う方法を確認する。
- Issue の範囲を明確に定めることの重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。最初のサイトを作った開発者は、ドキュメントを十分に整備していません。まず、アプリケーションのすべてのエクスポート関数に TSDoc のドキュメントコメントを追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。ゲームの詳細ページに同じカテゴリのほかのタイトルを表示し、ユーザーが閲覧を続けられるようにします。まずは UX の受け入れテストに使える、作り込みすぎていない実装を求めています。この項目は現在ブロッカーですが、優先度の高い Issue がほかにもあります。

どちらも優先順位が下がりやすいタスクです。Copilot cloud agent に非同期で作業を任せれば、開発者はほかの仕事に集中し、後から結果をレビューできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]など、いくつかの方法で作業を依頼できます。割り当てると、Copilot は進捗を記録するドラフトプルリクエストを作り、環境を準備してタスクに取り組みます。実行中でも完了後でもセッションを確認できます。解決案のレビュー準備が整うと、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省けるわけではありません。一見単純な操作にも、詳しく見ると複雑な点があります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に作業し、学び、試し、調整します。生成 AI を使っても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と、Copilot に対するその役割を確認しました。カスタム指示の演習を終えていれば、`.github/copilot-instructions.md` の動作を確認済みです。まだの場合は、この機会に確認してください。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後でその影響を見つけられるようにします。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメントコメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` に適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てると、結果のプルリクエストにこれらの指示が反映されます。後の演習で PR をレビューするときに、もう一度確認します。

## Copilot cloud agent の開発環境を設定する

コードを作成するときは、通常、環境やセットアップ用スクリプトが必要です。Copilot にタスクを割り当てる場合も同様で、ソフトウェアエンジニアと同じように環境を準備します。

cloud agent は作業に [GitHub Actions][github-actions] を使います。作業開始前に実行する[特別なセットアップワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成すると、必要な開発ツールや依存関係を利用できるよう環境をカスタマイズできます。このラボでは学習をスムーズに進めるため、あらかじめ設定されています。Node.js、プロジェクトの依存関係、E2E テスト用ブラウザーバイナリ、および単一の Astro アプリ用に移行とシードが済んだ SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # セットアップ手順に必要な最小限の権限を指定します。Copilot は操作用に独自のトークンを取得します。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は contents: read が必要です。クローンしない場合、手順完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
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

      # 単一の Astro アプリ用にローカル SQLite データベースを移行し、シードデータを投入します。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub Actions ワークフローと似ていますが、いくつか特徴があります。

- `copilot-setup-steps` という1つのジョブがあります。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するまで待たずにセットアップをテストできます。

## ドキュメントを追加する

ドキュメントの重要性は誰もが理解していますが、情報が古い、あるいは不足しているプロジェクトは少なくありません。こうした技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメント作成が得意なので、cloud agent に依頼するのに適した Issue です。バックグラウンドで必要なドキュメントを作成してもらい、後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Issue を Copilot に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。PR が表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が TODO リストを作成します。

> [!NOTE]
> Copilot の TODO リストが PR に表示されるまで数分かかる場合があります。Copilot は先ほど説明したワークフローを実行して環境を構築し、プロジェクトを分析して、問題への最適な対処方法を検討します。

13. TODO リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を探します。
15. **View session** ボタンを選択します。

    ![Copilot のセッション表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ウィンドウを再読み込みしてください。

16. ライブセッションをスクロールして、Copilot の作業を確認します。コードを調べて状態を把握し、計画を検討して判断し、コードを作成する様子を確認できます。

完了まで数分かかる場合があります。Copilot cloud agent の主な利点は、非同期にタスクを実行して、開発者がほかの作業に集中できることです。その機能を活用するため、もう1つのタスクを cloud agent に割り当ててから、アプリケーションへの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の利点の1つは、作業を分担できることです。デザインチーム向けの関連ゲームセクションは時間のかかる作業ではないかもしれませんが、その時間をほかの作業に使えます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. 空のテンプレートを使うため、**Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Issue を Copilot に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、最初の Issue に対するコメントに目の絵文字 👀 が表示され、Copilot が作業中であることがわかります。

![目の絵文字で Copilot が作業中であることを示す Issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組み始めました。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後に結果をレビューできます。次は、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に Issue を割り当てると、非同期で作業を依頼できます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに活用できます。

次の内容を確認しました。

- コード生成の環境をカスタマイズする。
- 操作を安全に行う方法を確認する。
- Issue の範囲を明確に定めることの重要性を理解する。
- Issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントを作成して使用します。[Copilot cloud agent は MCP サーバーも使用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

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
