---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでの 2 つの演習では、ユーザー エクスペリエンスの改善と機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。GitHub.com の[agents ページ][agents-page]もその 1 つです。この mission control では、オープンなプルリクエスト（PR）があるすべてのエージェント タスクを確認できます。実行された操作を調べたり、進行中のセッションに追加の指示を出したりできます。

このレッスンでは、次のことを行います。

- agents ページで cloud agent のタスクを監視します。
- 進行中のセッションに指示を出し、機能の追加を依頼します。

## シナリオ

高コントラスト モードの作成をエージェントに割り当てた後、チームはライト モードも追加するよい機会だと考えました。サイトのスタイル更新と切り替え機能の追加をすでに進めているため、ライト モードも含めるのが自然です。高コントラスト モードだけでなくライト モードも追加されるよう、エージェントに追加の指示を出します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインのタスク一覧を確認します。次のような、Copilot に割り当てたタスクが表示されます。
    - コードベースのドキュメントの更新。
    - ゲームを変更する API の生成。
    - Web サイトへの高コントラスト モードの追加。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。次のような操作が表示されることがあります。
    - リポジトリからのコードの checkout。
    - Copilot が作業する環境の作成。
    - MCP サーバーの設定。
    - 割り当てられたタスクを完了するためのさまざまな手順。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト（PR）ペインも確認してください。PR と変更された file を確認し、作業を監視できます。

## Cloud agent に追加の指示を出す

実行中のタスクを確認したので、高コントラスト モードの作業中にライト モードの切り替え機能も追加するよう Copilot に依頼します。

1. 高コントラスト モードを追加するセッションを選択します。タイトルは、Copilot が付けた名前や作業状況によって異なります。

    ![Mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことが表示されるまで数分待ちます。次のようなメッセージが表示されたら作業開始です。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot agents ページで、Steer active session while Copilot is working ダイアログを表示した cloud agent タスク](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に取り入れることを確認します。

## Copilot の作業を待つ

前回と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順が終わると、新しい依頼を作業の流れに取り入れます。

作業には数分かかります。その間に、これまでに学んだことを振り返ってみましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視して指示するための中心的な場所、Copilot agents ページを確認しました。mission control では、進行中と完了済みのタスクを追跡し、実行中の作業をレビューし、作業中のタスクに追加の指示を出せます。

次の概念を確認しました。

- mission control と agents ページで cloud agent のタスクを監視する。
- 進行中のセッションに指示を出し、機能の追加を依頼する。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: Custom agents][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
