---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローに取り組みました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されたフィルタリングの Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの Issue から始め、Plan モードで方針を決め、Autopilot モードで構築し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの証拠を評価しました。
- フィルター機能の変更全体をレビューし、PR で Agent Merge を有効にしました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドを確認しました。

## 作成したもの

ワークショップには、更新済みの `main` からそれぞれ個別のブランチで作成する、2つの PR の区切りがあります。

1. **星評価:** 既存の `starRating` と、未評価であることを明示する表示をゲームカードに追加します。
2. **フィルタリングと品質のワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルタリングの計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡潔に進めるため、作業を1つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー操作を使ってコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、カスタマイズした形式で結果を報告しました。QA は PR 作成前に要件とテストカバレッジをレビューし、これらの結果をまとめました。

追加するテストは、実際に不足しているカバレッジを補うものにします。QA を実行しても追加テストが不要という結果はあり得ます。マージを許可する前にコードと証拠を確認し、変更を加えた後は影響を受ける証拠を更新します。

## ベストプラクティス

Copilot に与えるコンテキストとツールは、作業結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。これらのカスタマイズを会話をまたいで再利用し、チームのニーズに合わせて調整します。指示は標準を定め、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前に方針を検討し、**Interactive** は対象を絞った変更を確認しながら進め、**Autopilot** は範囲が明確なタスクを任せるときに使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

インフラと同じように、コンテキストも重要です。実現したいこと、その理由、方法を明確に説明すると、出力が大きく変わります。

## さらに確認する内容

中心的なワークフローを学びました。Copilot CLI には、次のような機能もあります。

- `/review` でコードレビューエージェントに変更の分析を依頼します。
- `/rubber-duck` で問題を説明し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることで上達します。本番コード、趣味のコード、長い間作りたいと思っていた小さなアプリなどに活用してください。チームで学んだことを共有し、互いに学びましょう。ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI のコマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
