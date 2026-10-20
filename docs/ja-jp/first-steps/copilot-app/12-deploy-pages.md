---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app を使って Pages ワークフローを設定し、プルリクエストをレビューして、Space Quiz の公開サイトを確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Space Quiz を GitHub Pages にデプロイし、誰でもアクセスできるようにします。Copilot にワークフローの準備やデプロイを依頼できますが、変更をレビューし、サイトが公開されたことを確認するのは利用者の責任です。

このレッスンでは、次の作業を行います。

- GitHub Pages のデプロイを設定するよう Copilot に依頼します。
- マージする前にワークフローとプルリクエストをレビューします。
- デプロイの進行状況を確認し、公開されたサイトを検証します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、簡単に試せる場所が必要です。GitHub Pages で単一ファイルのサイトを公開すれば、サーバーやランタイム依存関係を追加せずに、公開 URL から利用できるようになります。

## Copilot にサイトをデプロイしてもらう

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Space Quiz のプルリクエストのマージ、Build website・Upload artifact・Deploy to Pages の各手順を含む GitHub Actions の Deploy website 実行成功、yourname.github.io/space-quiz/ に公開された Space Quiz の3段階を示す図。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトでは、エージェントにプルリクエストの作成とマージ、リポジトリの Pages 設定の変更を依頼します。実行する前に、差分、デプロイ権限、GitHub の承認要求を確認してください。

## ワークフローをレビューしてデプロイを検証する

1. リポジトリに README がない場合にのみ、README が追加されていることを確認します。
2. ワークフローのビルド手順とデプロイ手順を含め、プルリクエストの変更をレビューします。
3. 必須のプルリクエストチェックが成功してからマージします。
4. GitHub Actions のデプロイ実行を追跡し、成功するまで確認します。
5. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

エージェントに Pages の有効化やプルリクエストのマージを行う権限がない場合は、エージェントが提示する具体的な手順に従ってください。デプロイが成功し、公開サイトを表示できるまでは、タスクが完了したと見なさないでください。

## まとめと次のステップ

Space Quiz を公開し、エージェントの報告だけに頼らず、デプロイが成功したことを確認しました。[レッスン 13: レビューと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
