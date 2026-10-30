---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスの改善や機能追加に関する複数のタスクを GitHub Copilot と進めてきました。コードにドキュメントを追加し、デザインチームが検討する関連ゲーム機能を実装し、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能を追加するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を導入してもソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体にレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認しましょう。

## セキュリティと GitHub Copilot クラウドエージェント

Copilot クラウドエージェントは非同期かつ無人でタスクを実行するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスはコード作成に使うブランチに限られます。
- クラウドエージェントは GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- MCP サーバーなど、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

GitHub Copilot クラウドエージェントが作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認しましょう。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認する際に、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は MCP サーバーへのアクセスなど、外部リソースへのアクセスが既定で制限されているため、これは想定どおりの動作です。必要であれば、[Copilot クラウドエージェントのファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が付いています。作業が完了するまで待ってください。数分かかることがあります。その間に休憩したり、これまでに学んだことを振り返ったりしましょう。

4. プルリクエストの準備ができたら、**Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメントコメントやその他のドキュメントが追加されたコードを確認します。具体的な変更内容は状況によって異なります。

    変更を確認するときは、TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーに注目してください。これらは演習 2 の冒頭で確認したカスタム指示ファイルから反映されています。

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェックセクションでワークフローがキューに登録されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクトチェックが合格します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot とのプルリクエストでのやり取りは一方通行ではありません。チームメンバーと同じように、プルリクエストやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、変更に対応する新しいセッションを開始します。結果は予測できないため、必ず特定の文面で依頼する必要はありません。次のような変更を依頼できます。

- 各コードファイルの先頭に、役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例などを参考に、コードベースに追加したいドキュメントを Copilot に依頼するコメントを追加し、ユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot が作業を行う様子を確認します。更新のために新しいセッションが開始されます。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![プルリクエストに戻るボタン](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが追加されます。
5. **Files changed** タブを選択して、変更をレビューします。

満足するまで改善を繰り返してください。準備ができたら、ドラフトからレビュー可能な状態に変更して、`main` ブランチにマージします。

![レビュー可能な状態に変更するボタン](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェックセクションでワークフローがキューに登録されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクトチェックが合格します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名（例: **copilot/fix-8**）を `<branch-name>` に指定して、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot クラウドエージェントにコメントで更新を依頼し、反復的に改善できます。たとえば、表示する関連ゲームの数を変更したり、カスタム指示を追加する前に割り当てた issue について、コメントヘッダーや TSDoc のドキュメントコメントを追加したりできます。**Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティのカスタムエージェントを使って実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 で Mission Control から依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が付いています。数分かかることがあるため、作業が完了するまで待ってください。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - 表示モードを切り替える UI コンポーネント
   - ユーザー設定を保存するためのローカルストレージの使用
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェックセクションでワークフローがキューに登録されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクトチェックが合格します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名を `<branch-name>` に指定して、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動して、ハイコントラストモードとライトモードの切り替えが動作し、ページを再読み込みしても設定が保持されることをブラウザーで確認します。

アクセシビリティのカスタムエージェントが、Copilot にアクセシビリティのベストプラクティスに沿った機能を実装するよう案内したことがわかります。アクセシビリティに関する懸念や改善点がある場合は、前の PR と同様にコメントで **@copilot** をメンションして、更新を依頼できます。

## 任意の演習 — さらに作業を委任する

クラウドエージェントは、実際のバックログ項目を渡して別の作業に取り組めるときに効果を発揮します。習慣にするため、リポジトリに issue をいくつか追加し、Copilot に割り当ててみましょう。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

クラウドエージェントの作業環境で、次のことを行いました。

- **このリポジトリのカスタム指示ファイルを確認し、後でクラウドエージェントの出力に反映される様子を確認しました。**
- **Copilot クラウドエージェントに issue を割り当て、非同期で環境を準備し、計画を立て、タスクを実行する様子を確認しました。**
- **アクセシビリティのカスタムエージェントを作成して使い、ハイコントラストモードとライトモードを追加しました。**
- **Copilot agents ページを Mission Control として使い、作業中のアクセシビリティセッションを監視して調整しました。**
- **Copilot が作成したプルリクエストをレビューして改善し、変更を依頼するために `@copilot` をメンションし、ワークフローを承認しました。**

## レビューと次のステップ

クラウドエージェントの作業環境を完了しました。ここで学んだ内容をさらに試す場合は、ほかの作業環境も役立ちます。

- 🖥️ **[VS Code 作業環境](../../vscode/)** — IDE 内で Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI 作業環境](../../cli/)** — Plan モードと Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、任意の `/delegate` によるクラウドエージェントへの連携を使って、ターミナルから 2 つの変更をレビューしてリリースします。

自分のリポジトリで、次のアイデアを試してみましょう。

- リファクタリングやテストカバレッジの issue をクラウドエージェントに割り当てる。
- 別の分野のカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot クラウドエージェントのファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
