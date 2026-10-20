---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

直近の演習では、ユーザー体験の向上と機能追加に関する3つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクの監視も重要です。

cloud agent に割り当てたタスクを管理するツールは多数あります。たとえば GitHub.com の[エージェントページ][agents-page]を使うと、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行した操作を確認し、進行中のセッションに指示を加えることもできます。

このレッスンでは、次の作業を行います。

- エージェントページで cloud agent のタスクを監視します。
- 実行中のセッションに機能の追加を依頼します。

## シナリオ

エージェントにハイコントラストモードの作成を依頼した後、チームはライトモードも追加したいと考えました。サイトのスタイルと切り替え機能を更新中なので、ライトモードも含めるのが自然です。ハイコントラストモードに加えてライトモードも実装されるよう、エージェントに指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン領域と左側のペインにあるタスクの一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲームを変更する API を生成する。
   - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを1つ選択します。Copilot が実行した次のような作業を確認できます。
   - リポジトリのチェックアウト。
   - Copilot が作業する環境の作成。
   - MCP サーバーの設定。
   - 割り当てられたタスクを完了するための各種作業。

> [!NOTE]
> 表示される具体的な手順は、Copilot の作業状況や選択したアプローチによって異なります。

4. 右側に表示されるプルリクエスト (PR) のペインも確認します。PR と変更されたファイルを表示し、さらに詳しく監視できます。

## cloud agent に指示を追加する

実行中のタスクを確認したので、ハイコントラストモードの作業中にライトモードの切り替えも実装するよう依頼します。

1. ハイコントラストモードを追加するセッションを選択します。Copilot が付けた名前や作業状況によって、タイトルは異なります。

    ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業を開始したことが表示されるまで数分待ちます。作業が始まると、次のようなメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページの cloud agent タスクで、作業中のセッションに指示を追加するダイアログを強調表示したスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に追加することを確認します。

## Copilot に作業を任せる

前回と同様に、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行中の手順が完了すると、新しい依頼が作業に反映されます。

完了まで数分かかります。その間に、これまで学んだことを振り返ってください。

## まとめと次のステップ

Copilot cloud agent のタスクを監視して指示を出す中心的な場所である、Copilot agents ページを確認しました。進行中と完了済みのタスクを追跡し、作業内容をレビューして、実行中のタスクに追加の指示を出せます。

次の内容を確認しました。

- ミッションコントロールとエージェントページを使って cloud agent のタスクを監視しました。
- 実行中のセッションに指示を追加し、機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、Copilot が作成したプルリクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
