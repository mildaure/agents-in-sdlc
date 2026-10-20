---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルタリング、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、1つのブランチに保存されています。まとめてレビューし、プルリクエストを作成します。星評価の PR は手動でマージしました。今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔に進めるため、フィルタリングと品質のワークフロー全体を1つの会話とブランチで進め、この PR にまとめています。

このレッスンでは、次の作業を行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びます。
- 機能全体の変更と検証の証拠を確認します。
- フィルタリングの PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリングのワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージの承認権限を開発者が維持しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗を修正し、レビューコメントに対応し、必要に応じてリベースするなど、PR をブロックしている問題に対応します。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せられます。有効にする前に、変更をレビューして準備ができていることを確認してください。

## Agent Merge で PR を管理する

すべてのコードを作成したので、レビューして PR を作成し、残りの処理を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーション処理を監視します。すべて成功すると、マージを実行します。
7. `PR #14 was squash-merged successfully.` のような Copilot のメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限不足を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可されていればレビュー済みの変更を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証に加えて、プルリクエストの作業も自動化しました。次の作業を行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証の証拠を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウド委任に役立つ[Copilot CLI のスラッシュコマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI のコマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
