---
title: "レッスン 4 - カスタム指示で Copilot を誘導する"
description: "リポジトリの指示を確認し、ドキュメント規約を追加してフィルターコードへ適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う場合、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合は、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]は、どんなコードが必要かだけでなく、どのように構造化するかも定義します。フィルターを構築したので、Copilot が使用した指示を確認し、ドキュメント規約を追加してコードへ適用します。

このレッスンでは、次の操作を行います。

- リポジトリの指示ファイルとパススコープの指示ファイルがエージェントへ届く仕組みを確認します。
- コーディング規約を守るために指示ファイルを更新します。
- 指示ファイルがコードへ与える影響を確認します。

## シナリオ

優れた開発チームには、開発方法に関するガイドラインや要件があります。Tailspin Toys では次の方針を使用します。

- コメントはコードを言い換えるだけでなく、意図や判断に至った理由を説明します。
- `db/` と `src/lib/` の公開関数は、TSDoc/JSDoc で目的、引数、戻り値を文書化し、該当する場合は注入可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントは `Props` の契約を文書化し、関連コードの変更時にはコメントも最新に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使うことで、Copilot がチームの慣行に沿った作業を行えるようになります。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝えられます。リポジトリ全体または特定のファイル種別、タスク向けのコンテキストを指定できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md`: リポジトリのすべてのリクエストへ送られる単一の指示ファイルです。ほとんどのリクエストに関連するプロジェクトレベルの情報を記述します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイル種別、タスク向けのガイドラインです。

> [!NOTE]
> 指示の形式やサポート状況は環境によって異なります。特定の形式を使用する前に、[カスタム指示のサポートリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには指示ファイルが含まれています。変更の影響を見る前に、現在の内容を確認しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示はリポジトリ内の Copilot 操作すべてに適用されます。
4. `.github/instructions` フォルダーを開いて各ファイルを確認します。Astro、Drizzle データ層、テストなどの指示があることを確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。指示を適用する glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存プロジェクトファイルへの参照を確認します。これにより、大規模な指示セットを再利用可能な小さなファイルへ分割し、Copilot が参照する例を指定できます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは良い出発点ですが、追加すべき規約があります。コアの `copilot-instructions.md` を更新し、新しく生成する TypeScript に TSDoc コメントを追加させます。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクションを探します。通常は 35 行付近です。
2. 次の項目を最後の箇条書きとして追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使用する

Copilot CLI は会話開始時にリポジトリの指示を読み込みます。指示を保存したら、フィルター会話を終了して再開し、機能のコンテキストを失わずに新しいガイダンスを適用させます。

1. フィルター会話が開いている場合は `/exit` を入力して終了します。Copilot CLI は会話を自動保存します。
2. リポジトリのルートから `game-filters-cli` ブランチの保存済み会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたらフィルター会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して変更された TypeScript ファイルを確認します。生成された TSDoc コメントがコードを正確に説明していることを確認します。

## 要約と次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する仕組みを確認し、新しい規約を機能へ適用しました。具体的には次の操作を行いました。

- リポジトリの指示ファイルとパススコープの指示ファイルがエージェントへ届く仕組みを確認しました。
- コーディング規約を守るために指示ファイルを更新しました。
- 指示ファイルがコードへ与える影響を確認しました。

次は、lint とテストを一貫して実行できるよう、再利用可能な quality-checks スキルを[カスタマイズして実行][next-lesson]します。

## リソース

- [Copilot CLI 用のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示作成のベストプラクティス][instructions-best-practices]
- [Awesome Copilot — instruction ファイルのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
