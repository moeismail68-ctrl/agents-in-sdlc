---
title: "演習 1 - カスタム指示"
description: "Copilot cloud agent が従うリポジトリ指示を追加します。"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

カスタム指示を使うと、cloud agent にリポジトリ固有の規約を伝えられます。

## 指示を確認する

リポジトリの `.github/copilot-instructions.md` を確認し、プロジェクトの概要、コーディング規約、テスト方法を把握します。

## 指示を追加する

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトが守るべき開発方針を追加します。
3. コメント、テスト、エラー処理、ドキュメントに関する規約を明記します。
4. 変更をコミットします。

指示は Cloud Agent がリポジトリで作業する際のコンテキストとして使用されます。
