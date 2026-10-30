---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合は、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]には、どのようなコードが必要かだけでなく、どのように構成してほしいかも記載します。フィルター機能を実装できたので、Copilot が参照した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス単位の指示ファイルがエージェントに読み込まれる方法を確認します。
- コーディング標準が守られるよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

優れた開発組織である Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントでは、コードの内容を繰り返すのではなく、意図や自明ではない判断を説明します。
- `db/` と `src/lib/` にあるエクスポート関数には、目的、パラメーター、戻り値を TSDoc/JSDoc で記載します。該当する場合は、注入可能な `db` 引数についても説明します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記載し、関連コードが変更された場合はコメントも更新します。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使い、これらの慣行に沿ってタスクを実行できるように Copilot に必要な情報を伝えます。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストや希望を Copilot に提供できます。コーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定して、より適切な提案やコードスニペットを得られます。リポジトリ全体、または特定のファイル種類やタスクに指示を設定できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの **すべての** リクエストで Copilot に送信される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種類、タスクに関するガイドラインを提供します。

> [!NOTE]
> ほかの指示形式やサポート状況は、利用する環境によって異なります。特定の形式を前提とする前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、すでに指示ファイルが含まれています。変更による影響を確認するため、まず既存のファイルを見てみましょう。

1. Codespace に戻ります。
2. Codespaces エディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングガイダンスを確認します。これらの指示は、このリポジトリで Copilot とやり取りするすべての場面に適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、指示の適用対象を決める glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルやプロジェクト内の既存ファイルへのリンクを確認します。大きな指示を再利用可能な小さなファイルに分け、Copilot が従う例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは出発点として十分ですが、まだ不足があります。コアの `copilot-instructions.md` を変更し、新しく生成される TypeScript に TSDoc コメントが含まれるようにします。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** というセクションを探します。およそ 35 行目にあります。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存した後、CLI を終了してフィルター機能の会話を再開すると、機能のコンテキストを保ったまま、新しい指示が反映されます。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択画面が表示されたら、フィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトを使って、更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがあり、コードの内容を正しく説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの指示とパス単位の指示ファイルがエージェントに読み込まれる方法を確認しました。
- コーディング標準が守られるよう、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
