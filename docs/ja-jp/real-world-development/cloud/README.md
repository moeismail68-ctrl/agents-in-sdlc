---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub 上で作業を割り当てると、cloud agent がバックグラウンドでリポジトリを調査、変更を加え、プルリクエストを作成します。作業中もほかの作業を進められます。

このワークショップでは、cloud agent が従うカスタム指示を追加し、GitHub Issue を割り当てて実装させます。カスタムエージェントを使って方法を調整し、agents ダッシュボードでセッションを監視して誘導し、プルリクエストを確認して結果を反復改善します。

## 演習

| 演習 | トピック | 説明 |
|------|---------|------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. カスタム指示][ex1] | コンテキスト | cloud agent が従う指示を追加します |
| [2. Cloud Agent][ex2] | 非同期エージェント | Copilot cloud agent に Issue を割り当てます |
| [3. カスタムエージェント][ex3] | 専門エージェント | カスタムエージェントを確認して使用します |
| [4. エージェントの管理][ex4] | 監視 | エージェントセッションを監視して誘導します |
| [5.反復改善][ex5] | 確認 | PR を確認し、Copilot の作業を反復改善し、次のステップを選択します |

## 前提条件

参加前に、次を確認してください。

- [ ] GitHub アカウントと、有効な **Copilot Student、Pro、Pro+、Business、または Enterprise** プラン
- [ ] アカウントまたは組織で Copilot cloud agent が有効になっていること

> [!TIP]
> 有料プランを利用していない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] から GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI 機能が含まれます。

> [!NOTE]
> Cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free を除くすべてのプランで利用可能です。Business または Enterprise の場合は、管理者が組織向けに有効化する必要があります。

> [!NOTE]
> このワークショップでは MCP を扱いません。Copilot で MCP サーバーを使用するには、[CLI ワークショップ](../cli/)または [VS Code ワークショップ](../vscode/)を参照してください。

## はじめる

**[演習 0: 前提条件から開始する →][ex0]**

[callout-student-plan-education]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
