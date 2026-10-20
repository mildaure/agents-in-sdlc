---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更を繰り返し計画してコードを作成し、動作をテストし、プルリクエストをレビューして、エージェントと作業を管理します。

## シナリオ

ボードゲームのクラウドファンディングを提供する架空の企業、Tailspin Toys に新しく加わった開発者として作業します。チームのバックログは GitHub Issue として登録済みで、すぐに取り組めます。フィルターやページネーションなどの機能に加え、アクセシビリティやコーディング標準など品質向上の項目も含まれています。

## 環境を選ぶ

GitHub Copilot は、どこで作業していても利用できます。開発の進め方に合った環境を選んでください。各ワークショップは個別のセットアップから始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使用します。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを扱います。

### [GitHub Copilot app][app]

デスクトップアプリでエージェントセッションを並行して実行し、セッションモードを切り替え、キャンバスで共同作業を行い、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を導き、進捗を監視し、エージェントが作成したプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使用します。エディターを離れずに、Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを扱います。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
