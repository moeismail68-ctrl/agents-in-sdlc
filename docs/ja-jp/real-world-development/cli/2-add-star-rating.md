---
title: "レッスン 2 - 星評価を追加する: 最初の変更"
description: "Copilot CLI を使ってゲームカードに小規模な変更を加え、転送されたブラウザーで確認して、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、次はプロジェクトへ最初の変更を加えます。変更は小さく、ゲームのデータには星評価がすでにありますが、ホームページのゲームカードにはまだ表示されていません。エージェントに評価を表示させ、変更を確認し、最初のプルリクエストとしてマージします。

このレッスンでは、次の操作を行います。

- 機能ブランチで Copilot に絞った会話を開始します。
- エージェントにプロジェクトへの小規模な変更を依頼します。
- `/diff` を使って変更を確認します。
- 転送されたブラウザーでアプリを起動し、変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価が設定でき、詳細ページにはすでに表示されています。ただし、ホームページのゲームカードにはタイトル、カテゴリ、発行元、説明しか表示されていません。ウォームアップとして、エージェントに既存の評価を各カードへ表示させます。これは独立した小規模な変更なので、最初のセッションに適しています。

## 会話のしくみ

**会話**は、Copilot CLI でタスクを処理する場所です。Copilot app とは異なり、通常の CLI 会話は専用の worktree を作成せず、ターミナルで現在チェックアウトされているリポジトリと Git ブランチを使用します。保存した会話では、ファイルとブランチを通常の Git 状態としてディスクに残したまま、同じやり取りを後から再開できます。

会話には次の 3 要素があります。プロンプトとエージェントの回答、エージェントがプロジェクトを調査してファイルを編集する際のツールアクティビティ、`/diff` で確認できる変更です。

## 会話を開始して変更を依頼する

新しい会話を開始し、機能の実装を進めましょう。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. Copilot が起動していない場合は、次のコマンドで開始します。

   ```bash
   copilot --yolo
   ```

4. `/new` スラッシュコマンドを入力し、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調査し、ゲーム詳細の表示に使用されているファイルを特定し、必要なコードを作成します。これで Copilot CLI を使って新機能を追加できました。

## 差分を確認する

たとえ小規模でも、AI が生成した変更はすべてマージ前に確認する必要があります。Copilot CLI から直接変更を確認しましょう。

1. `/diff` を入力し、変更されたファイルをすべて確認します。
2. 星評価が設定されている場合は数値が表示され、`starRating` が `null` の場合は `No rating yet` が表示されることを確認します。
3. テストが両方の状態をカバーしていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. 確認が完了したら、<kbd>Esc</kbd> を押して diff画面を終了します。

> [!NOTE]
> Copilot を含む生成 AI は確率的に動作するため、実装が完全に決定的になるとは限りません。特定の実装を期待するのではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると思い込むべきではありません。Copilot にウェブサイトを起動させ、Codespaces が転送するブラウザーで更新された UI を確認しましょう。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces がポート `4321` を利用可能だと通知したら、**Open in Browser** を選択します。
3. ゲームカードに評価が 5 点満点で表示されることを確認します。
4. Copilot に戻り、起動したサーバーの停止を依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。新しいコードをプロジェクトへマージするため、プルリクエスト (PR) を作成します。

1. デフォルトエージェントに変更のコミットを依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR 作成時に既存のコミットを push し、PR の URL を表示します。
3. Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら、Copilot CLI に表示された URL を選択して PR を開きます。
4. 変更されたファイルとチェックを確認します。
5. 問題がなければ **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## 要約と次のステップ

GitHub Copilot CLI を使って最初の変更を実装できました。具体的には次のことを行いました。

- 機能ブランチで対象を絞った Copilot 会話を開始しました。
- エージェントにゲームカードへの小規模な変更を指示しました。
- `/diff` を使って変更を確認しました。
- 転送されたブラウザーでアプリを起動し、星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は[フィルター Issue を起点として Plan モードと Autopilot モードを使用][next-lesson]し、より大規模な機能を実装します。

## リソース

- [GitHub Copilot CLIについて][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
