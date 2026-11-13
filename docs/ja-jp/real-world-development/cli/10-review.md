---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、さらに学ぶためのリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローに取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、フィルタリングの Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト（PR）を手動で merge しました。
- フィルタリングの Issue から始め、Plan モードでアプローチを決め、Autopilot モードで構築し、Interactive モードでレビューしました。
- custom instruction でエージェントを導き、既存の `quality-checks` skill をカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol（MCP）サーバーを追加し、実際のブラウザーでフィルタリング機能を確認しました。
- 品質保証（QA）用のカスタム エージェントを作成して選び、要件、カバレッジ、skill の結果、ブラウザーでの検証結果を評価しました。
- フィルタリングの変更全体をレビューし、フィルタリングの PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認しました。

## 完成したもの

ワークショップでは、更新済みの `main` から作成した個別のブランチで、2 つの PR を作成します。

1. **星評価:** 既存の `starRating` と未評価であることを示す表示を、ゲーム カードに追加します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、instruction を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連するテストを追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡略化するため、作業を 1 つの PR にまとめました。

## さまざまな方法で検証する

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー操作という複数の方法でコードを確認しました。`quality-checks` skill はユニット テスト、lint、型チェックを実行し、追加した形式で結果を報告しました。QA エージェントは、要件とテスト カバレッジのレビューにこれらの結果を組み合わせ、PR の前にまとめて確認しました。

追加したテストで、実際に不足している部分を補う必要があります。QA の実行で新しいテストが不要と判断される場合もあります。merge を許可する前に、コードと根拠をレビューしてください。変更を加えた場合は、影響を受ける検証結果を更新します。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業の結果に影響します。このワークショップでは、instruction の更新、skill のカスタマイズ、QA プロファイルの作成、MCP サーバーの設定を行いました。こうしたカスタマイズを会話間で再利用し、チームのニーズに応じて調整してください。Instruction は標準を設定し、skill は繰り返し実行するタスクを記述し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。構築の前にアプローチを検討するときは **Plan**、範囲を絞った変更に対話しながら取り組むときは **Interactive**、明確に範囲が定まったタスクには **Autopilot** を使います。日常的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

環境と同じくらい、コンテキストも重要です。何を、なぜ、どのように構築するのかを明確に記述すると、出力が大きく変わります。

## さらに試す

中心的なワークフローは完了しました。Copilot CLI には、さらに便利な機能があります。

- `/review` でコード レビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を話し合い、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に進めます。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることが上達への近道です。製品コード、趣味のコード、何年も作ろうと思いながら手を付けていなかった小さなアプリなどで活用してください。チームと学びを共有し、互いに学びましょう。そして、ドキュメントも確認してください。

ターミナルから Tailspin Toys をさらに拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
