---
title: "演習 4 - Custom agents"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## Custom agent とは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown file を作成してエージェントを定義すると、特定の作業を効果的に実行するための指示、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に与えられます。チームの専門知識を再利用できるエージェントにできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントなどです。

カスタム エージェントは、プロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown file で定義します。各 file には `name` と `description` を含む YAML frontmatter と、エージェントの振る舞い、専門知識、指示を記述する Markdown prompt が必要です。

### カスタム エージェントと agent skill の違い

カスタム エージェントと[agent skill][agent-skills-concept]には、役割が重なる部分があります。どちらも主に Markdown file で定義し、AI に作業方法を伝えます。分かりやすく言うと、**カスタム エージェントは作業者、skill はツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一部として skill（場合によっては別のエージェントも）を指揮します。このラボのアクセシビリティ カスタム エージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエスト ワークフローの skill や、テストを実行・管理する skill を呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI のほかの使い方と同じように、環境やシナリオに合わせてテストし、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにすることに取り組んでいます。最近のユーザー フィードバックでは、テキストと背景色のコントラストが不十分なため、現在のダーク テーマを読みにくいと感じる人がいることがわかりました。このアクセシビリティの課題に対応するため、デザイン チームはユーザーが切り替えられる高コントラスト モードを求めています。

アクセシビリティは重要なので、できるだけ早く実装します。機能の生成にはカスタム エージェントを使います。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、高コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントは、すでに作成されています。Copilot への指示内容を把握するため、内容を確認しましょう。

codespace に戻り、ターミナルを開いて、アクセシビリティ作業用に `main` から新しいブランチを作成します（演習 3 のフィルタリング PR とは分けておきます）。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` field を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成における中核的な責任。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェントのドロップダウンには、`.github/agents` で定義されたすべてのカスタム エージェントが表示されます。カスタム エージェントを選択すると、そのエージェントの指示とツールに合わせてチャット セッションを構成できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は手順内で説明します。

1. エージェントのドロップダウンで、まだ選択されていない場合は **Agent** を選択します。

   ![Chat ビューの agent picker](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にあるエージェントのドロップダウンを選択します。アクティブなエージェントが表示されています。既定では **default** です。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティ エージェントに次の prompt を送ります。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat はコードベースを調べ、変更を提案してプロジェクトの file に適用します。各編集は file path とレビュー可能な差分とともにチャットに表示されます。
6. 編集が追加されると **Files changed** の表示が更新され、このセッションで変更された file を確認できます。

> [!NOTE]
> この作業には数分かかることがあります。Copilot はリポジトリに実際の変更を加えています。作業中に Astro component、CSS、関連テストなど既存の file を編集します。

次の演習では、進行中の作業をレビューし、追加の指示を出します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野向けに調整する [GitHub Copilot のカスタム エージェント][custom-agents]を確認しました。カスタム エージェントを使うと、チームの専門知識や標準を再利用可能なエージェントに記述し、特定の作業をより効果的に行えるよう Copilot を導けます。

次の概念を確認しました。

- カスタム エージェントの定義方法。
- Copilot Chat の agent mode でカスタム エージェントを使う方法。

次は、[エージェントの作業を監視して方向付けます][next-lesson]。変更を確認しながら、同じセッションにライト モードの切り替え機能も追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
