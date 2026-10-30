---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で issue のフィルター機能を実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

プロジェクトに小さな機能を追加しました。より大きな変更には、さらにしっかりとしたプロセスが必要です。GitHub Copilot CLI は組織の既存のワークフローに沿って作業できます。いくつかのレッスンを通じて、issue から機能を作成し、コードが正しいことを確認し、機能の動作を検証して、プロジェクトに正常にマージするまでの一般的なエージェント駆動型開発の流れをたどります。

> [!NOTE]
> 機能の作業を続ける間、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中できるように手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot の会話を始めます。
- Plan モードで要件を決めます。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

機能の開発を続ける間に、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えてきたため、訪問者がカテゴリや出版社でゲームを絞り込めるようにします。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細もあります。Plan モードで判断を確定し、Autopilot には範囲を限定した実装を許可します。

## 背景

AI コーディングエージェントを開発ワークフローに導入しても、基本は変わりません。むしろ、基本がより重要になります。多くの開発者は、次のような手順で作業します。

1. 必要な作業を記載した issue から始めます。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行して検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 手順の詳細は、チームや組織によって異なります。ただし、ほとんどのワークフローはこの手順の一部を変えたものです。

標準的な手順を守ることで、AI が生成したコードが要件を満たし、手作業で作成したコードと同じレビューを受けるようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、続行する前にユーザーの入力を待ちます。
- **Plan**: エージェントが計画を作成します。プロジェクトファイルの編集は禁止されます。
- **Autopilot**: エージェントがコードを書き、テストを実行し、タスクが完了するまで反復的に作業します。

まず Plan モードで計画を確認し、その後 Autopilot モードで実装します。

## issue から始める

フィルター機能の作業に取りかかる前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押して、すべての issue を表示します。
8. 矢印キーを使って、**Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7`（または同様の番号）で始まることを確認してください。`#` を使うと、GitHub 上の issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

Plan モードを使うと、機能の実装方法やタスクの進め方を Copilot に任せる前に決められます。複雑なタスクは、まず計画を立てるのがお勧めです。Plan モードに切り替えて Copilot に計画を作成させましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を判断して計画を作成します。

3. フィルター機能の動作について質問された場合は、希望に応じて回答します。唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールして確認します。
6. 判断と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画が完成してレビューできたら、Copilot に実装を任せます。Autopilot モードでは、Copilot は問題が解決したと判断するまで作業を繰り返します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンで同様の名前が付いた選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 計画を承認すると、すぐに実装が始まることがあります。必ず計画を先に確認してください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同様にマージ前にレビューします。コードをレビューし、サイトを実行して問題がないことを確認しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** または **Autopilot** が表示されていないことを確認します。
2. `/diff` を入力して、フィルターの実装とテストを確認します。
3. 結果を issue と計画時の判断に照らして確認します。
4. コードのレビューが終わったら、<kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば修正を依頼します。

## 新しい機能を試す

コードに問題がないことを確認したら、アプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. 次のプロンプトで Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces からポート `4321` が利用可能であると通知されたら、**Open in Browser** を選択します。
3. 計画時に合意したカテゴリ、出版社、その組み合わせによるフィルターを試します。
4. リセットと結果がない場合の動作が、issue と合意内容に沿っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

会話モードを使い分けて、機能を作成しレビューできました。このレッスンでは、次のことを行いました。

- GitHub issue から新しい Copilot の会話を始めました。
- Plan モードで要件を決めました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は[カスタム指示を使って][next-lesson]、コードが記載された規約に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
