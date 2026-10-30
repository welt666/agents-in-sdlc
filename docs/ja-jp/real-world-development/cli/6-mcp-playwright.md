---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに説明したように、コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が使える自動化機能を追加する必要もあります。そこで役立つのが MCP サーバーです。MCP サーバーを使うと、Copilot CLI に組み込まれている機能を拡張し、ツールやサービスを追加できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びます。
- Playwright MCP サーバーがまだ利用できない場合は追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post]を使うと、AI エージェントが外部のツールやサービスと通信できます。MCP によって、エージェントは外部サービスにリアルタイムで接続し、最新情報にアクセスしたり、ユーザーに代わって操作したりできます。

ツールやリソースは、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じて利用します。各 MCP サーバーは、エージェントが使えるツールとリソースを提供します。

よく使われる MCP サーバーの例を次に示します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにもさまざまな MCP サーバーがあります。GitHub は、エコシステム内での発見や貢献を促進するために [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーもほかの依存関係と同様に扱ってください。利用する前にソースコードを確認し、発行元を検証し、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるように追加します。

> [!NOTE]
> Copilot CLI に Playwright MCP がすでに設定されている場合は、セットアップ手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加画面を開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力して <kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd> を押してから <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP の画面を終了します。

## Playwright を使って機能を調べるよう Copilot に依頼する

これまでは機能が期待どおりに動作することを手動で確認しました。次は、Copilot で使える Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自動的に MCP サーバーを選択するため、明示的に指示する必要はありません。ただし、使用するツールがわかっている場合は、プロンプトで示すとよいでしょう。動作の一貫性を高め、トークンを節約できます。

2. Copilot がブラウザーで機能を確認する手順を実行する様子を確認します。
3. レポートを読み、すべてが期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright でサイトを操作し、作業後にサーバーを停止して結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認できました。このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びました。
- Playwright MCP サーバーを利用できない場合に追加しました。
- エージェントにブラウザーを操作させ、機能を確認しました。

次はスキルとブラウザーツールを専門的な役割として組み合わせる、[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
