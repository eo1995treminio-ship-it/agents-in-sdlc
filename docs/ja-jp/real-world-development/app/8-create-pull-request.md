---
title: "レッスン 8 - 機能 PR を作成して merge する"
description: "フィルタリング、instruction、skill の更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

フィルタリング機能の実装、instruction の更新、skill の更新、品質保証（QA）プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成します。星評価のプルリクエスト（PR）は自分で merge しましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、instruction の更新、skill の更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡略化するため、フィルタリングと品質確認の作業全体を 1 つのセッションとブランチで進め、この PR にまとめています。

このレッスンでは、次のことを行います。

- Agent Merge とは何か、merge のライフサイクルをどのように自動化するかを学びます。
- 機能の PR 全体と検証結果を確認します。
- レビュー後にのみ Agent Merge を許可し、PR が merge されたことを確認します。

## シナリオ

フィルタリング機能のワークフロー全体で、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、merge の許可を開発者の管理下に置いたまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** は、GitHub Copilot app でプルリクエストを取り込むために必要な残りの作業を自動化します。有効にすると、app のセッションがプルリクエストを読み込み、CI チェックの失敗を修正し、レビュー コメントに対応し、必要な場合は rebase するなど、PR を妨げている問題を解決します。その後、GitHub で許可されると merge します。バックグラウンドで実行され、app を再起動しても継続し、プルリクエストが merge されると自動的に停止します。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその役割を任せられますが、コードの編集と merge を行うには、引き続き明示的な許可が必要です。merge を許可する前に、許可される操作と作業内容をレビューしてください。

## Agent Merge で PR を管理する

コードの作成とレビューが完了しました。Agent Merge に PR プロセスを管理させましょう。

1. agent picker で **Default agent** を選択します。
2. **Create PR** の横にあるドロップダウンを選択します。
3. **Agent Merge** を選択します。ボタンが **Agent Merge** に変わります。
4. **Agent Merge** ボタンを選択してプロセスを開始します。ドロップダウンの項目を選んだだけでは開始されません。

Agent Merge のプロセスが始まります。次の作業を行います。

- タイトルと説明を含むプルリクエストを作成します。
- Issue からセッションを開始した場合は、説明本文に関連する Issue を記載します。
- target ブランチに rebase するか、merge 競合に対処します。
- CI を監視して、すべてのチェックが成功することを確認します。
- 他の開発者や Copilot code review からのフィードバックがないか PR を監視し、コメントを解決するために更新します。
- 必要に応じて、すべてが成功した後に PR を自動的に merge します。

すべてのチェックが成功したら、Agent Merge に PR も merge させましょう。

5. merge の許可を与える前に、PR の差分と検証結果をレビューします。
6. **Agent Merge** の横にあるドロップダウンを選択します。
7. **Merge pull request** の横にチェックが付いていることを確認します。
8. 次のように明示して、エージェントに merge を依頼します。

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 進捗を確認し、報告された問題に対処します。続行する前に PR の状態が **Merged** であることを確認してください。merge 権限が有効になっていることや、エージェントが実行中であることだけでは、merge 完了の確認になりません。

> [!IMPORTANT]
> Agent Merge は、リポジトリの保護や権限不足を回避しません。続行する前に、それらの問題に対処してください。

## まとめと次のステップ

コード生成、テストと検証、プルリクエストのプロセスなど、開発プロセスの一部を自動化しました。次のことを行いました。

- Agent Merge とは何か、merge のライフサイクルをどのように自動化するかを学びました。
- 機能の PR 全体と検証結果を確認しました。
- レビュー後にのみ Agent Merge を許可し、PR が merge されたことを確認しました。

次は、[既存の Canvas を使い、トリアージ Canvas を作成します][next-lesson]。エージェントと一緒に作業を確認、計画、視覚化する方法を試します。

## リソース

- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
