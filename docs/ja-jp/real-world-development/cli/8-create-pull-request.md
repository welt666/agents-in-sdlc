---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストが、1 つのブランチに保存されています。これらをまとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしました。今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は機能、指示、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、フィルター機能と品質ワークフロー全体を同じ会話とブランチで進め、1 つの PR にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びます。
- 機能の変更全体と検証結果を確認します。
- フィルター機能の PR を作成します。
- レビューの後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフローを通じて、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージの承認を開発者が管理しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージ可能にするための残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗を修正し、レビューコメントに対応し、必要に応じてリベースするなど、PR をブロックしている問題に取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にマージ作業を任せることもできます。Agent Merge を有効にする前に、変更をレビューして、マージ可能な状態であることを確認してください。

## Agent Merge で PR を管理する

コードの作成がすべて完了したら、変更をレビューして PR を作成し、残りの作業を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを実行して PR を作成します。
5. `/pr agentmerge` コマンドを実行して Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべてのチェックに合格すると、マージが行われます。
7. `PR #14 was squash-merged successfully.` のような Copilot のメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は、報告された理由を確認し、リポジトリで許可されている方法でレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの作成など、開発プロセスの一部を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びました。
- 機能の変更全体と検証結果を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後に Agent Merge を有効にして、PR がマージされたことを確認しました。

次は[Copilot CLI のスラッシュコマンド][next-lesson]を確認し、コンテキスト、モデル、共有、任意のクラウド委任などを扱います。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
