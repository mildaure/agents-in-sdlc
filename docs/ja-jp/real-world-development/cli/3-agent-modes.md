---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で実装方針を決め、Autopilot で Issue からフィルター機能を構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に小さな機能をプロジェクトに追加しました。大きな変更には、より確実なプロセスが必要です。GitHub Copilot CLI は組織の既存の作業方法に合わせて使えるため、適切なものを適切な方法で作成できます。このレッスンから、エージェントを使った一般的な開発プロセスに沿って、Issue から機能を作成し、コードの妥当性と機能の動作を確認し、最終的にプロジェクトへマージする複数レッスンのワークフローを始めます。

> [!NOTE]
> 機能のワークフローを続ける間は、同じ会話とブランチを使います。通常はファイル種別ごとに別のブランチや PR を使う場合がありますが、ここでは中心的な概念に集中するため、手順を簡略化します。

このレッスンでは、次の作業を行います。

- GitHub Issue から Copilot の新しい会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この機能開発を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP 検証を追加し、QA エージェントを作成して機能 PR を作成します。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がカテゴリと publisher でゲームを絞り込めるようにする必要があります。バックログの Issue には機能が記載されていますが、複数カテゴリの組み合わせなど、コーディング前に決めておくことがあります。Plan モードで方針を決め、Autopilot に範囲を定めた実装を依頼します。

## 背景

AI コーディングエージェントを開発ワークフローに導入しても、基本は変わりません。むしろ、重要性が増します。多くの開発者は次のような流れで作業します。

1. 必要な作業を説明する Issue から始めます。
2. 実装する内容の計画を立てます。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細はチームや組織によって異なりますが、多くの場合、上記の流れを基にしています。

この標準的な進め方を守ることで、AI が生成したコードも要件を満たし、手作業で書いたコードと同じレビューを受けられます。

## 会話モード

**会話モード**によって、エージェントの自律性が変わります。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、続行する前に入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントがコードを書き、テストを実行し、タスクの完了まで繰り返し作業します。

最初に Plan モードで計画を確認してから、Autopilot で実装します。

## Issue から始める

フィルタリング作業を始める前に、Codespace に戻り、リポジトリとターミナルの準備を確認します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新して、フィルタリング作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まっていることを確認します。`#` を使うと、GitHub の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、Copilot に作業を任せる前に、機能の実装やタスクの進め方を定められます。複雑な作業では、少し時間をかけて計画するのがお勧めです。Plan モードに切り替えて、計画を作成するよう Copilot に依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方針を検討して計画を作成します。

3. フィルタリング機能の動作について質問された場合は、希望に基づいて回答します。ここに唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. スクロールして計画をレビューします。
6. 希望と異なる部分があれば、修正を依頼します。

## Autopilot を承認する

計画の作成とレビューが終わったので、実装に進みます。Autopilot を使って Copilot に作業を任せます。

Autopilot では、タスクが完了したと判断するまで Copilot が繰り返し作業します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンで同様の名前の項目を選択します。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が合意した計画に沿って作業し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まることがあります。先に計画をレビューしてください。依存関係が見つからない、またはポートが競合していると Copilot が報告した場合は、チェック完了と見なす前にセットアップの問題を解決してください。

## 実装をレビューして検証する

ほかのコードと同じように、生成されたコードもマージ前にレビューします。コードとサイトを確認します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードにします。モード表示に **Plan** または **Autopilot** がないことを確認します。
2. `/diff` を実行し、フィルタリングの実装とテストを確認します。
3. 結果を Issue と計画時に決めた内容と比較します。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新しい機能を確認する

コードは問題なさそうですが、実際に動作するでしょうか。前と同じようにアプリを起動し、Codespaces の転送ポート経由でサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能と表示されたら、**Open in Browser** を選択します。
3. カテゴリ、publisher、および計画で合意した組み合わせによるフィルタリングを試します。
4. リセットと結果が空の場合の動作が、Issue と決めた内容に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

会話モードを切り替えて機能を構築し、レビューしました。このレッスンでは、次の作業を行いました。

- GitHub Issue から Copilot の新しい会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、カスタム指示を使って[コード生成の方法を確認し、文書化されたプラクティスに従わせます][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI のコマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
