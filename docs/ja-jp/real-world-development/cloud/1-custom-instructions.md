---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進めたい場合や、Copilot が知っておくべき背景情報がある場合は、その情報を利用できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」してほしいかだけでなく、「どのように」進めるかも伝えられます。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパス単位の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期に動作するため、リアルタイムで並べて比較するのが難しいからです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発のガイドラインと要件があります。たとえば、次のようなものです。

- データレイヤーには常に単体テストが必要です。
- UI はダークモードで、現代的な見た目にします。
- コードには TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの実践に沿って作業できるように必要な情報を伝えます。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを得られます。推奨するコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用する指示も、特定のファイル種別やタスクに適用する指示も作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に渡される単一の指示ファイルです。多くの Copilot のチャットや CLI リクエストに関係するプロジェクト全体の情報を記載します。たとえば、技術スタック、開発対象の概要、ベストプラクティス、全体的なガイダンスなどです。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けの指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントや単体テストの作成などのタスク向けにガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準の指示ファイル][custom-instructions-support]にも対応しています。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することはこのワークショップの範囲外ですが、サンプルプロジェクトには一般的な方法が示されています。

- `copilot-instructions.md` には、作成対象の概要、プロジェクト構造、全体的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` は、単体テストや Astro コンポーネントなどのファイル種別、または特定のタスク向けに使用します。
- 自然な言葉で、明確なガイダンスとコードの例を記載します。

指示ファイルの作り方は1つではありません。AI の使い方と同じように、試しながら自分の環境やシナリオに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルをそろえることをお勧めします。このプロジェクトの指示ファイルを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、多様なタスク向けのファイルが見つかります。
>
> Copilot に指示ファイルを作成してもらうこともできます。操作方法は利用環境によって異なります。たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使用します。利用中の環境に対応するレッスンで、関連する操作を説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。共通の `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` があります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot とのすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これは、リポジトリルートからの相対 glob で適用対象を指定します。たとえば `**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトで単体テストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。これにより、大きな指示を再利用できる小さなファイルに分け、コード生成時に参照する例を Copilot に示せます。ここに記載されたパスは、リポジトリルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc のドキュメントコメントとファイルコメントヘッダーのルールを追加します。

## リポジトリ標準を追加する

ここで行う編集は、プロジェクト全体に適用するルールを追加することです。コード内のドキュメントは TSDoc のドキュメントコメントで記述し、ファイルレベルのコメントヘッダーも追加するよう定めます。このハーネスの後半で Issue を割り当てると、cloud agent がこのルールを適用します。

編集する前にブランチを作成します。cloud agent は Issue の対象ブランチにある指示を読み取りますが、ここでは変更を個別にコミットします。

1. Codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のように、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コード提案を改善するため、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27行目付近にある **Code formatting requirements** セクションを探します。ここにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントのルールはありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準のすぐ下に、TSDoc ドキュメントコメントとファイルコメントヘッダーを追加する次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、リポジトリ全体に適用する `.github/copilot-instructions.md` と、言語、ファイル種別、タスクごとの `*.instructions.md` の両方で指示を作成できます。TSDoc のコメントルールのようなプロジェクト全体の標準は、リポジトリレベルのファイルに記載します。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は Issue が対象とするブランチの指示ファイルを読み取ります。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。指示の変更を cloud agent に適用させるには、変更を `main` にマージする必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成し、マージします。次の演習で Issue に割り当てると、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチの作成を省略し、変更を `main` に直接コミットしてもかまいません。ここでは実際のプロジェクトでの作業方法に近づけるため、ブランチを使っています。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを得る方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。このハーネスの後半で cloud agent が作成するプルリクエストに、この標準が適用されます。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
