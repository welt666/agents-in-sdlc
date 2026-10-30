---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、ユーザーエクスペリエンスの改善や機能追加を目的とする 3 つのタスクを Copilot クラウドエージェントに依頼しました。クラウドエージェントは非同期かつ自律的に作業しますが、タスクを監視することも重要です。

クラウドエージェントに割り当てたタスクを管理する方法はいくつかあります。GitHub.com の[エージェントページ][agents-page]では、オープンなプルリクエスト (PR) があるエージェントタスクをすべて確認できます。実行内容を調べたり、実行中のセッションに追加の指示を与えたりすることもできます。

このレッスンでは、次のことを行います。

- エージェントページでクラウドエージェントのタスクを監視します。
- 実行中のセッションに追加機能を依頼します。

## シナリオ

エージェントにハイコントラストモードの作成を依頼した後、チームはライトモードも追加するよい機会だと考えました。サイトのスタイル更新と切り替え機能をすでに実装しているため、同じ作業に含めるのが合理的です。エージェントの作業を調整し、ハイコントラストモードに加えてライトモードも実装してもらいましょう。

## Copilot クラウドエージェントのタスクを確認する

Copilot クラウドエージェントに割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインのタスクリストを確認します。次のような、Copilot に割り当てたタスクが表示されます。
    - コードのドキュメントを更新する
    - ゲームを変更する API を生成する
    - Web サイトにハイコントラストモードを追加する
3. 実行中のタスクを選択し、Copilot が実行した手順を確認します。次のような作業があります。
    - リポジトリをチェックアウトする
    - Copilot が作業する環境を作成する
    - MCP サーバーをセットアップする
    - 割り当てられたタスクを完了するための各種手順を実行する

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認し、作業の進捗を監視できます。

## クラウドエージェントの作業を調整する

実行中のタスクを確認したので、エージェントがハイコントラストモードを作成しているセッションに、ライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードの追加に関連するセッションを選択します。エージェントが使うタイトルや作業状況によって、正確なタイトルは異なります。

    ![Mission Control に表示されたアクセシビリティのセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことが表示されるまで、数分間セッションを確認します。次のようなメッセージが表示されたら作業開始の合図です。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページで、作業中の Copilot セッションに追加指示を送るダイアログを示すスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを確認し、作業計画に反映する様子を確認します。

## Copilot の作業を待つ

更新したタスクに Copilot が取り組みます。メッセージを送信した時点で実行中だった手順が終わると、次の作業に新しい依頼が取り込まれます。

前と同じように、完了まで数分かかります。その間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot クラウドエージェントのタスクを監視して案内する中心的な場所である、Copilot agents ページを確認しました。Mission Control では、実行中と完了済みのタスクを追跡し、実行内容をレビューして、進行中の作業の範囲を変更したり追加の指示を与えたりできます。

この演習では、次のことを確認しました。

- Mission Control とエージェントページを使って、クラウドエージェントのタスクを監視しました。
- 実行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを改善する][next-lesson]方法を確認しましょう。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復的に改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
