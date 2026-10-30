---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "Copilot CLI を使ってゲームカードに小さな変更を加え、転送されたブラウザーで確認し、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次はプロジェクトに初めて変更を加えます。小さな変更から始めましょう。各ゲームのデータには星評価がすでにありますが、ホームページのゲームカードには表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで、目的を絞った Copilot の会話を始めます。
- エージェントにプロジェクトの小さな変更を依頼します。
- `/diff` を使って変更をレビューします。
- 転送されたブラウザーでアプリを実行し、変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホームページのゲームカードに表示されるのはタイトル、カテゴリ、出版社、説明だけです。練習として、エージェントに各カードで既存の評価を表示させます。初めてのセッションに適した、小さく独立した変更です。

## 会話の構成

**会話**は、Copilot CLI を使ってタスクに取り組む場所です。Copilot アプリとは異なり、通常の CLI の会話では、ターミナルで現在チェックアウトされているリポジトリと Git ブランチを使用します。保存した会話は後で再開できますが、ファイルとブランチは通常の Git 状態としてディスク上にあります。

会話では、プロンプトとエージェントの応答、ファイルを調べて編集するエージェントのツール操作、`/diff` で確認できる変更の 3 つを扱います。

## 会話を開始して変更を依頼する

新しい会話を開始し、機能の実装に取りかかります。

1. Codespace に戻ります。
2. まだターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. まだ Copilot を起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュコマンド `/new` を実行して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトを使って変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot がプロジェクトを調べ、ゲーム詳細の表示に使われているファイルを見つけ、必要なコードを作成します。Copilot CLI を使って新機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はすべてマージ前にレビューします。Copilot CLI 内で変更を確認しましょう。

1. `/diff` を入力して、変更されたすべてのファイルを確認します。
2. ゲームカードに評価がある場合は数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. どちらの状態もテストされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら、<kbd>Esc</kbd> を押して差分画面を終了します。

> [!NOTE]
> すべての生成 AI ツールと同様に、Copilot の出力には確率的なばらつきがあるため、実装の詳細は異なることがあります。特定の実装を期待するのではなく、動作を確認してください。

## 変更を確認する

コードを読んだだけで動作すると判断せず、Copilot に Web サイトを起動してもらい、Codespaces で転送されたブラウザーから更新された UI を確認します。

1. 次のプロンプトで Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces からポート `4321` が利用可能であると通知されたら、**Open in Browser** を選択します。
3. ゲームカードに 5 段階の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次は、プロジェクトにコードをマージするためのプルリクエスト (PR) を作成します。

1. 既定のエージェントに変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、Copilot CLI に表示された URL を選択して PR を開きます。
4. 変更されたファイルとチェック結果を確認します。
5. 問題がなければ **Merge pull request** を選択してマージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って初めての変更をリリースできました。このレッスンでは、次のことを行いました。

- 機能ブランチで目的を絞った Copilot の会話を始めました。
- ゲームカードに小さな変更を加えるようエージェントに依頼しました。
- `/diff` を使って変更をレビューしました。
- 転送されたブラウザーでアプリを実行し、星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は[フィルター機能の issue から始め、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を作成します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
