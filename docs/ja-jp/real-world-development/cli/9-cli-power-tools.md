---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト管理、モデル選択、セッション共有、任意のクラウド委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同じように、GitHub Copilot CLI には操作に使えるスラッシュコマンドが多数あります。高度な機能、内部情報、追加の設定オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ほかの便利なコマンドも確認します。

このレッスンでは、次の作業を行います。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の中心的なワークフローが終わりました。コンテキスト管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルのコンテキストウィンドウの上限に達することがあります。Copilot CLI は必要に応じて自動的に会話を圧縮します。また、スラッシュコマンドでコンテキストを確認したり、自分で圧縮したりできます。

1. Codespace に戻り、Copilot CLI が起動していない場合はリポジトリルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどう分割されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を実行して結果を比較します。会話が短い場合は、大きく変化しないことがあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的に圧縮します。圧縮するタイミングを選びたい場合は `/compact` を使います。無関係なタスクに切り替えるときに新しい会話を始めるには、`/clear` または `/new` を使います。

## モデルを選択する

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用できるモデルを一覧表示して選べます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで作業し、学んだことを共有すると、AI ツールの活用を互いに改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を実行し、インストール済みのバージョンで `/share` が利用できる方法を確認します。
2. セッションを共有する場合は、目的に合う保存先を選びます。たとえば `/share file` はローカルの Markdown ファイルにエクスポートします。
3. エクスポートした内容を送信または公開する前に確認します。プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を拡張または委任する

中心的なワークショップは完了しました。Copilot CLI には、大きなタスクに使えるコマンドもあります。

- `/fleet` は独立したサブタスクをサブエージェントに分け、並列で実行できます。
- `/worktree` は別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は Copilot cloud agent にタスクを送信します。非同期で作業し、プルリクエストを作成する場合があります。

これらのコマンドは追加の worktree やリモートの作業を作成する可能性があるため、任意です。試す前に、新しく範囲を明確にしたタスクを開始し、通常のワークフローに沿って結果をレビューしてください。非同期エージェントの作業をさらに学びたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使って設定し、セッションを共有し、内部の動作を確認しました。このレッスンでは、次の作業を行いました。

- `/context` と `/compact` で会話のコンテキスト管理を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI にはほかにも多くのスラッシュコマンドがあります。これまでの内容を[振り返り][next-lesson]、学習を続けるための次のステップを確認しましょう。

## リソース

- [Copilot CLI のコマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
