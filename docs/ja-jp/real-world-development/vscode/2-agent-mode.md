---
title: "演習 2 - Copilot Agent Mode で新しい機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションへの簡単な変更でも、通常は複数のファイルを更新し、テストなどを実行する必要があります。開発者は必要なファイルを探し、変更を加え、テストを実行し、デバッグし、見落としたファイルを探してさらに変更する、といった手順を繰り返します。

こうした作業に Copilot Agent Mode が役立ちます。

Copilot Agent Mode は IDE 内でより自律的に動作します。開発者と同じように、まず既存のプロジェクト構造を調べ、必要な更新を行い、テストなどのタスクを実行して、見つかった問題を自動で修正します。Agent Mode を使ってサイトに機能を追加する方法を確認します。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる体験向けに作られています。エージェントモードは IDE 内で作業し、すばやくフィードバックを得ながら対話できます。cloud agent はチームメンバーのように非同期で作業し、Issue やプルリクエストを通じてやりとりします。

この演習では、次の方法を学びます。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数ファイルにまたがる変更を行う。
- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる新機能を実装する。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリと publisher で絞り込めるようにします。前の演習で publishers ヘルパーを追加しました。Copilot Agent Mode を使い、残りのデータレイヤー、UI、テストを仕上げます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲーム一覧と各ゲームの詳細を表示します。単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。ページの frontmatter から Drizzle ORM を通じてローカルの SQLite データベースを直接照会するため、別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな動作は標準 DOM API を使った小規模な Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。次の手順で GitHub Codespace から実行できます。

1. Codespace に戻ります。前の演習を終えていれば `main` にいます。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルを開きます。
3. フィルター機能の作業用ブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くか尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動でリダイレクトされます。これは Codespace の Web サーバーへのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認できます。主な機能は次のとおりです。

- **ホームページ**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、publisher、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、機能を追加して改善します。まずバックログを確認しましょう。テンプレートからプロジェクトを作成したときに Issue のバックログが自動で作成されています。GitHub Copilot に一覧を表示してもらいます。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。**Agent** は自律的に計画を立て、複数ファイルを変更し、ターミナルコマンドやツールを実行します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。作成時に Claude Sonnet 4.5 を使ったため、体験をそろえる目的で手順に記載しています。LLM の出力は確定的ではないため、提案が手順と異なる場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使うことがありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信し、Issue のバックログについて質問します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が、ユーザーにとって役立つと思われる順に項目を優先付けしたことを確認してください。

## 指示ファイルを確認する

コード生成エージェントを起動する前に、Copilot にコンテキストを伝える指示ファイルを確認します。Web サイトに機能を追加する方法が記載された[ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。内容には次のものがあります。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト容易性、アクセシビリティの原則。
   - 次のようなファイル種別ごとの指示へのリンク。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示の再利用や複雑な内容の分割が容易になり、コード生成時に例やテンプレートを Copilot に示せます。

## フィルタリング機能を実装する

フィルタリング機能を完成させるには、少なくとも次の3つを個別に更新する必要があります。

- データレイヤー (`src/lib/`) のフィルタリングロジックを追加または調整する。
- フィルタリング動作のテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

さらに、コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要なファイルを特定します。データレイヤーのヘルパー、UI、テストを見つけ、ファイルを変更してテストを実行する様子を確認します。

   ![Copilot がプロジェクトのファイルを調べているスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによって数分かかることがあります。作業中にエディターに **Keep** と **Undo** が表示される場合があります。Copilot が完了すると、すべての変更を確認できるため、作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテスト実行を確認するダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 途中で待ち時間が発生したり、テストが失敗したりする場合があります。問題ありません。Copilot はコード生成とテストを繰り返し、タスクを完了してエラーがないと判断するまで作業します。

   ![Copilot Agent Mode とのチャットが完了したスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やその他の AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を確認します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys に機能を追加する方法を確認しました。次の内容を学びました。

- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる機能を実装する。
- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数ファイルにまたがる変更を行う。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナス演習 — ページ分割を実装する

ゲームの一覧が増えると、ページ分割が必要になります。この演習で学んだスキルを使い、Copilot にページ分割を実装するよう依頼します。コードの検討事項は次のとおりです。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従います。
- ユーザーがページサイズを選べるようにするか、固定値にするかなど、ページ分割の方法を検討します。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot に変更を依頼し、コンテキストを追加しながら反復する必要がある場合があります。これは Copilot を使う通常の流れです。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
