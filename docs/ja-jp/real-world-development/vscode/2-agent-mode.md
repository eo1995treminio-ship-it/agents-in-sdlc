---
title: "演習 2 - Copilot agent mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの単純な更新でも、通常は複数の file の変更やテスト実行などが必要です。開発者は一般的に、必要な file を探し、変更し、テストを実行し、デバッグし、更新し忘れた file を探して、さらに変更を加えるといった作業を行います。必要な作業は尽きません。

このようなときに役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、IDE 内でより自律的に作業します。開発者と同じように、既存のプロジェクト構造を調べ、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる利用方法向けに作られています。Agent mode は IDE 内で動作し、短いフィードバック サイクルで対話しながら作業します。Cloud agent はチームの一員のように非同期で作業し、Issue やプルリクエストを通じてやり取りするペア プログラマーとして設計されています。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連 file を特定し、複数の変更を調整する方法。
- GitHub Copilot Agent Mode が UI とデータ レイヤーにわたって新機能を実装する方法。
- Copilot Agent Mode が生成した変更とテストを、コードベースに merge する前にレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと発行者で絞り込めるようにします。前の演習ですでに publishers helper を追加しました。次は、Copilot Agent Mode でデータ レイヤー、UI、テストの残りを仕上げます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲームの一覧や詳細を表示できます。単一の Astro アプリで、ビルド時に静的 HTML としてページを描画します。ページは frontmatter 内で Drizzle ORM を通じてローカル SQLite データベースを直接検索します。別のバックエンド API やクライアント側 UI framework はありません。再利用可能なデータ アクセス helper は `src/lib/` に置き、インタラクティブな機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` task を実行すると、単一の Astro 開発サーバーが起動します。GitHub Codespace では次の手順で実行できます。

1. codespace に戻ります。前の演習の後なので `main` に戻っているはずです。
2. <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押して、Codespace 内に新しいターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが起動すると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くように求めるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで、<kbd>Command</kbd>（Mac）または <kbd>Ctrl</kbd>（Windows/Linux）を押しながら Astro サーバーの URL `http://localhost:4321` を選択して、Web サイトを開きます。

> [!NOTE]
> codespace では、Codespace のターミナルにある localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動転送されます。これは、Web サーバーをホストしている codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行者、カテゴリなどの詳しい情報が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は手順内で説明します。

Web サイトの初期実装は動作していますが、新しい機能を追加したいと考えています。まずバックログを確認します。テンプレートからプロジェクトをセットアップすると、Issue のバックログが自動的に作成されています。GitHub Copilot に項目を表示してもらいましょう。

1. Chat ビューの agent dropdown で **Agent** を選択します。**Agent** は複数の file にわたって自律的に計画し、変更を実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューの agent picker](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨するものではありません。ワークショップを作成したときに Claude Sonnet 4.5 を使ったため、手順に含めています。コード提案に一貫性を持たせ、体験をよくすることを意図しています。ただし、LLM は確率的に動作するため、提案は記載された内容と異なる場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. Copilot に次の prompt を送信して、バックログの Issue を確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot は、ユーザーに最も役立つと考えられる項目に優先順位も付けています。

## Instruction file を確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に伝える instruction file を確認します。Web サイトへの機能追加方法に関するコンテキストを含む [user interface（UI）][ui-instructions] file を使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - component の設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などの file 種別ごとの詳細な instruction へのリンク。

> [!TIP]
> Instruction file から、ほかの instruction file やプロジェクトの file を参照できます。パスは instruction file の場所からの相対パスです。再利用や複雑な instruction の分割、例やテンプレートの提示に役立ちます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも 3 つの更新が必要です。

- データ レイヤー（`src/lib/`）に残りのフィルタリング ロジックを追加または調整します。
- フィルタリングの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

コードベースに merge する前にテストを実行し、成功を確認する必要もあります。Copilot Agent Mode にこれらの作業を任せましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、instruction file を追加します。

   ![UI instruction file を選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューの agent dropdown で **Agent** が選択されていることを確認します。

   ![Chat ビューの agent picker](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログ内の関連 Issue に基づいて機能を実装するよう、次の prompt を Copilot に送ります。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関係する file を見つける様子を確認します。データ レイヤーの helper、UI、テストを探した後、file を変更してテストを実行します。

   ![Copilot がプロジェクトの file を調べている様子](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、file の変更、テスト実行など、複数の作業を行います。タスクやコードベースの複雑さによっては、数分かかることがあります。作業中、コード エディターに **Keep** と **Undo** が表示される場合があります。作業が完了すると、すべての変更に対して **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot に確認されたら **Continue** を選択してテストを実行します。

   ![Copilot Chat のテスト実行確認ダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot は、タスクが完了してエラーが見つからなくなるまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode のチャット セッションが完了した状態](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動したままのブラウザーに戻り、新機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot Agent Mode が UI とデータ レイヤーにわたって新機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調べ、関連 file を特定し、複数の変更を調整する方法。
- Copilot Agent Mode が生成した変更とテストを、コードベースに merge する前にレビューする方法。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを開きます。

### おまけの演習 — ページ分割を実装する

ゲーム一覧が増えると、ページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割を実装するよう依頼します。次の点を検討してください。

- 既存の instruction file を使うなど、現在のベスト プラクティスに従います。
- ページ サイズをユーザーが選べるようにするか、固定にするかなど、ページ分割の方法を検討します。
- 希望どおりの実装にするために、prompt に必要なガイダンスを含めます。
- Copilot に変更を依頼してコンテキストを与えながら、何度かやり取りする場合があります。Copilot を使うときの通常の流れです。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの役割と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
