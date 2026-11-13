---
title: "レッスン 1 - GitHub Copilot app をインストールする"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使うデスクトップ アプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。自分ですべての作業を行うのではなく、複数のエージェントをそれぞれ独立したワークスペースで並行して動かし、繰り返し作業を自動化するワークフロー向けに設計されています。Node.js のインストールとプロジェクトの準備が完了したので、次は app をインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトを app に追加します。
- テンプレートで準備されたバックログなど、ワークスペースの使い方を確認します。
- クイックチャットを試し、app について質問します。

## シナリオ

チームでは、増え続けるバックログに取り組むために AI エージェントを導入しています。Copilot app では、Issue を選び、エージェントを実行し、変更をレビューして、プルリクエストを merge するまで、1 か所から指示できます。このレッスンでは、app をインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> Copilot Free、Copilot Student、または有料プラン（Pro、Pro+、Business、Enterprise）などの対象プランが必要です。Copilot Business または Copilot Enterprise の場合、管理者が **GitHub Copilot app** policy を有効にしている必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使うには、まずインストールします。Windows、macOS、Linux 向けのバージョンがあります。app をインストールして認証し、Tailspin Toys リポジトリを追加しましょう。

1. ブラウザーで [GitHub Copilot app のランディング ページ][download-app]を開きます。
2. 使用しているプラットフォーム向けの app をダウンロードし、ランディング ページの手順に従ってインストールします。
3. インストール後、app を開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って GitHub.com または GHE.com アカウントでサインインします。データ所在地に対応する GitHub Enterprise Cloud を使う場合は **GitHub Enterprise Cloud** を選び、求められたら `*.ghe.com` のホスト名を入力します。

   GitHub Enterprise Server を使う場合は、まずクラウド アカウントでオンボーディングを完了します。その後、**Settings** を開き、**Accounts**、**Add account**、**GitHub Enterprise Server** の順に選択します。[Enterprise Server のサインイン ガイド][ghes-sign-in]に従い、ブラウザーで app を承認します。

5. オンボーディング中にリポジトリを選択する画面が表示された場合は、テンプレートから作成した Tailspin Toys リポジトリを選択します。リポジトリの選択をスキップし、後からプロジェクトを追加することもできます。
6. テーマを選ぶよう求められたら選択し、オンボーディングを完了します。
7. サイドバーの **Sessions** にリポジトリが表示されているか確認します。すでに一覧にある場合は、再度追加する必要はありません。
8. まだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた owner とリポジトリ名を使います。
9. サイドバーにリポジトリが表示されることを確認します。このレイアウトでは `tailspin-toys` などのリポジトリ フォルダーが **Sessions** の下にまとまり、その下に個別のセッションが表示されます。

## ワークスペースの使い方を確認する

プロジェクトを接続したら、アプリの使い方を確認します。サイドバーには主に次の領域があります。

- **New** - Copilot との新しいチャット セッションを開始します。
- **Pull requests** - GitHub とネイティブに統合されたプルリクエストです。app を離れずに参照、絞り込み、CI 状態の確認、レビューを行えます。
- **Issues** - 同じように表示される Issue です。Issue の参照や絞り込み、Issue からのセッション開始を行えます。
- **Automations** - スケジュールまたはオンデマンドで実行する、保存済みのエージェント タスクです。やることリストの管理、定期的な保守など、手間のかかる作業を任せるのに役立ちます。振り返りでは次のステップとして紹介します。ワークショップの演習には含まれません。
- **Customize** - MCP server、プラグイン、skill などの component を追加して Copilot app を拡張します。Playwright MCP の設定で使います。
- **Chats** - 独自のブランチやワークスペースが不要な質問やブレインストーミング向けの簡易的な会話です。最初のチャットを始めると表示されます。見当たらない場合は、**Sessions** の横にある **+** を選んで最初のチャットを開始します。
- **Sessions** - リポジトリのフォルダーと、その中のエージェント セッションをまとめます。`tailspin-toys` などのリポジトリ名を選ぶとリポジトリ ビューが開き、配下のセッションを選ぶと会話に戻ります。セッションごとに独立したワークスペースで動くため、変更が競合せず複数のセッションを同時に実行できます。最初のセッションは星評価を追加するときに開始します。

ワークショップを進めながら、ワークスペースを確認していきます。

> [!TIP]
> 迷ったときは Copilot に質問しましょう。操作方法が分からない場合や、何かが可能かどうか知りたい場合は、Copilot が案内してくれます。

### 用意されたバックログを確認する

バックログのないプロジェクトはほとんどありません。Tailspin Toys も同様です。テンプレートからリポジトリを作成したときに用意されたバックログを確認しましょう。

1. サイドバーの **Sessions** で、個別のセッションではなく Tailspin Toys のリポジトリ フォルダーを選択します。リポジトリの上部にあるナビゲーションで **Issues** を選びます。
2. Issue 番号ではなく、次のタイトルで探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Issue を選択して内容を読みます。各 Issue はエージェント セッションの開始点にもなります。簡単な最初の変更が終わったら、フィルタリングの Issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧は自動的に絞り込まれ、Copilot app に追加したリポジトリの項目だけが表示されます。ほかのリポジトリの作業項目を確認するには、そのリポジトリも app に追加してください。

## クイックチャットを試す

app に慣れるには、app 自体について質問するとよいでしょう。**quick chat** はその用途に適しています。ブランチや worktree を作成せずに質問やブレインストーミングができるため、セッションを必要としない、その場限りの簡単な質問に向いています。

1. サイドバーで **Chats** の横にある **+** を選択して、新しいチャットを開きます。**Chats** がまだ表示されていない場合は **Sessions** の横にある **+** を選択して最初のチャットを始めます。その後 **Chats** が表示されます。
2. app のセッションの仕組みについて質問します。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションが独立した git worktree で動くため、変更が競合せず複数のエージェントを並列に実行できることが分かります。会話は続けることも、新しいチャットを始めることもできます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースの使い方を確認しました。次のことを学びました。

- app をインストールして GitHub にサインインする。
- GitHub リポジトリからプロジェクトを追加する。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つける。
- クイックチャットで、その場限りの簡単な質問をする。

次は、[最初のエージェント セッションを開始し][next-lesson]、ゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の使用を開始する][getting-started]
- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
