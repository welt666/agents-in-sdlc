---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用しましょう。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業管理を反復的に行います。

## シナリオ

あなたは、開発者をテーマにしたボードゲームのクラウドファンディングを提供する架空の会社、Tailspin Toys の新しい開発者です。チームのバックログは GitHub issue として登録済みで、すぐに着手できます。アクセシビリティやコーディング規約の改善に加え、フィルターやページネーションなどの機能開発も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、さまざまな作業環境で利用できます。自分に合った環境を選んで開発を始めましょう。各ワークショップは個別のセットアップから始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを扱います。

### [GitHub Copilot アプリ][app]

デスクトップアプリで複数のエージェントセッションを並行して実行し、セッションモードを切り替え、キャンバスで共同作業を行い、GitHub issue とプルリクエストを管理します。

### [GitHub Copilot クラウドエージェント][cloud]

GitHub issue を非同期エージェントに割り当て、その作業を案内して進捗を監視し、エージェントが作成したプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを扱います。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
