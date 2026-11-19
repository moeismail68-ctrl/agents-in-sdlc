---
title: "演習 2 - Cloud Agent"
description: "GitHub Issue を Copilot cloud agent に割り当て、非同期で実装させます。"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent は、GitHub 上で Issue をバックグラウンドに割り当てると、リポジトリを調査して変更を実装し、プルリクエストを作成します。

## Issue を割り当てる

1. 実装する GitHub Issue を開きます。
2. Copilot を担当者またはエージェントとして割り当てます。
3. Issue の要件を確認します。
4. エージェントが作業を開始するまで待ちます。

## セッションを確認する

agents ダッシュボードでセッションの進行状況を確認し、エージェントが変更したファイルを確認します。必要に応じて、追加の指示を送って作業を誘導します。

> [!NOTE]
> エージェントの作業は非同期で実行されます。セッションを監視し、変更を PR として確認してください。
