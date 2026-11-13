---
title: "演習 1 - Custom instructions (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストにアクセスできるようにします。特定のコンテキストを Copilot に伝える方法はいくつかあります。代表的な方法は[instruction file][instruction-files]です。instruction file で、コード生成に関するガイダンスを伝えます。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位で適用される instruction file によって、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどう伝わるかを確認します。
- *現在の* instruction を使って、フィルタリング用の最初のデータ スライス（publishers helper）を作成します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する標準を追加します。
- 同じ prompt をもう一度実行し、生成されたコードが新しい標準に従う様子を確認します。
- instruction の更新とフィルタリングの最初の変更を `main` に commit し、次の演習で Copilot が更新したガイダンスを使えるようにします。

> [!CAUTION]
> 生成されたコードは、設定した標準から外れる場合があります。Copilot は非決定的に動作します。この演習の目的は、instruction の更新後に振る舞いがどう変化するかを確認することであり、出力を 1 文字単位で一致させることではありません。

## Instruction file

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のような内容です。

- データ レイヤーには、必ずユニット テストを用意します。
- UI はダーク モードにし、現代的な印象にします。
- コード内のドキュメントには TSDoc の doc comment を使います。
- 各 file の先頭に、その file の役割を説明するコメント ブロックを追加します。

Instruction file を使うと、これらのプラクティスに沿ってタスクを実行するために必要な情報を Copilot に伝えられます。

### Custom instructions

Custom instruction を使うと、コンテキストや好みを Copilot に伝え、コーディング スタイルや要件を理解させられます。より関連性の高い提案やコード スニペットを得るのに役立ちます。希望するコーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体に適用する instruction も、特定のファイル種別に適用する instruction も作成できます。

Instruction file は次の 2 種類です。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の instruction file です。使用する技術スタック、開発中のものの概要、ベスト プラクティスなど、ほとんどの chat や CLI リクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けに作成できます。TypeScript や Astro などの言語、UI component の作成、ユニット テストの追加などに関するガイドラインを記載できます。

> [!NOTE]
> IDE で作業する場合、instruction file は Copilot Chat でのコード生成にのみ使われます。コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの instruction file と、`applyTo` frontmatter が指定された `*.instructions.md` file の両方を使います。
>
> Copilot は [ほかの標準に基づく instruction file][custom-instructions-support]（AGENTS.md や CLAUDE.md など）にも対応しています。

### Instruction file を管理するためのベスト プラクティス

Instruction file の作成について詳しく説明することは、このワークショップの範囲外です。ただし、サンプル プロジェクトには代表的な方法が示されています。概要は次のとおりです。

