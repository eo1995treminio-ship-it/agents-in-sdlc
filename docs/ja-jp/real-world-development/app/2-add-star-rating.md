---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "GitHub Copilot app で最初のエージェント セッションを開始し、ゲーム カードに小さな変更を加えて、最初のプルリクエストとして merge します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認し、クイックチャットを試しました。次は**エージェント セッション**を開始して、プロジェクトに最初の変更を加えます。変更は小規模なものにします。ゲームのデータにはすでに星評価がありますが、ホーム ページのゲーム カードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとして merge します。

このレッスンでは、次のことを行います。

- エージェント セッションを開始し、セッションの構成を学びます。
- エージェントに小さく範囲を絞った変更を依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを作成して merge します。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードには、タイトル、カテゴリ、発行者、説明だけが表示されています。ウォームアップとして、各カードに既存の評価を表示するようエージェントに依頼します。小さく自己完結した変更なので、最初のセッションにぴったりです。

## セッションの構成

**セッション**は、独立したワークスペースで実行されるエージェントとの会話です。各セッションには**専用の git worktree とブランチ**が割り当てられます。そのため、一方で機能を追加しながら、別のセッションでバグを修正するなど、変更を競合させずに複数のセッションを同時に実行できます。セッションはリポジトリごとにサイドバーにまとめて表示されます。選択するとセッションを切り替えられます。

セッション内には、エージェントとの**会話**、ファイルの調査や編集に使う**ツール操作**、差分を確認できる**変更されたファイル**の一覧が表示されます。

## セッションを開始して変更を依頼する

新しいセッションを開始し、プロジェクトの調査と機能の実装に取りかかります。[app の設定][prior-lesson]では、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用のセッションを作成して変更を依頼します。

1. GitHub Copilot app に戻ります（または開きます）。
2. サイドバーの上部にある **+** を選択してセッションを開始します。
3. リポジトリに `tailspin-toys` を選択します。
4. プロンプト ボックスの下で **new working tree** と **Interactive** モードを選択し、次の prompt で変更を依頼します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して prompt を Copilot に送信します。

Copilot app は、最初にプロジェクトの独立したコピーである新しい worktree を作成します。続いてプロジェクトを調べ、新機能に必要なファイルを見つけてコードを作成します。Copilot app を使って新しい機能を追加できました。

## 差分をレビューする

AI が生成した変更は、小さなものでも merge する前にレビューします。Copilot app 内で変更を確認しましょう。

1. app の右上隅にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選択すると、変更されたファイルと差分が表示されます。

    ![Create PR ボタンの右側に Toggle review panel ボタンがある GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲームの詳細を表示する中心的なファイル `GameCard.astro` にコードが追加されていることを確認します。評価があれば表示し、`starRating` が `null` なら `No rating yet` を表示する、次のような小さなブロックです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot を含む生成 AI ツールは決定論的ではなく確率的に動作するため、実際のコードは上記と異なる場合があります。大まかな構造は似たものになります。

## 変更を確認する

ブラウザーを開く前に、エージェントが実行した自動チェックの結果をレビューします。数値の `starRating` と、`null` の場合の代替表示をテストで確認していることを確認してください。前提条件が不足していたり、チェックがスキップされていたりする場合は成功ではありません。インストールの要求は内容を確認してから承認します。

コードを読むだけで動くと判断するのではなく、Copilot に Web サイトを起動してもらい、更新された UI を確認します。ブラウザー Canvas で開くよう依頼できます。

> [!TIP]
> Canvas は Copilot app 内で使えるインタラクティブな widget です。後のレッスンではカスタム Canvas を調べて独自のものも作成します。ここでは組み込みのブラウザー Canvas を使います。

1. 次の prompt で、アプリを起動してブラウザー Canvas にページを開くよう Copilot に依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザー ウィンドウが開きます。
3. 評価があるゲーム カードに 5 段階の値が表示されることを確認します。
4. 確認が終わったら、次の prompt でこのセッションが起動した開発サーバーを停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初のプルリクエストを作成して merge する

機能を作成できました。次は、新しいコードを既存のコードベースに取り込むプルリクエスト（PR）を作成します。

1. 右上隅にある **Create PR** を選択します。
2. 認証を求められた場合は **Sign in with your browser** を選択し、画面の指示に従います。
3. リポジトリの policy により、PR の作成前に関連する Issue が必要だと Copilot が説明した場合は、その手順に従うよう確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** bubble を選択し、review pane で PR を開きます。必要に応じて内容をレビューします。
6. 問題がなければ **Ready to merge** を選択します。
7. 表示されたダイアログで **Merge pull request** を選択し、PR を merge します。

## まとめと次のステップ

GitHub Copilot app で最初の変更をリリースしました。具体的には、次のことを行いました。

- エージェント セッションを開始し、セッションの構成を学びました。
- エージェントにゲーム カードの小さく範囲を絞った変更を依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成し、チェックをレビューして、明示的に merge しました。

次は、フィルタリングの Issue から始めて [Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
