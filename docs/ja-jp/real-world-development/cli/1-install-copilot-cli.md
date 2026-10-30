---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、作業環境を確認して、用意されたフィルター機能の issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディングアシスタントです。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールと連携できます。すべてコマンドラインから行えるため、作業を任せたり、変更を依頼したりしながら集中を保てます。まずはツールをインストールします。すでに使い慣れたツールを使って簡単にインストールできます。

このレッスンでは、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーを使って、フィルター機能の issue を見つけます。

## シナリオ

チームは、増え続けるバックログに対応するために AI エージェントを使い始めています。Copilot CLI は多くの開発者が日常的に作業するターミナルに、その機能を追加します。このレッスンでインストールと認証を済ませ、ワークショップの残りで利用できるようにします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew を使ってインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、このレッスンでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、デバイスコードの手順に従って認証し、Copilot CLI を許可します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. パスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されていることがあります。Copilot CLI が認証を求めずに起動した場合は、そのまま続行できます。

## 作業環境を確認する

通常のシェルプロンプトで実行するコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語をエージェントに送り、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選び、<kbd>Enter</kbd> を押して、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストール済みのバージョンで使用できるコマンドを確認し、<kbd>Esc</kbd> でヘルプ画面を閉じます。
3. 次の質問を Copilot に送り、すべてが動作することを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、Copilot が答える前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが使用可能であることを確認します。
6. Copilot に次のプロンプトを送り、フィルター機能の issue を探します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開き、issue を読みます。最初の小さな変更を完了した後に使用します。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルでチェックアウトされているブランチを使用し、自動的に worktree を作成しません。変更ごとに機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は既存の権限の範囲外でツールを使用する前に確認を求めます。このワークショップでは `--yolo` を付けて再起動します。ユーザーが承認したこのショートカットを使うと、Codespace 内で承認プロンプトが表示されなくなり、演習に集中できます。

> [!CAUTION]
> `--yolo` は完全な自動権限（`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`）を有効にします。Codespace や VM のような分離された環境でのみ使用し、日常的な開発の既定値としてエイリアスにしないでください。詳しくは[ツールの使用を許可または禁止する][allow-all-warning]を参照してください。

このワークショップでは、`--enable-all-github-mcp-tools` を使って、後のレッスンで issue やプルリクエストを扱うための GitHub MCP の読み取り/書き込みツールを有効にします。Codespace によってローカルコンピューターへのアクセスは制限されますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をして会話が動作することを確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりした後、`copilot --resume` を使うと、同じ機能の会話とブランチに戻れます。

## まとめと次のステップ

GitHub Copilot CLI をインストールして認証できました。このレッスンでは、次のことを行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーを使って、フィルター機能の issue を見つけました。

次は[最初の対象を絞った変更][next-lesson]に取り組み、Copilot CLI を使ってゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
