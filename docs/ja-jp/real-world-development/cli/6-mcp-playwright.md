---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI の Playwright MCP を設定または確認し、ブラウザーでフィルター機能を検査します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くことだけが開発ではありません。データ、外部サービス、Copilot が利用できる自動化も扱う必要があります。ここで MCP サーバーが役立ちます。Model Context Protocol (MCP) サーバーを使うと、Copilot は CLI に組み込まれている機能を超え、追加のツールやサービスへアクセスできます。

このレッスンでは、次の操作を行います。

- MCP の概要と Copilot CLI における利用方法を理解します。
- 未設定の場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

単体テストやエンドツーエンドテストは重要ですが、UI の更新を検証するには実際に UI を操作する必要があります。Copilot に作業中のウェブサイトをユーザーとして操作させることで、変更の検証を自動化し、更新が期待通りに動作するかの確信を高められます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部ツールやサービスと通信するための仕組みです。MCP を使うと、エージェントはリアルタイムで外部ツールやサービスへアクセスし、依頼された操作を実行できます。

ツールやリソースには MCP サーバー経由でアクセスします。MCP サーバーは AI エージェントと外部ツールの橋渡しとして機能し、サーバーごとに利用可能なツールとリソースが異なります。

代表的な MCP サーバーには次があります。

- **[GitHub MCP サーバー][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API へのアクセスを提供します。
- **[Playwright MCP サーバー][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub はエコシステムの発見と貢献を促進するため、[MCP レジストリ][mcp-registry]を提供しています。

> [!CAUTION]
> MCP サーバーはプロジェクトの他の依存関係と同じように扱ってください。利用前にソースコードを確認し、公開者を検証し、セキュリティ上の影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーが利用可能でない場合は追加し、Copilot がユーザーと同じようにウェブサイトへアクセスできるようにします。

> [!NOTE]
> Copilot CLI に Playwright MCP がすでに設定されている場合は、このセットアップを省略して次へ進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバータイプが **STDIO** のままであることを確認し、<kbd>Enter</kbd> を押し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログへ次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押してサーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを終了します。

## Copilot に Playwright で機能を確認させる

これまで機能を手動で確認しました。ここでは、Copilot が利用できる Playwright MCP サーバーを使って検証しましょう。

1. 次のプロンプトを使い、Playwright MCP で機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> MCP サーバーの使用を明示的に指示しなくても、Copilot が通常は自動的に判断します。ただし、使用するツールを事前に把握している場合は、方向を示すこともできます。結果の一貫性が高まり、トークンの節約にもなります。

2. Copilot がブラウザーで実行する手順と機能確認の状況を確認します。
3. レポートを読み、すべてが期待通りに動作することを確認します。

Copilot はサーバーを起動し、Playwright でウェブサイトを操作し、サーバーを停止し、レポートを提示します。

## 要約と次のステップ

Copilot CLI から Playwright MCP サーバーを使って、実際のブラウザーで機能を確認できました。ここでは次の操作を行いました。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を学びました。
- 未設定の場合は Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認させました。

次は、スキルとブラウザーツールを専門ロールに統合する[QA カスタムエージェントを作成][next-lesson]します。

## リソース

- [MCP の概要と注目される理由][mcp-blog-post]
- [Microsoft Playwright MCP サーバー][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
