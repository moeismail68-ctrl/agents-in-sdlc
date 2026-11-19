---
title: "演習 1 - カスタム指示"
description: "VS Code の Copilot が従うリポジトリ指示とパススコープの指示を追加します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

カスタム指示を使うと、Copilot にコーディング規約、ドキュメント要件、テスト方針を伝えられます。

## 指示ファイルを確認する

1. `.github/copilot-instructions.md` を開きます。
2. `.github/instructions` フォルダー内の指示ファイルを確認します。
3. 各指示の `applyTo` フィールドが対象ファイルをどのように決めるか確認します。

## 指示を更新する

リポジトリの開発方針に合わせて指示を追加します。たとえば、コメント、TSDoc、フォーマット、テスト、アクセシビリティを明記できます。

指示を保存した後、Copilot Chat の新しいセッションを開始し、指示が適用されることを確認します。
