---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks skill、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill で自動チェックを実行し、Playwright MCP でブラウザーからフィルタリング機能を確認しました。次は、明確な QA プロセスを備えたカスタム エージェントに、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが instruction、skill、MCP ツールとどのように連携するかを確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選び、フィルタリングの Issue に照らして調査結果をレビューします。

## シナリオ

Tailspin Toys は、プルリクエスト（PR）を作成する前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザーでの動作を一貫した方法でレビューしたいと考えています。カスタム エージェントを使うと、QA プロセスを調整し、再利用できるレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専門的な構成です。プロファイルには、エージェントの目的、指示、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの instruction はチームの標準を説明します。`quality-checks` skill は繰り返し実行するチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、それらを使って要件を評価し、調査結果を報告する方法を Copilot に伝えます。これらの機能を置き換えるものではなく、新しい会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。QA が実行するチェックと、その作業の境界を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. default agent に、新しいカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと作業の境界を取り込んだことを確認します。検証だけを依頼したのに、機能を変更してしまうような不完全または広範すぎるエージェントを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は省略できますが、指定するとエージェントを分かりやすい名前で表示できます。
3. プロファイルの指示を読み、QA が要件を起点とし、リポジトリの instruction に従い、`quality-checks` skill を実行して、Playwright MCP を使うことを確認します。
4. QA が根拠を示し、実装コードを変更する前に確認を求め、変更を commit したりプルリクエストを作成したりしないことを確認します。
5. これらの責任や境界がプロファイルに含まれていない場合は、続行する前に default agent に修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成したら、同じフィルタリングの会話を再開し、QA を選択して、Issue と計画の決定事項を利用できるようにします。

1. 次の prompt でエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧に表示されない場合があります。エージェントは存在するため、上記のコマンドで有効にできます。

2. 次の prompt で、QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、レポートを読みます。

## まとめと次のステップ

再利用可能な専門的な役割をワークフローに追加し、その作業をレビューしました。このレッスンでは、次のことを行いました。

- カスタム エージェントが instruction、skill、MCP ツールとどのように連携するかを確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選び、フィルタリングの Issue に照らして調査結果をレビューしました。

実装、skill の更新、QA プロファイル、テスト、検証レポートが揃いました。次は、[機能 PR にまとめて Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用カスタム エージェントを作成する][create-agents]
- [カスタム エージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
