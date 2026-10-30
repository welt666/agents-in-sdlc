---
title: "演習 1 - カスタム指示 (クラウドエージェント)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot クラウドエージェント →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報にアクセスできるようにします。[指示ファイル][instruction-files]を使うと、何をしてほしいかだけでなく、どのように実行してほしいかも Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示やパス単位の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加します。

> [!NOTE]
> VS Code や CLI の作業環境とは異なり、ここではプロンプトの変更前後を比較しません。Copilot クラウドエージェントは GitHub issue を非同期に処理するため、リアルタイムで並べて効果を確認するのは困難です。この作業環境の後半で、エージェントが作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

優れた開発組織である Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには必ず単体テストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、これらの慣行に沿ってタスクを実行できるように Copilot に必要な情報を伝えます。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストや希望を Copilot に提供できます。これにより、より適切な提案やコードスニペットを得られます。好みのコーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体、または特定のタスクやファイルの種類に指示を設定できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの **すべての** リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発対象の概要、ベストプラクティス、全般的なガイダンスなど、ほとんどのチャットや CLI のリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に使用する指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成、単体テストの追加などに関するガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot クラウドエージェントは、リポジトリレベルの指示ファイルと、`applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方をコード生成時に使用します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく指示ファイルにも対応しています。[指示ファイルのサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法を詳しく扱うことは、この演習の範囲外です。ただし、サンプルプロジェクトの例から、一般的な方針を確認できます。

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

## プロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意されている指示ファイルを確認します。基本となる `copilot-instructions.md` と、さまざまなタスクに対応する `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の指示にも注目してください。これらの指示は Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示の適用対象を決めるリポジトリルート相対の glob です。この例では、`**/*.test.ts` に一致する TypeScript のテストファイルが対象になります。
5. このプロジェクトで単体テストを作成するときの指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。このように大きな指示を再利用可能な小さなファイルに分け、例を参照できます（パスはリポジトリルートではなく、指示ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメントコメントとファイルヘッダーに関する規則を追加します。

## リポジトリ全体に適用する標準を追加する

この演習で編集するのは、プロジェクト全体に適用するルールを追加する箇所だけです。コード内のドキュメントを TSDoc コメントとファイルレベルのコメントヘッダーで記載するようにします。後の演習で issue を割り当てると、クラウドエージェントがこの指示を読み込みます。

編集の前に、作業用のブランチを作成します。クラウドエージェントは issue の対象ブランチにある指示を読み込みますが、ここでは変更をきちんとコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を伝えます。コードの提案を改善するために、リポジトリのコーディング規約を明記しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクションを探します。およそ 27 行目にあります。既存の規約を確認してください。コードコメントの規則がまだないため、生成されたヘルパーにドキュメントコメントが含まれていません。
3. 既存の規則の直後に、次の Markdown を追加して、ファイルヘッダーと TSDoc のドキュメントコメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベル（`.github/copilot-instructions.md`）または言語、ファイル種類、タスクごとの `*.instructions.md` ファイルとして作成できます。今回追加したドキュメントコメントの規則のようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

クラウドエージェントは、issue の対象ブランチから指示ファイルを読み込みます。次の演習で Copilot に issue を割り当てると、`main` からブランチを作成します。クラウドエージェントに指示の変更を読み込ませるには、その変更を `main` にマージしておく必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で issue を割り当てるとき、クラウドエージェントはこの指示を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチ作成を省略して、変更を `main` に直接コミットできます。ブランチを使う手順は、実際のプロジェクトでの作業方法に近づけるためのものです。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。この作業環境の残りの演習で、クラウドエージェントが作成するプルリクエストを通じて、その標準が使われる様子を確認します。

次は、[Copilot クラウドエージェントに最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示をよりよく書くための 5 つのヒント][copilot-instructions-five-tips]
- [awesome-copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
