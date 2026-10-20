---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクの進め方を指定したい場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]には、コードで実現したいことだけでなく、コードの構成方法も記載できます。フィルター機能を作成したので、Copilot が使った指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次の作業を行います。

- リポジトリの指示とパス単位の指示ファイルがエージェントに伝わる仕組みを確認します。
- コーディング標準に沿うよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- コメントにはコードを言い換えるのではなく、意図や自明でない判断を記載します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記載します。注入可能な `db` 引数がある場合は、それも説明します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記載します。関連コードの変更時はコメントも最新の状態に保ちます。
- 既存の書式設定と lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの実践に沿ってタスクを実行できるよう、必要な情報を伝えます。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを得られます。推奨するコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定のファイル種別やタスクに適用する指示を作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に渡される単一の指示ファイルです。多くのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイドラインを記載します。

> [!NOTE]
> その他の指示形式への対応状況は環境によって異なります。利用する前に[カスタム指示のサポート状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、すでに指示ファイルが含まれています。変更前にその内容を確認し、影響を把握します。

1. Codespace に戻ります。
2. Codespaces エディター (ターミナルではありません) で `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示は、リポジトリ内の Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどの指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この値が指示の適用対象となるファイルの glob を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存ファイルへのリンクを確認します。大きな指示を再利用できる小さなファイルに分け、コード生成時に参照する例を Copilot に示せます。

## チームのガイダンスに沿って指示ファイルを更新する

既存の指示はよい出発点ですが、TypeScript の新しいコードに TSDoc コメントを追加するよう、コアの `copilot-instructions.md` を更新します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します。35行目付近にあります。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存した後、フィルタリングの会話を終了して再開します。これにより、機能のコンテキストを失わずに新しい指示を読み込めます。

1. フィルタリングの会話が開いている場合は `/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. 次のプロンプトで、更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を実行して、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。次の作業を行いました。

- リポジトリの指示とパス単位の指示ファイルがエージェントに伝わる仕組みを確認しました。
- コーディング標準に沿うよう、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、lint とテストを一貫して実行するために、再利用可能な quality-checks スキルを[カスタマイズして実行します][next-lesson]。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート状況][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
