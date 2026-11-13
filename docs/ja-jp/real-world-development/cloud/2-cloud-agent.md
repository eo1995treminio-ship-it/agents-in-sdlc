---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom instructions][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなど、さまざまな形があります。GitHub Copilot cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。時間のかかる作業を任せて開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次のことを学びます。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- 範囲を明確に定めた Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。最初のサイトを開発した請負業者が、ドキュメントを十分に整備しませんでした。まず、アプリケーション内のエクスポートされたすべての関数に TSDoc コメントを追加します。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのタイトルを「関連ゲーム」として表示します。まずは UX の受け入れテストに使える状態にしたいので、完成度の高い実装はまだ必要ありません。現在は作業の妨げになっていますが、優先度の高い Issue がほかにあります。

どちらも、優先順位が下がりやすい一方で、Copilot cloud agent に任せやすいタスクです。非同期で作業する cloud agent に任せてほかの仕事を進め、後から戻って成果をレビューできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]など、いくつかの方法で作業を依頼できます。割り当てると、Copilot は進捗を記録するドラフト プルリクエストを作成し、環境を準備してタスクに取りかかります。作業中でも完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を適切に定めた指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな操作も、詳しく見ると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階を踏んで学び、試し、調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への指示方法を確認しました。カスタム指示を扱う演習をすでに終えている場合は、`.github/copilot-instructions.md` が機能することを確認しています。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリにある指示ファイルを読み取り専用で確認し、後で適用状況を確かめましょう。

リポジトリの github.com の Web UI、または起動済みの codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` に一致するファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てるとき、作成されたプルリクエストに TSDoc コメント、TypeScript の規約、コメント ヘッダーが反映されているか確認してください。後の演習で PR をレビューするときに、もう一度取り上げます。

## Copilot cloud agent の開発環境を設定する

誰が作業する場合でも、コードの作成には通常、特定の開発環境と初期設定が必要です。Copilot にタスクを割り当てる場合も同様です。Copilot は SWE と同じように作業します。

Cloud agent は、作業時の環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用の setup workflow][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係をインストールできます。このラボの流れを進めやすくするため、workflow はあらかじめ設定されています。単一の Astro アプリのために、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションと seed 済みの SQLite データベースを Copilot が利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブから setup steps をテストできるようにします。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 自分の steps に必要な最小限の権限を設定します。Copilot の操作には専用の token が与えられます。
    permissions:
      # setup steps の一部としてリポジトリを clone する場合（依存関係のインストールなど）は、contents: read 権限が必要です。clone しない場合は、steps の完了後に Copilot が自動的に clone します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js（アプリ全体は Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカル SQLite データベースをマイグレーションして seed し、ビルドとテスト用データを用意します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub workflow と同じ構成ですが、次の点に注目してください。

- `copilot-setup-steps` という 1 つの job があります。この job は、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` trigger により、リポジトリの **Actions** タブから workflow を手動で実行できます。Copilot が実行するのを待たずに、workflow が成功するかテストできます。

## ドキュメントを追加する

誰もがドキュメントの重要性を理解していますが、多くのプロジェクトでは情報が古かったり、ドキュメント自体がなかったりします。この種の技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメント作成が得意なので、cloud agent に割り当てるのに適した Issue です。バックグラウンドでドキュメントを作成してもらい、後の演習で成果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

  ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト（PR）を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo list を作成したことを確認します。

> [!NOTE]
> Copilot の todo list が PR に表示されるまで、数分かかる場合があります。Copilot は環境を作成し（前述の workflow を実行）、プロジェクトを分析して、問題に取り組む方法を決めています。

13. todo list と、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッション画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が見えるように、ブラウザーのウィンドウを更新する必要がある場合があります。

16. セッションをリアルタイムでスクロールし、Copilot が問題を解決する様子を確認します。コードを調査して状態を把握すること、計画を考えて判断するために一時停止すること、コードを作成することなどが含まれます。

この作業には数分かかることがあります。Copilot cloud agent の主な目的の 1 つは、非同期で作業している間にほかのタスクへ集中できるようにすることです。この利点を活用するため、もう 1 つのタスクも cloud agent に割り当て、その間にアプリケーションへ機能を追加します。

## ゲームの詳細ページに関連ゲームを追加する

前述のとおり、GitHub Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザイン チームのために関連ゲームを追加する作業に時間がかからないとしても、その時間をほかのタスクに使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次のテキストを入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることがわかります。

![目の絵文字で Copilot が Issue に取り組んでいることを示す](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent は SWE と同じように作業するため、常に監視する必要はありません。作業が完了したらレビューできます。続いて、カスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使って、Issue を Copilot に割り当て、非同期で作業させる方法を学びました。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などを Copilot に任せられます。

次の概念を確認しました。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- 範囲を明確に定めた Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

Cloud agent がバックグラウンドで作業している間に、カスタム エージェントの作成と使用に取り組みます。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで扱った custom instruction にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent setup workflow のベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: Custom agents →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
