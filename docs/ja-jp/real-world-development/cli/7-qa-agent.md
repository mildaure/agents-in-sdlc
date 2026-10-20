---
title: "レッスン 7 - QA エージェントを作成して使用する"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルを使って自動チェックを実行し、Playwright MCP を使ってブラウザーでフィルター機能を確認しました。次は、明確な QA プロセスを持つカスタムエージェントにこれらの機能をまとめます。

このレッスンでは、次の作業を行います。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルタリングの Issue に照らして報告をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタムエージェントを使うと QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門的な役割です。目的、指示、利用可能なツールを記載します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまで使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明します。`quality-checks` スキルは繰り返し可能なチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、要件を評価して報告する際にそれらの機能をどう使うか Copilot に伝えます。ほかの機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を作成する前に、再利用可能な QA プロファイルを作るよう Copilot に依頼します。プロファイルには、QA が実施するチェックと従うべき制約を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードになっていることを確認します。
2. 既定のエージェントに、次のカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、意図した QA ワークフローと制約が Copilot に伝わったことを確認します。これにより、検証だけを求めているのに、不完全または範囲の広すぎるエージェントが機能を変更するのを防ぎます。

1. `/diff` を実行し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は任意ですが、指定すると一覧にわかりやすい表示名が付きます。
3. プロファイルの指示を読み、QA が要件から作業を開始し、リポジトリの指示に従い、`quality-checks` スキルと Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更をコミットしたりプルリクエストを開いたりしないことを確認します。
5. これらの責務や制約が不足している場合は、続行する前に既定のエージェントへ修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトエージェントを読み込みます。プロファイル作成後、同じフィルタリングの会話を再開して QA を選択します。これにより、Issue と計画時の判断を活用できます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 新しくエージェントを作成した直後は、利用可能なエージェントの一覧に表示されない場合があります。それでも、上記のコマンドで有効にできます。

2. 次のプロンプトで、機能をレビューするよう QA エージェントに依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が終わったら、レポートを確認します。

## まとめと次のステップ

再利用可能な専門家の役割をワークフローに追加し、その作業をレビューしました。このレッスンでは、次の作業を行いました。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルタリングの Issue に照らして報告をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートがそろいました。次は、[機能 PR にまとめて Agent Merge を使用します][next-lesson]。

## リソース

- [Copilot CLI 用カスタムエージェントを作成する][create-agents]
- [カスタムエージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
