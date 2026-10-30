---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は通常、必要なファイルを探して変更し、テストを実行して、デバッグし、更新し忘れたファイルを見つけて、さらに変更を加えます。作業は次々と続きます。

ここで役立つのが Copilot エージェントモードです。

Copilot エージェントモードは、IDE 内でより自律的に作業するように設計されています。開発者と同じように、まず既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェントモードを使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードとクラウドエージェントは異なる用途向けに作られています。エージェントモードは IDE 内で動作するため、すばやいフィードバックと対話が可能です。クラウドエージェントはチームメンバーのように非同期で作業し、issue とプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot エージェントモードがプロジェクトを調べ、関連ファイルを特定し、複数のファイルをまとめて変更する方法
- GitHub Copilot エージェントモードが UI とデータレイヤーにまたがる新機能を実装する方法
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法

## シナリオ

ゲームの一覧が増えてきたため、カテゴリや出版社で絞り込めるようにしたいと考えています。前の演習で出版社ヘルパーを追加しました。次は Copilot エージェントモードを使って、残りのデータレイヤー、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。単一の Astro アプリで、ビルド時にページを静的 HTML として描画します。ページはフロントマターから Drizzle ORM を使ってローカルの SQLite データベースを直接照会します。別のバックエンド API やクライアントサイド UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるように、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルター機能用の新しいブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くかどうか尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Codespace の Web サーバーに接続するプライベートトンネルです。

### Web サイトを確認する

サイトを起動したら、機能を確認できます。主な機能は次のとおりです。

- **ホームページ**: タイトル、画像、説明とともにボードゲームを一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、出版社、カテゴリなどの情報を含む詳細ページが表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善したいと考えています。まずはバックログを確認しましょう。テンプレートからプロジェクトを作成したときに、issue のバックログも自動的に用意されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。**Agent** は複数のファイルにわたって自律的に変更を計画、実装し、ターミナルコマンドやツールを実行できます。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。このワークショップの作成時に Claude Sonnet 4.5 を使ったため、比較的一貫した体験になるよう手順で指定しています。ただし、大規模言語モデル (LLM) の出力にはばらつきがあるため、手順と異なる提案が表示されることがあります。これは正常な動作です。

> [!NOTE]
> LLM の性質上、Copilot が異なる MCP コマンドを使うことがありますが、タスク自体は完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーにとって有益だと考える項目を基準に、優先順位も付けていることを確認してください。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilot にコンテキストを与える指示ファイルを確認します。Web サイトの機能追加に取り組む方法を説明する [ユーザーインターフェイス (UI)][ui-instructions]の指示ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティに関する原則
   - Astro、Tailwind CSS、Drizzle など、ファイル種類ごとの指示へのリンク

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、大きな指示を管理しやすい小さなファイルに分け、例やテンプレートを参照できます。

## フィルター機能を実装する

フィルター機能の実装には、少なくとも次の 3 つの更新が必要です。

- データレイヤー (`src/lib/`) のフィルターロジックを追加または調整する
- フィルターの動作を確認するテストを追加または更新する
- ゲーム一覧ページにフィルター UI を追加する

さらに、コードベースにマージする前にテストを実行し、合格させる必要があります。Copilot エージェントモードにこれらのタスクを依頼しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 引き続き **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログ内の関連 issue をもとに機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要なファイルを特定する様子を確認します。データレイヤーのヘルパー、UI、テストが見つかり、ファイルの変更とテストの実行が始まります。

   ![Copilot がプロジェクトのファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによって、数分かかることがあります。作業中、エディターに **Keep** と **Undo** ボタンが表示されることがあります。すべての作業が完了すると変更ごとに **Keep** または **Undo** が表示されるので、処理中に選択する必要はありません。

7. Copilot から求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインにテストを実行してよいか確認するダイアログを示すスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクが完了し、エラーがないと判断するまで、コード生成とテストを繰り返します。

   ![Copilot エージェントモードの完了した Chat セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを実行したブラウザーに戻り、新しい機能を試します。
11. 動作を確認し、コードのレビューが完了したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェントモードを使って、Tailspin Toys の Web サイトに機能を追加する方法を確認しました。この演習では、次のことを学びました。

- GitHub Copilot エージェントモードが UI とデータレイヤーにまたがる新機能を実装する方法
- Copilot エージェントモードがプロジェクトを調べ、関連ファイルを特定し、複数ファイルをまとめて変更する方法
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法

次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### おまけの演習 — ページ分割を実装する

ゲームの一覧が増えると、ページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割の実装を依頼してみましょう。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定にするかを検討します。
- 希望どおりの実装を得られるよう、必要なガイダンスを Copilot に提供します。
- Copilot に変更やコンテキストを伝え、複数回やり取りする必要がある場合があります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
