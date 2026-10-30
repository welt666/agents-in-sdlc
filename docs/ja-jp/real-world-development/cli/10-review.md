---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用できるカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Copilot CLI を使って、Tailspin Toys の一連のワークフローに取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、フィルター機能の issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントの作業を案内し、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- QA カスタムエージェントを作成して選択し、要件、テストカバレッジ、スキルの結果、ブラウザーでの検証結果を評価しました。
- フィルター機能の変更全体をレビューし、Agent Merge に PR のマージを許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドを確認しました。

## 作成したもの

このワークショップでは、更新済みの `main` から作成したそれぞれ別のブランチで、2 つの PR を作成しました。

1. **星評価:** 既存の `starRating` と未評価の状態をゲームカードに表示します。
2. **フィルター機能と品質ワークフロー:** フィルターを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルター機能の計画から PR の作成まで、同じ会話とブランチを使いました。作業を簡潔にするため、すべての変更を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は PR の前に、要件とテストカバレッジのレビュー結果をこれらの結果と組み合わせました。

追加するテストは、実際に不足しているカバレッジを補うものにします。QA の実行で新しいテストが不要と判断されることもあります。マージを許可する前に、コードと検証結果をレビューしてください。変更を加えた場合は、影響を受ける検証結果を更新します。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の質に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。会話をまたいでこれらのカスタマイズを再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行できるタスクを記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** で実装前に方針を検討し、範囲の狭い変更では **Interactive** で進行中の作業を確認し、範囲が明確なタスクには **Autopilot** を使います。日常的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を作るのか、その理由、方法を明確に伝えることで、出力が大きく変わります。

## さらに試す

主要なワークフローを確認できました。Copilot CLI のほかの機能も試してみましょう。

- `/review` を使い、コードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` を使い、問題について対話して別の視点を得ます。
- `/fleet` を使い、独立したサブタスクを並行して調整します。
- `/worktree` を使い、別のタスクを分離します。
- `/delegate` を使い、Copilot クラウドエージェントにタスクを送信します。

## 次のステップ

どのツールでも、上達する最善の方法は使い続けることです。実際のコード、趣味のプロジェクト、何年も作りたいと思っていた小さなアプリなどで試してみてください。学んだことをチームと共有し、チームのメンバーからも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの作業環境を確認するには、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリのワークショップ][app]、または [Copilot クラウドエージェントのワークショップ][cloud]を参照してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
