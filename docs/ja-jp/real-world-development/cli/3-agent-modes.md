---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan モードで方針を合意し、Autopilot モードで Issue のフィルター機能を構築し、Interactive モードで結果を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

小規模な機能は追加できましたが、より大きな変更には堅牢なプロセスが必要です。GitHub Copilot CLI は組織の既存ワークフローに沿って動作し、適切な機能を適切な方法で構築できます。このレッスンでは、Issue を使って機能を生成し、コードを検証し、期待通りに動作することを確認し、最後にマージする、エージェント主導の開発プロセスを実践します。

> [!NOTE]
> 機能ワークフローを進める間は、同じ会話とブランチを使用します。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは中核となる概念に集中するためショートカットを使用します。

このレッスンでは、次の操作を行います。

- GitHub Issue から新しい Copilot 会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードを確認します。
- 転送されたブラウザーで機能を手動検証します。

この機能を進める間、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP 検証を追加し、QA エージェントを作成して機能 PR を作成します。

## シナリオ

Tailspin Toys のカタログは拡大しており、ユーザーはカテゴリと発行元でゲームを絞り込む必要があります。バックログ Issue には機能の概要がありますが、カテゴリの組み合わせ方法などはコーディング前に合意が必要です。Plan モードで判断を確定し、Autopilot モードでスコープを限定した実装を承認します。

## 会話モード

**会話モード**は、エージェントの自律度を制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: エージェントと協調して作業します。エージェントは変更案を提示し、次へ進む前に入力を待ちます。
- **Plan**: 最初に計画を作成し、プロジェクトファイルの編集は禁止されます。
- **Autopilot**: エージェントが自律的に作業し、コードを作成、テストを実行し、タスクが完了するまで反復します。

Plan モードから開始し、計画を確認した後、Autopilot モードで実装します。

## Issue から開始する

フィルター機能に着手する前に、Codespace に戻ってリポジトリとターミナルを準備します。

1. Codespace に戻り、停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を開始します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで、タイトルが **Allow users to filter games by category and publisher** の Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブへ戻ります。

プロンプトの先頭に `#7` などの番号が表示されます。`#` を使うと、GitHub の Issue またはプルリクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を作成すると、Copilot に作業を渡す前に、機能の実装方法やタスクの進め方を定義できます。複雑な作業では計画に時間をかけるとよいでしょう。Plan モードへ切り替え、計画を作成します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードへ切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 前の手順で追加した Issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

3. Copilot がフィルターの仕組みについて質問する場合があります。決定に従って回答します。
4. 計画が完了したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画をスクロールして確認します。
6. 自分の判断と一致しない部分を修正するよう依頼します。

## Autopilot を承認する

計画を作成して確認したら、実装に進みます。Autopilot を使って Copilot に実行させましょう。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みのバージョンで類似したラベルのオプションを選択します。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装を開始する場合があります。承認前に計画を確認してください。依存関係の不足やポート競合が報告された場合は、チェックを完了と見なす前にセットアップを解決します。

## 実装を確認して検証する

コードを生成したら、ほかのコードと同様にマージ前に確認します。コードを確認し、サイトを起動して動作を検証します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードへ切り替えます。モード表示に **Plan** または **Autopilot** が表示されていないことを確認します。
2. `/diff` を入力し、フィルターの実装とテストを確認します。
3. 結果を Issue と計画時の判断と照合します。
4. 確認後、<kbd>Esc</kbd> を押して diff ビューを終了します。
5. プロジェクトのチェック出力を確認し、失敗を解決するよう Copilot に依頼します。

## 新しい機能を確認する

コードは問題なくても実行できるとは限りません。前回と同様にアプリを起動し、Codespaces の転送ポート経由でサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces がポート `4321` を利用可能だと通知したら、**Open in Browser** を選択します。
3. カテゴリフィルター、発行元フィルター、および計画時に合意した組み合わせを試します。
4. リセット動作と結果が空の場合の動作が Issue と判断に一致することを確認します。
5. Codespace に戻り、起動した開発サーバーの停止を Copilot に依頼します。

## 要約と次のステップ

さまざまな会話モードを使って機能を構築し、確認しました。このレッスンでは、次の操作を行いました。

- GitHub Issue から新しい Copilot 会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードを確認しました。
- 転送されたブラウザーで機能を手動検証しました。

次は[カスタム指示を使う][next-lesson]ことで、コード生成に文書化された慣行を適用します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
