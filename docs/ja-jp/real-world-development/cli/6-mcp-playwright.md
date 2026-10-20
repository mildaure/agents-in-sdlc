---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI の Playwright MCP を確認または設定し、ブラウザーでフィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くことは、コードを書くことだけではありません。データや外部サービスを扱い、Copilot が追加の自動化を利用できるようにする必要があります。そこで MCP サーバーを使います。MCP サーバーを使うと、Copilot は CLI に組み込まれている機能を超えて、さらに多くのツールやサービスにアクセスできます。

このレッスンでは、次の作業を行います。

- Model Context Protocol (MCP) と、Copilot CLI での使い方を学びます。
- Playwright MCP サーバーがまだ利用できない場合に追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

単体テストと E2E テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot がユーザーと同じように作業中の Web サイトを使えるようにすると、変更が期待どおりに動作するかをより確実に確認できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントは外部のツールやサービスと通信できます。最新情報にアクセスし、ユーザーに代わって操作を実行できます。

これらのツールやリソースには、AI エージェントと外部サービスの橋渡しをする MCP サーバーを介してアクセスします。各 MCP サーバーが、エージェントから利用できるツールとリソースを提供します。

よく使われる MCP サーバーには次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにもさまざまな MCP サーバーがあります。GitHub は、エコシステム内のサーバーを見つけやすくし、貢献を促進するために [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクトのほかの依存関係と同様に扱ってください。使用前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるよう追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、セットアップ手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類を **STDIO** にし、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

前の手順では、機能が期待どおりに動作することを手動で確認しました。次は、Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot は自動的に MCP サーバーを見つけるため、使用するよう明示する必要はありません。ただし利用すべきサーバーがわかっている場合は、プロンプトで伝えると結果の一貫性が高まり、トークンも節約できます。

2. Copilot がブラウザーで機能を確認する手順を表示する様子を見ます。
3. レポートを読み、すべてが期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作して、サーバーを停止した後、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使って、実際のブラウザーで機能を確認しました。次の作業を行いました。

- Model Context Protocol (MCP) と、Copilot CLI での使い方を学びました。
- Playwright MCP サーバーが設定されていない場合に追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認しました。

次は、スキルとブラウザーツールを専門的な役割にまとめる[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
