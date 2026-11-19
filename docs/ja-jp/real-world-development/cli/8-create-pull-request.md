---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルターの変更をレビューし、プルリクエストを作成し、Copilot Agent Merge を使ってマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示、スキル更新、ブラウザーテストが完了したら、変更全体をレビューして PR を作成します。

このレッスンでは、次の操作を行います。

- Issue、計画、実装、テストの対応を確認します。
- 完成したフィルター変更の PR を作成します。
- 必要なレビューとチェックが完了したことを確認します。
- Agent Merge を使って PR をマージします。

## 最終レビュー

1. Copilot CLI または GitHub の PR ビューで変更を確認します。
2. Issue に記載されたフィルター要件と計画時の判断がすべて実装されていることを確認します。
3. 変更されたファイル、テスト、ドキュメントを確認します。
4. QA エージェントの指摘を解決し、必要な再検証を実行します。
5. CI チェックが成功するまで待ちます。

> [!NOTE]
> PR をマージする前に、チェックとレビューを確認してください。失敗しているチェックや未解決のレビューコメントがある場合は、マージしないでください。

## PR を作成する

1. Copilot CLI に次のように依頼します。

   ```plaintext
   Create a pull request for the filtering changes. Summarize the issue, implementation, tests, and verification evidence.
   ```

2. 表示された PR タイトルと本文を確認します。
3. 必要に応じて、作成された PR URL をブラウザーで開きます。

## PR をマージする

1. GitHub の PR ページでチェックとレビューの状態を確認します。
2. 問題がないことを確認したら、Agent Merge を使用します。
3. マージ後に `main` ブランチへ切り替え、最新の状態を pull します。

   ```bash
   git checkout main
   git pull --ff-only
   ```

## 要約と次のステップ

フィルター機能の変更をレビューし、PR を作成し、チェックとレビューを確認してマージしました。

次は、Copilot CLI の便利なスラッシュコマンドを使って、コンテキスト、モデル、会話の共有、およびオプションのクラウド委任を確認します。

## リソース

- [PR を作成する][create-pr]
- [Copilot を使って PR をレビューする][review-pr]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[create-pr]: https://docs.github.com/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request
[review-pr]: https://docs.github.com/copilot/using-github-copilot/code-review/using-copilot-code-review
