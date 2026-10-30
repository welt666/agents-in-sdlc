---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報にアクセスできるようにします。Copilot にコード生成の指針を伝える方法の 1 つが、[指示ファイル][instruction-files]です。

この演習では、次のことを行います。

- リポジトリのカスタム指示やパス単位の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 既存の指示に従って、フィルター機能の最初のデータ部分（出版社ヘルパー）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再度実行し、追加した標準が生成コードに反映されることを確認します。
- 指示の変更とフィルター機能の一部を `main` にコミットし、次の演習で Copilot が更新されたガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の出力は一定ではありません。この演習では、文字単位で同じ出力になることではなく、指示の変更後に傾向が変わることを確認します。

## 指示ファイル

### シナリオ

優れた開発組織である Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには必ず単体テストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、これらの慣行に沿ってタスクを実行できるように Copilot に必要な情報を伝えます。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストや希望を Copilot に提供できます。より適切な提案やコードスニペットを得るために、コーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体、または特定のタスクやファイルの種類に指示を設定できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの **すべての** リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発対象の概要、ベストプラクティス、全般的なガイダンスなど、ほとんどのチャットや CLI のリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に使用する指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成、単体テストの追加などに関するガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot クラウドエージェントは、リポジトリレベルの指示ファイルと、`applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方をコード生成時に使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく指示ファイルにも対応しています。[指示ファイルのサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法を詳しく扱うことは、この演習の範囲外です。サンプルプロジェクトの例から、一般的な方針を確認できます。

- `copilot-instructions.md` には、開発対象、プロジェクト構成、全体に適用するコーディング規約など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、単体テスト、Astro コンポーネント、データレイヤーなどのファイル種類や、特定のタスクに関する指示を記載します。
- 自然な言葉を使い、明確なガイダンスとコードの例を示します。

指示ファイルの作成や AI の使い方に決まった方法はありません。プロジェクトで試しながら、最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意すると役立ちます。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot を使って指示ファイルを作成することもできます。操作方法は利用する環境によって異なります（たとえば、VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init`）。該当する操作は各環境のレッスンで説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を参照してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意されている指示ファイルを確認します。基本となる `copilot-instructions.md` と、さまざまなタスクに対応する `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の指示にも注目してください。これらの指示は Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示の適用対象を決めるリポジトリルート相対の glob です。この例では、`**/*.test.ts` に一致する TypeScript のテストファイルが対象になります。
5. このプロジェクトで単体テストを作成するときの指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。このように大きな指示を再利用可能な小さなファイルに分け、例を参照できます（パスはリポジトリルートではなく、指示ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメントコメントとファイルヘッダーに関する規則を追加します。

## 変更用のブランチを作成する

ここではベストプラクティスに従って、変更用ブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新し、同じプロンプトを再送信して違いを確認します。

1. Copilot が必要なコンテキストだけを使えるよう、前の演習で開いたエディタータブを閉じます。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、ファイルの変更を適用できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、コードを更新します。ヘルパーファイルとテストなど、複数のファイルが変更されることがあります。
6. 提案されたヘルパーが、`db` クライアントを最初の引数に取り、型付きの出版社配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の出力は一定ではないため、指示がなくてもドキュメントコメントが追加されることがあります。その場合も問題ありません。指示を更新した後の一貫性の改善を確認するのが、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を伝えます。コードの提案を改善するために、リポジトリのコーディング標準を明記しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクションを探します。およそ 27 行目にあります。既存のコーディング規約を確認してください。コードコメントの規則がまだないため、生成されたヘルパーにドキュメントコメントがありません。
3. 既存の規約の直後に、次の Markdown を追加して、ファイルヘッダーと TSDoc のドキュメントコメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベル（`.github/copilot-instructions.md`）または言語、ファイル種類、タスクごとの `*.instructions.md` ファイルとして作成できます。今回追加したドキュメントコメントの規則のようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## プロンプトを再送信して変更を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を再度選択し、Copilot に対象ファイルを示します。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 今度は、提案されたファイルの先頭に次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で使う実際のコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもほかの資産と同様に、ソース管理で管理します。指示の変更とブランチをリポジトリにコミットしてプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで、指示の変更とヘルパーの変更が存在することを次のコマンドで確認します。

   ```bash
   git status
   ```

3. ターミナルで、指示の更新とヘルパーの基盤に関する Copilot の変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する issue を関連付けたプルリクエストを作成します。手動で行う代わりに、Copilot に issue を探させ、issue をクローズする PR を作成してマージしてもらいましょう。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を依頼します。プロジェクトテンプレートでは、GitHub ツールがワークスペースにあらかじめ接続されています。Copilot が初めてツールを使うときに、VS Code から GitHub へのサインインを求められることがあります。画面の指示に従ってください。後の演習では、この接続の仕組みである Model Context Protocol (MCP) を説明します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントの選択で **Agent** が選ばれていることを確認します。
3. 指示ファイルの更新と出版社ヘルパーの基盤を説明し、現在のブランチから PR を作成するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が GitHub 操作の実行を求めた場合は、コマンドを確認して、適切な操作に対して **Allow** を選択します。
5. PR が作成されたら、Copilot に PR をマージしてローカルブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub 操作やシェルコマンドの実行を求められたら、内容を確認し、適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示ファイルを使ってコード生成を依頼し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再送信し、提案されたコードに新しい標準が適用されることを確認しました。
- 指示の更新と出版社ヘルパーの基盤を `main` にコミットし、次の演習で使えるようにしました。

次は[エージェントモード][next-lesson]で指示を活用し、Copilot にコードベース全体にわたる新機能を追加してもらいます。

## リソース

- [GitHub Copilot の指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示をよりよく書くための 5 つのヒント][copilot-instructions-five-tips]
- [個人用の GitHub Copilot カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
