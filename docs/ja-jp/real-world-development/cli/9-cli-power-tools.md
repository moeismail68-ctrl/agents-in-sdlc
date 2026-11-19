---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "スラッシュコマンドでコンテキスト、モデル、会話の共有、オプションのクラウド委任を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のスラッシュコマンドは、セッション中に便利な情報や操作へのショートカットを提供します。このレッスンでは、コンテキストウィンドウ、モデル、会話の共有、およびクラウドへの委任を確認します。

## コンテキストを確認する

`/context` を使うと、会話で使用されているコンテキストウィンドウの内訳を確認できます。

```plaintext
/context
```

コンテキストが不足する場合は、不要な会話を整理するか、新しいセッションを開始します。`/compact` を使うと、会話を要約してコンテキストを節約できます。

## モデルを変更する

`/model` を使うと、利用可能なモデルを選択できます。

```plaintext
/model
```

タスクに適したモデルを選びます。複雑な計画、コード生成、レビューでモデルの挙動が変わる場合があります。

## 会話を共有する

現在の会話を共有する必要がある場合は、`/share` を使用します。

```plaintext
/share
```

表示される共有オプションを確認し、意図した相手へ共有してください。機密情報や認証情報が会話に含まれていないことを確認します。

## オプションでクラウドエージェントへ委任する

クラウドエージェントへの委任が有効な環境では、CLI からリモートセッションへ作業を委任できます。

1. `/delegate` または利用可能なクラウド委任コマンドを確認します。
2. 委任するタスクとブランチを確認します。
3. セッションを開始し、GitHub 上で進行状況を確認します。

クラウドエージェントが利用できない場合や組織ポリシーで許可されていない場合は、この手順を省略できます。

## 要約と次のステップ

Copilot CLI のスラッシュコマンドを使って、コンテキスト、モデル、共有、クラウド委任を確認しました。

次はワークショップのまとめに進みます。

## リソース

- [Copilot CLI スラッシュコマンド][slash-commands]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[slash-commands]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
