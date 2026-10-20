---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使用する"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能の検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことは、コードを書くことだけではありません。これまでは手動でコードの動作を確認し、指示ファイルを使って標準に従うようにしました。では、テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このようなタスクには**エージェントスキル**が適しています。スキルを使うと、Copilot がこれらの操作を適切に実行する方法を学べます。

このレッスンでは、次の作業を行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルは、再利用可能なタスクの指示、実行可能なスクリプト、関連リソースをまとめたもので、エージェントが必要に応じて読み込みます。基本的な構造はスキル名のフォルダーと、その中の Markdown ファイル `SKILL.md` です。Markdown には、スキルの名前と説明を定義する frontmatter、スキルの概要、呼び出すべきタイミングに関するガイダンスを記載します。フォルダーにスクリプトやその他のリソースを含むサブフォルダーを追加することもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルには `SKILL.md` だけがあります。プロジェクトで既存のコマンドを使うためです。

スキルは、チームが共有して再利用できるプロジェクトの `.github/skills` フォルダー、またはユーザーの `~/.copilot/skills` フォルダーに配置できます。

## スキルを確認する

Tailspin Toys のチームが単体テスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認します。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。description により、Copilot はスキルを呼び出すタイミングを判断します。
3. 指示を読み、テストと lint の手順がどのようにガイドされているか確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

Copilot CLI から直接スキルを呼び出すか、自然言語で依頼できます。3つのチェックを実行するよう Copilot に依頼します。

1. Interactive モードでフィルタリングの会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したか、ツールが実際に報告した内容がわかるレポートに改善します。

1. `.github/skills/quality-checks/SKILL.md` をもう一度開きます。
2. ファイル末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、スキルを実行します。Copilot CLI は会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前と**まったく同じプロンプト**を使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後のレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使用しました。このレッスンでは、次の作業を行いました。

- 単体テスト、lint、型チェックに使う `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、フィルタリング機能の PR に含まれます。次は、[Playwright MCP サーバー][next-lesson]を通じて Copilot がサイトを直接操作できるようにします。

## スキルの例

以下のコミュニティ例は参考資料です。追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [貢献ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とバンドルされたエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
