---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進めたい場合や、Copilot が知っておくべき背景情報がある場合は、その情報を利用できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。主な方法の1つが[指示ファイル][instruction-files]です。指示ファイルを使って、コード生成に関するガイダンスを伝えます。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパス単位の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- 現在の指示を使ってフィルタリング用のデータスライス (publishers ヘルパー) を最初に生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。
- 同じプロンプトを再度実行し、生成コードに新しい標準が反映される様子を確認します。
- 指示の更新とフィルタリングのデータスライスを `main` にコミットし、次の演習で Copilot が使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準の一部に沿わない場合があります。Copilot の結果は確定的ではありません。この演習では、指示を更新した後の動作の**傾向**を確認します。出力の文字列を完全に一致させることが目的ではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発のガイドラインと要件があります。たとえば、次のようなものです。

- データレイヤーには常に単体テストが必要です。
- UI はダークモードで、現代的な見た目にします。
- コードには TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの実践に沿って作業できるよう必要な情報を伝えます。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを得られます。推奨するコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定のファイル種別やタスクに適用する指示を作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に渡される単一の指示ファイルです。技術スタック、開発対象の概要、ベストプラクティスなど、多くのチャットや CLI リクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けの指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントや単体テストの作成などのタスク向けにガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準の指示ファイル][custom-instructions-support]にも対応しています。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することはこの演習の範囲外ですが、サンプルプロジェクトには一般的な方法が示されています。

- `copilot-instructions.md` には、開発対象の概要、プロジェクト構造、全体的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` は、単体テスト、Astro コンポーネント、データレイヤーなどのファイル種別や、特定のタスク向けに使用します。
- 自然な言葉で、明確なガイダンスとコードの例を記載します。

指示ファイルの作り方は1つではありません。AI の使い方と同じように、試しながら自分の環境やシナリオに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルをそろえることをお勧めします。このプロジェクトのファイルを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、多様なタスク向けの指示があります。
>
> Copilot に指示ファイルを作成してもらうこともできます。操作方法は利用環境によって異なります。たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使用します。利用中の環境に対応するレッスンで、関連する操作を説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。共通の `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` があります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot とのすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これはリポジトリルートからの相対 glob で適用対象を指定します。たとえば `**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトで単体テストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。これにより、大きな指示を再利用できる小さなファイルに分け、コード生成時に参照する例を Copilot に示せます。記載されたパスは、リポジトリルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で TSDoc のドキュメントコメントとファイルコメントヘッダーのルールを追加します。

## 変更用のブランチを作成する

ベストプラクティスに沿って、変更用ブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot に必要なコンテキストだけが渡るようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを置く場所を Copilot に示します。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べてコードを更新します。ヘルパーファイルとテストの両方が変更されることがあります。
6. 提案されたヘルパーは、最初の引数に `db` クライアントを受け取り、publisher の型付き配列を返す関数です。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 提案されたコードに TSDoc のドキュメントコメントとファイルコメントヘッダーが**含まれていない**ことを確認します。

> [!CAUTION]
> Copilot の結果は確定的ではないため、指示がなくてもドキュメントコメントが追加される場合があります。その場合も問題ありません。この演習の目的は、指示の更新によって一貫性が向上することを確認することです。

## リポジトリ標準を追加する

前述のように、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コード提案を改善するため、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27行目付近にある **Code formatting requirements** セクションを探します。ここにはコーディング標準が記載されていますが、コード内ドキュメントのルールはありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準のすぐ下に、ファイルコメントヘッダーと TSDoc ドキュメントコメントを Copilot に追加させる次の Markdown を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、`.github/copilot-instructions.md` でリポジトリ全体に適用する指示を作成できます。また、言語、ファイル種別、タスクごとの `*.instructions.md` も作成できます。ここで追加したドキュメントコメントのルールのようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## 同じプロンプトを再度実行し、違いを確認する

1. Copilot Chat に戻り、**New Chat** を選択してバッファーをクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot に対象ファイルを示します。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準を適用させ、次の演習で使う実際のコードに反映できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもほかのリポジトリ資産と同様に、ソース管理で管理します。変更をコミットしてリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. 次のコマンドを実行し、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. ターミナルから指示の更新と、Copilot がヘルパー作成時に変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたので、バックログにあるドキュメント標準の Issue に関連付けてプルリクエストを作成します。手動で行うこともできますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に作成させます。該当する Issue を探し、それを完了する PR を作成してからマージするよう依頼します。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのは今回が初めてです。プロジェクトのテンプレートには GitHub のツールがあらかじめ設定されています。そのため、セットアップは不要です。初めてツールを使うときに、VS Code から GitHub へのサインインを求められる場合があります。画面の指示に従ってください。この接続の仕組み (Model Context Protocol) は後の演習で確認します。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** を選択します。
3. 指示ファイルの更新に関連する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新に加えて、今後のフィルタリング作業に向けた publishers ヘルパーの基盤を追加したことも説明するよう伝えます。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot に代わって GitHub の操作を **Allow** するよう求められた場合は、コマンドを確認し、適切であれば **Allow** を選択します。
6. PR の作成後、Copilot に PR をマージしてブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot に代わって GitHub の操作やシェルコマンドの実行を **Allow** するよう求められた場合は、コマンドを確認し、適切であれば **Allow** を選択します。

GitHub Copilot の支援を使って、プルリクエストを作成し、マージできました。

## まとめと次のステップ

指示ファイルから Copilot がコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- 既存の指示を使ってコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- 指示の更新とヘルパーの基盤をコミットして `main` に追加し、次の演習で使えるようにしました。

次は、Copilot がコードベース全体に新機能を追加するときに、[エージェントモード][next-lesson]で指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
