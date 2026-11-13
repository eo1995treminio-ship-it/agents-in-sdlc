---
title: "演習 1 - Custom instructions (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストにアクセスできるようにします。[指示ファイル][instruction-files]を使うと、何をしてほしいかだけでなく、どのように進めてほしいかも Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位で適用される指示ファイルによって、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどう伝わるかを確認します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムで影響を並べて示すのは困難です。後続の演習で cloud agent が作成するプルリクエストをレビューするときに、この指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のような内容です。

- データ レイヤーには、必ずユニット テストを用意します。
- UI はダーク モードにし、現代的な印象にします。
- コード内のドキュメントには TSDoc コメントを使います。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使うと、これらのプラクティスに沿ってタスクを実行するために必要な情報を Copilot に伝えられます。

### Custom instructions

カスタム指示を使うと、コンテキストや好みを Copilot に伝え、コーディング スタイルや要件を理解させられます。これにより、より関連性の高い提案やコード スニペットを得やすくなります。希望するコーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体に適用する指示も、特定のファイル種別に適用する指示も作成できます。

指示ファイルは次の 2 種類です。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。使用する技術スタック、開発中のものの概要、ベスト プラクティスなど、ほとんどのチャットや CLI のリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けに作成できます。TypeScript や Astro などの言語、UI component の作成、ユニット テストの追加などに関するガイドラインを記載できます。

> [!NOTE]
> IDE で作業する場合、指示ファイルは Copilot Chat でのコード生成にのみ使われます。コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの指示ファイルと、`applyTo` front matter が指定された `*.instructions.md` ファイルの両方を使います。
>
> Copilot は [ほかの標準に基づく指示ファイル][custom-instructions-support]（AGENTS.md や CLAUDE.md など）にも対応しています。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。ただし、サンプル プロジェクトには代表的な方法が示されています。概要は次のとおりです。

- `copilot-instructions.md` には、作成中のもの、プロジェクトの構造、全体に適用するコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` は、ユニット テスト、Astro component、データ レイヤーなどのファイル種別や、特定のタスクに対する具体的な指示に使います。
- 自然な言葉で明確に説明します。望ましいコードと望ましくないコードの例も示します。

指示ファイルに唯一の正解の作り方はありません。AI の使い方にも唯一の方法はないため、実際に試しながらプロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実した指示ファイルのセットを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。各環境での方法は異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など）。必要な場合は、利用中の環境のレッスンで説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタム エージェントなどのリソースを集めたリポジトリ [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** に関するガイダンスにも注目してください。これらの内容は Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示の適用対象を決める glob（リポジトリのルートからの相対パス）です。この例では、`**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトでユニット テストを作成するための指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。`unit-tests.instructions.md` などの別の指示ファイルや、プロジェクト内のファイルへのリンクを確認してください。大きな指示のセットを小さく再利用しやすい単位に分け、コード生成時に Copilot が参考にできる例を示せます。（ここに記載されたパスはリポジトリのルートではなく、指示ファイルからの相対パスです。）

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントはまだ必須ではありません。次の手順で、TSDoc コメントとファイル先頭コメントのルールを追加します。

## リポジトリ全体に適用する標準を追加する

ここでは、コード内ドキュメントを TSDoc コメントとファイル先頭コメントで記述する、プロジェクト全体のルールを追加します。後続の演習で cloud agent に Issue を割り当てると、このルールが適用されます。

編集の前に、作業用のブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み取りますが、変更をきれいにコミットするためにブランチを使います。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コーディング標準を記載し、コード提案の品質を高めましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントのルールはありません。そのため、生成されたヘルパー関数には TSDoc コメントがありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加します。Copilot にファイル先頭コメントと TSDoc コメントを追加するよう指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルは全体的なガイダンス向けのリポジトリ レベル（`.github/copilot-instructions.md`）にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。今回追加した TSDoc コメントのルールのような、プロジェクト全体に適用する標準にはリポジトリ レベルの指示ファイルが適しています。

## 指示ファイルの更新をコミット、push、マージする

Cloud agent は、Issue の対象ブランチにある指示ファイルを読み込みます。次の演習で Copilot に Issue を割り当てるとき、Copilot は `main` からブランチを作成します。そのため、cloud agent が変更後の指示を使うには、変更を `main` に取り込む必要があります。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。次の演習で Issue を割り当てると、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチの作成を省略して `main` に直接 commit できます。ここでは、実際のプロジェクトと同じ手順を練習するためにブランチを使います。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。以降のハーネスで cloud agent が作成するプルリクエストをレビューすると、この標準が適用されていることを確認できます。

次は、[Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の custom instruction file][instruction-files]
- [Custom instruction を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot の custom instruction を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
