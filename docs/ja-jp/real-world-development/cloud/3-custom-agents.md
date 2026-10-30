---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせて、専用の AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルでエージェントを定義し、対象の作業を効果的に実行するための具体的な指示、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に与えます。チームの専門知識を再利用可能なエージェントとしてまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディング規約に従うセキュリティエージェント、一貫したテストパターンを適用するテストエージェントを作成できます。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義され、AI が作業を実行する方法を示します。最も簡単に区別するなら、**カスタムエージェントが作業者、スキルがツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキル（場合によってはほかのエージェント）を調整しながら作業します。このラボでは、アクセシビリティのカスタムエージェントがガイドラインに沿ってサイトをレビューし、更新します。その際、プルリクエストのワークフローやテスト実行を管理するスキルなどを呼び出すことができます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI を使うほかの方法と同様に、対象の環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタムエージェントについて、次のことを確認します。

- カスタムエージェントの定義方法
- タスクをカスタムエージェントに割り当てる方法

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにしたいと考えています。最近のユーザーフィードバックでは、現在のダークテーマはテキストと背景色のコントラストが不十分で、読みにくいと感じる人がいることがわかりました。このアクセシビリティの問題に対処するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。カスタムエージェントを使って機能を生成しましょう。

## アクセシビリティのカスタムエージェントを確認する

アクセシビリティ作業向けのカスタムエージェントは、すでに用意されています。Copilot をどのように支援するか理解するため、内容を確認しましょう。

Codespace に戻り、アクセシビリティのカスタムエージェントファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主要な責務
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題と避けるべきミス

## issue を作成して割り当てる

Mission Control は、環境内のエージェントを管理する場所です。Copilot クラウドエージェントにタスクを割り当て、進捗を監視し、追加の指示で作業を調整できます。まず、ハイコントラストモードの作成を Copilot に依頼しましょう。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue の作成画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当て画面を開きます。
9. カスタムエージェントの一覧から **Accessibility agent** を選択します。

    ![カスタムエージェントの割り当て画面で、Accessibility agent が選択されているスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントは、特定のタスクや分野に特化した AI アシスタントです。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot が作業を効果的に実行できるようにします。

この演習では、次のことを確認しました。

- カスタムエージェントの定義方法
- タスクをカスタムエージェントに割り当てる方法

Copilot がハイコントラストモードを実装している間に、[エージェントセッションの監視と調整][next-lesson]に進みましょう。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot クラウドエージェント][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
