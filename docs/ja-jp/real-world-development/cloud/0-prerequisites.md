---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot クラウドエージェントの演習を始める前に、必要な環境を準備します。Tailspin Toys リポジトリのコピーを作成し、[Codespace][codespaces] を起動します。Codespace では、指示ファイルを編集したり、クラウドエージェントが作成した成果をレビューしたりします。

## ラボ用リポジトリをセットアップする

コードを作成するためのリポジトリのコピーを用意します。[テンプレート][template-repository]から新しいインスタンスを作成すると、演習に必要なファイルが含まれたリポジトリを利用できます。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボリポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選択して、自分用のコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップを受講している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリテンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えてください。この後の演習で使用します。

## Codespace を作成する

続いて、Codespace を使って演習に取り組みます。

[GitHub Codespaces][codespaces] は、ブラウザーで直接コードの作成、実行、デバッグを行えるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートするフル機能の IDE を利用できます。

1. 作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい Codespace を作成する画面](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスを手作業でインストールするよりはずっと速く完了します。その間に GitHub Copilot のほかの機能を確認してもよいでしょう。次の演習でそれらを紹介します。

> [!CAUTION]
> 次の演習で Codespace に戻ります。今はブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で実行できます。どちらも必要な前提条件がインストールされた環境を提供します。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択してください。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボリポジトリのコピーを作成できました。また、Copilot クラウドエージェントと一緒に作業するための Codespace の作成も開始しました。

## 次のステップ

クラウドエージェントが従うカスタム指示を追加しましょう。[演習 1 - カスタム指示][next-lesson]に進んでください。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
