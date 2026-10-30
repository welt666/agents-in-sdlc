---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot をクラウド上で非同期に動作させることができます。GitHub 上で作業を割り当てると、クラウドエージェントがバックグラウンドでリポジトリを調べ、変更を加え、プルリクエストを作成します。その間、別の作業を続けられます。

この一連の演習では、まずクラウドエージェントが参照するカスタム指示を追加し、GitHub issue を割り当てて実装を依頼します。カスタムエージェントを使って方針を定め、エージェントのダッシュボードからセッションの進捗を監視して作業を調整し、最後にプルリクエストをレビューして結果を反復的に改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. カスタム指示][ex1] | コンテキスト | クラウドエージェントが従うカスタム指示を追加します |
| [2. クラウドエージェント][ex2] | 非同期エージェント | GitHub issue を Copilot クラウドエージェントに割り当てます |
| [3. カスタムエージェント][ex3] | 専門エージェント | カスタムエージェントを確認して使用します |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視して調整します |
| [5. 反復][ex5] | レビュー | プルリクエストをレビューし、Copilot の作業を改善して、次の手順を選びます |

## 前提条件

このワークショップを始める前に、次の条件を満たしていることを確認してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを利用できる GitHub アカウント
- [ ] アカウントまたは組織で Copilot クラウドエージェントが有効になっていること

> [!TIP]
> 有料プランを利用していない場合でも、学生認証を受けた学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、どの作業環境でもすべての演習を完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> クラウドエージェントは **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free を除くすべてのプランが対象です。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> この作業環境では MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI 作業環境](../cli/)または [VS Code 作業環境](../vscode/)を参照してください。

## はじめに

**[演習 0: 前提条件から開始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
