---
title: "レッスン 5 - quality-checks skill をカスタマイズして使う"
description: "既存の quality-checks skill を確認し、レポート形式をカスタマイズして、フィルタリング機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書く作業は、コードを記述するだけではありません。手動で動作を確認し、指示ファイルを使って標準に従うようにしました。テストや lint、継続的インテグレーション（CI）のほかの作業はどうでしょうか。

このような作業には、**agent skill** が適しています。skill は、こうした操作を適切に実行する方法を Copilot が理解するために役立ちます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` skill を確認します。
- 結果の形式をカスタマイズします。
- skill を再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニット テスト、lint、型チェックに `quality-checks` skill を使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

Agent skill は、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめたもので、エージェントが必要なときに読み込みます。基本的には、スキル名のフォルダーと、その中の `SKILL.md` で構成されます。Markdown ファイルには、スキルの名前と説明を定義する frontmatter、スキルの概要、呼び出すべきタイミングのガイダンスを記載します。必要に応じて、スクリプトやほかのリソース用のサブフォルダーを追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにある既存のコマンドを使うため、`SKILL.md` だけで構成されています。

Skill は、チームで共有して再利用できるリポジトリ資産として `.github/skills` に配置するか、ユーザーの skill フォルダー `~/.copilot/skills` に配置できます。

## Skill を確認する

Tailspin Toys のチームがユニット テスト、lint、型チェックの実行用に作成した `quality-checks` skill を確認しましょう。

1. Codespace に戻ります。Codespaces のエディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。description は Copilot が skill を呼び出すタイミングを判断するのに役立ちます。
3. 指示を読み、テストと lint の手順をどのように案内しているかを確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更を加える前に Skill を実行する

Skill は Copilot CLI から直接呼び出すことも、自然言語で指示することもできます。3 種類のチェックを実行するよう Copilot に依頼します。

1. Interactive モードでフィルタリングの会話に戻ります。
2. 次のプロンプトを入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールが実際に報告した内容を確認できるレポートに改善しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新した Skill を実行する

変更を加えたので、skill を実際に使ってみましょう。Copilot CLI では、会話を再起動せずに編集済みの skill を再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じプロンプトを入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使いました。このレッスンでは、次のことを行いました。

- ユニット テスト、lint、型チェック用の `quality-checks` skill を確認しました。
- 結果の形式をカスタマイズしました。
- skill を再読み込みして実行しました。

この変更は、機能の PR に含めます。次は、[Playwright MCP サーバーを使って Copilot がサイトを直接操作できるようにします][next-lesson]。

## Skill の例

以下のコミュニティの例は参考資料です。追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と export script: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
