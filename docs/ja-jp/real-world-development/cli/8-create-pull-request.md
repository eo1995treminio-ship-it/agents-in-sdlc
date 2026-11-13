---
title: "レッスン 8 - 機能 PR を作成して merge する"
description: "フィルタリング、instruction、skill の更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、instruction の更新、skill の更新、品質保証（QA）プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成します。星評価のプルリクエスト（PR）は自分で merge しましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、instruction の更新、skill の更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡略化するため、フィルタリングと品質確認の作業を 1 つの会話とブランチにまとめ、この PR に含めています。

このレッスンでは、次のことを行います。

- Agent Merge とは何か、merge のライフサイクルをどのように自動化するかを学びます。
- 機能全体の変更と検証結果を確認します。
- フィルタリングの PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR が merge されたことを確認します。

## シナリオ

フィルタリング機能を実装するまで、Copilot を使って計画、実装、検証を行いました。Tailspin Toys は、merge の許可を開発者の管理下に置いたまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** は、プルリクエストを取り込むために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題（失敗した継続的インテグレーション（CI）チェックの修正、レビュー コメントへの対応、必要な場合の rebase など）に取り組みます。その後、リポジトリで許可されていれば GitHub の auto-merge を有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその役割を任せられます。有効にする前に、作業をレビューして問題がないことを確認してください。

## Agent Merge で PR を管理する

コードの作成が完了しました。まとめてレビューし、PR を作成して、残りのプロセスを Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを確認します。すべて成功したら merge を実行します。
7. Copilot から「PR #14 was squash-merged successfully.」などのメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、merge queue、リポジトリ設定、または不足している権限を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可される場合は、レビュー済みの merge を手動で完了します。

## まとめと次のステップ

コード生成、テストとコードの検証、プルリクエストのプロセスなど、開発プロセスの一部を自動化しました。次のことを行いました。

- Agent Merge とは何か、merge のライフサイクルをどのように自動化するかを学びました。
- 機能全体の変更と検証結果を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR が merge されたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に役立つ [Copilot CLI のスラッシュ コマンドをさらに試します][next-lesson]。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