- `copilot-instructions.md` には、作成中のもの、プロジェクトの構造、全体に適用するコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` は、ユニット テスト、Astro component、データ レイヤーなどのファイル種別や、特定のタスクに対する具体的な指示に使います。
- 自然な言葉で明確に説明します。望ましいコードと望ましくないコードの例も示します。

Instruction file に唯一の正解の作り方はありません。AI の使い方にも唯一の方法はないため、実際に試しながらプロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実した instruction file のセットを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの file があります。
>
> Copilot に instruction file を生成してもらうこともできます。各 surface での方法は異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など）。必要な場合は、利用中の surface のレッスンで説明します。
>
> テンプレートや出発点を探している場合は、instruction file、カスタム エージェントなどのリソースを集めたリポジトリ [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

このリポジトリに含まれる instruction file を確認します。中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` file があります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** に関するガイダンスにも注目してください。これらの内容は Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro file、Drizzle データ レイヤー、テストなどに関する instruction があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` field は、instruction の適用対象を決める glob（リポジトリのルートからの相対パス）です。この例では、`**/*.test.ts` に一致する TypeScript のテスト file が対象です。
5. このプロジェクトでユニット テストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。`unit-tests.instructions.md` などの別の instruction file や、プロジェクト内の file へのリンクを確認してください。大きな instruction セットを小さく再利用しやすい単位に分け、コード生成時に Copilot が参考にできる例を示せます。（ここに記載されたパスはリポジトリのルートではなく、instruction file からの相対パスです。）

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントはまだ必須ではありません。次の手順で、TSDoc doc comment と file comment header のルールを追加します。

## 変更用のブランチを作成する

ベスト プラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## Instruction の更新*前に* Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は手順内で説明します。

Custom instruction の影響を確認するため、まず現在の instruction file を使って prompt を送ります。その後 file を更新し、同じ prompt をもう一度送信して違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. Copilot が helper の保存先を把握できるよう `src/lib/publishers.ts` を開きます。
3. Chat ビューの agent dropdown で **Agent** を選び、Copilot が file を変更できるようにします。

   ![Chat ビューの agent picker](../../../_images/shared-chat-mode-selector.png)

4. 次の prompt を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。helper file とテストなど、複数の file に変更が及ぶ場合があります。
6. 提案された helper が型付き関数で、最初の引数に `db` client を受け取り、型付きの publisher 配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に基づいています。
7. 提案されたコードに TSDoc doc comment と file-level comment header が**含まれていない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加する場合があります。その場合も問題ありません。重要なのは、instruction の更新後に一貫性が向上することです。

## リポジトリ全体に適用する標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コーディング標準を記載し、コード提案の品質を高めましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントのルールはありません。そのため、生成された helper には doc comment がありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加します。Copilot に file comment header と TSDoc doc comment を追加するよう指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、instruction file は全体的なガイダンス向けのリポジトリ レベル（`.github/copilot-instructions.md`）にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。今回追加した doc comment のルールのような、プロジェクト全体に適用する標準にはリポジトリ レベルの file が適しています。

## Prompt を再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` を選び、Copilot の対象を適切な file にします。
3. 前と**同じ prompt** を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案された file の先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc comment が含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot を新しいプロジェクト標準に沿うように導き、次の演習の土台となる実際のコードに適用できました。

## Instruction とブランチを commit して push する

Instruction file もリポジトリ内のほかの asset と同じように、ソース管理で管理します。変更を commit してブランチをリポジトリに push しましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しいターミナルを開きます。
2. 次のコマンドを実行し、instruction と helper の変更が含まれていることを確認します。

   ```bash
   git status
   ```

3. instruction の更新と、Copilot が helper の土台に対して変更したすべての file を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらの file も追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成して merge する

ブランチを push したので、プルリクエストを作成してバックログのドキュメント標準に関する Issue に関連付けます。手動で行う代わりに、すでにプロジェクトに接続されている GitHub ツールを使って Copilot に依頼します。Issue を検索し、それを close する PR を作成して、merge してもらいます。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を依頼します。プロジェクト テンプレートには GitHub ツールがあらかじめ接続されています。Copilot が最初にツールを使うとき、VS Code で GitHub へのサインインを求められることがあります。画面の指示に従って許可してください。後の演習で、この接続の仕組み（Model Context Protocol）を扱います。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）を押して Copilot Chat ビューを開き、agent picker で **Agent** が選択されていることを確認します。
3. instruction file の更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。PR には instruction の変更と、新しい publishers helper の土台の両方を記載します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探し、PR を作成します。
5. Copilot に GitHub 操作の **Allow** を求められた場合は、コマンドを確認して必要に応じて **Allow** を選択します。
6. PR が作成されたら、Copilot に merge とローカル ブランチの `main` への切り替えを依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot に GitHub 操作と shell command の **Allow** を求められた場合は、コマンドを確認して必要に応じて **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成して merge できました。

## まとめと次のステップ

Copilot がこのプロジェクトの instruction file からコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- *既存の* instruction を使ってコード生成 prompt を送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する標準を追加しました。
- 同じ prompt をもう一度実行し、提案されたコードが新しい標準に従う様子を確認しました。
- 次の演習で作業を続けられるよう、instruction の更新と helper の土台を `main` に commit しました。

次は [agent mode][next-lesson]を使い、コードベース全体に新機能を追加しながら instruction を活用します。

## リソース

- [GitHub Copilot の custom instruction file][instruction-files]
- [Custom instruction を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot の custom instruction を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用 custom instruction][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
