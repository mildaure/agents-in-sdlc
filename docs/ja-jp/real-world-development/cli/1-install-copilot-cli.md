---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本操作を確認して、用意されたフィルタリングの Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディングアシスタントです。コマンドラインからコードベースを調べ、コードを生成し、コマンドを実行して外部ツールと連携できます。タスクを任せたり、変更を依頼したりしながら、集中して作業できます。最初にツールをインストールします。使い慣れたツールを使って簡単にインストールできます。

このレッスンでは、次の作業を行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼して、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーを使ってフィルタリングの Issue を見つけます。

## シナリオ

チームは増え続けるバックログに対応するため、AI エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルでその機能を利用できるようにします。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで使える状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、この演習では npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされており、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub アカウントで認証するよう求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、device code の手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されているパスが Tailspin Toys のリポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めて問題ありません。

## 基本操作を確認する

通常のシェルプロンプトに入力したコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語でエージェントに依頼し、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力してインストール済みのバージョンで使えるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. 次の簡単な質問をして、動作を確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. Copilot が回答前にリポジトリを調べる様子を確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが使用可能であることを確認します。
6. 次のプロンプトを使い、フィルタリングの Issue を探します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて Issue を読みます。簡単な変更を終えた後に使用します。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトしているブランチを使い、自動で worktree を作成しません。変更を始める前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は、設定済みの権限を超えるツールを使う前に、通常は承認を求めます。このワークショップでは Codespace 内での演習に集中するため、`--yolo` を付けて再起動します。これは利用者が承認したショートカットで、承認プロンプトを省略します。

> [!CAUTION]
> `--yolo` は `--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls` を含む完全な自動権限を有効にします。Codespace や VM のように分離された環境でのみ使用してください。日常的な開発の既定値として alias を設定しないでください。詳細は[ツール利用の許可と拒否][allow-all-warning]を参照してください。

このワークショップでは、`--enable-all-github-mcp-tools` によって、後のレッスンで Issue やプルリクエストを扱うための GitHub MCP の読み取り / 書き込みツールを有効にします。Codespace によってローカルコンピューターへのアクセスは制限されますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をして会話が動作することを確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりしたら、`copilot --resume` で同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次の作業を行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼して、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーを使ってフィルタリングの Issue を見つけました。

次は、[最初の対象を絞った変更][next-lesson]を行い、Copilot CLI を使ってゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI のコマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
