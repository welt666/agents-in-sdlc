---
title: "演習 2 - GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot のクラウドエージェントは、コードの更新や機能の追加などを自律的に行います。作業が完了すると、人間の開発者がレビューできるドラフトのプルリクエストを作成します。面倒な作業を任せて開発を加速し、開発者はより大きな課題に集中できます。

Copilot クラウドエージェントを使って、次のことを確認します。

- コード生成の環境をカスタマイズする方法
- 操作を安全に行う方法
- issue のスコープを明確にする重要性
- issue を Copilot に割り当てる方法

## シナリオ

Tailspin Toys には解消したい技術的負債があります。サイトの初期バージョンを作った請負業者が、ドキュメントを適切に残しませんでした。実際には、ドキュメントはほとんどありません。まず、アプリケーション内のすべてのエクスポート関数に TSDoc のドキュメントコメントを追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示し、閲覧を続けられるようにします。現時点では、UX の受け入れテストに使える簡単な実装で十分です。この作業は現在ブロックされていますが、ほかに優先度の高い issue があります。

いずれも優先度が下がりやすいタスクで、Copilot クラウドエージェントへの割り当てに適しています。クラウドエージェントが非同期で作業している間、開発者は別のタスクに取り組み、後から成果をレビューできます。

## GitHub Copilot クラウドエージェントの紹介

[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる][assign-issue]方法などがあります。割り当てると、Copilot は進捗を追跡するドラフトのプルリクエストを作成し、環境をセットアップして、タスクに取りかかります。実行中または完了後に Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## スコープを明確にした指示の重要性

GitHub Copilot に魔法はありません。考える手間を省けるわけではありません。一見簡単そうな作業でも、詳細を掘り下げると複雑さが見えてきます。[Copilot クラウドエージェントに依頼するタスクのスコープ][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、調整しましょう。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習ではカスタム指示とその役割を確認しました。カスタム指示の演習を完了していれば、`.github/copilot-instructions.md` が機能する様子を見たはずです。まだの場合は、ここで内容を確認しましょう。Copilot クラウドエージェントに作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認しておくと、後でその影響に気付けます。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件に注目してください。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターが `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで `Code lacks documentation` issue をクラウドエージェントに割り当てるとき、作成されたプルリクエストに TSDoc コメント、TypeScript の規約、コメントヘッダーが反映されているか確認してください。後の演習でプルリクエストをレビューするときに、改めて説明します。

## Copilot クラウドエージェントの開発環境をセットアップする

誰がコードを書く場合でも、一般的には特定の環境と、状態を整えるためのセットアップスクリプトが必要です。Copilot にタスクを割り当てる場合も同様で、Copilot はソフトウェアエンジニアのように作業します。

クラウドエージェントは、作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係をインストールできます。演習の進行を簡単にし、この仕組みを学べるよう、あらかじめ設定されています。単一の Astro アプリに必要な Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシードを適用した SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じ構成ですが、次の点に注目してください。

- `copilot-setup-steps` という 1 つのジョブが含まれています。このジョブは、Copilot がプルリクエストで作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot に実行させる前に、ワークフローが正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は誰もが理解していますが、情報が古い、またはドキュメントがないプロジェクトも少なくありません。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しいメンバーの受け入れを難しくします。Copilot はドキュメントの作成を得意としているため、クラウドエージェントに割り当てるのに適した issue です。バックグラウンドで必要なドキュメントを作成します。後の演習で成果を確認します。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue の作成画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当て画面を開きます。

  ![issue を Copilot に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が TODO リストを作成したことを確認します。

> [!NOTE]
> Copilot の TODO リストが PR に表示されるまで、数分かかることがあります。Copilot は前述のワークフローを実行して環境を準備し、プロジェクトを分析して、問題に取り組む方法を決めています。

13. リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ウィンドウを再読み込みしてください。

16. ライブセッションをスクロールし、Copilot がどのように問題を解決しているか確認します。コードを調べて状態を理解し、計画を考えて、コードを作成する様子を確認できます。

この作業には数分かかります。Copilot クラウドエージェントの主な目的は、非同期でタスクを実行して、ほかの作業に集中できるようにすることです。この利点を活かすため、別のタスクもクラウドエージェントに割り当て、その間にアプリケーションへの機能追加に取り組みましょう。

## ゲーム詳細ページに関連ゲームを追加する

Copilot クラウドエージェントの利点の 1 つは、作業を分担できることです。デザインチーム向けに関連ゲームのセクションを追加するのに時間がかからないとしても、その時間をほかの作業に使えます。クラウドエージェントに割り当ててみましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue の作成画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次のテキストを入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当て画面を開きます。

  ![issue を Copilot に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

ほどなくして、issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![Copilot が issue の作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot クラウドエージェントはソフトウェアエンジニアと同じように作業するので、積極的に監視する必要はありません。完了後にレビューすればよいでしょう。次はカスタムエージェントの作成と使用に取り組みます。

## まとめと次のステップ

[GitHub Copilot クラウドエージェント][copilot-agents]を確認しました。クラウドエージェントに issue を割り当てると、非同期で作業させることができます。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などに活用できます。

この演習では、次のことを確認しました。

- コード生成の環境をカスタマイズする方法
- 操作を安全に行う方法
- issue のスコープを明確にする重要性
- issue を Copilot に割り当てる方法

クラウドエージェントがバックグラウンドで作業している間に、カスタムエージェントの作成と使用に取り組みましょう。[Copilot クラウドエージェントは MCP サーバー][cloud-agent-mcp]と、前のモジュールで説明したカスタム指示も利用できます。

## リソース

- [Copilot クラウドエージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
