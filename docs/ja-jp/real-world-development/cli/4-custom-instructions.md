---
title: "レッスン 4 - Custom instructions で Copilot を導く"
description: "リポジトリの instruction を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]には、作成するコードだけでなく、コードの構成方法も記述できます。フィルタリング機能を構築したので、Copilot が参照した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス単位で適用される指示ファイルがエージェントにどう伝わるかを確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば次のとおりです。

- コメントでは、コードを言い換えるのではなく、意図や自明ではない判断を説明します。
- `db/` と `src/lib/` にあるエクスポート済み関数は、目的、パラメーター、戻り値を TSDoc/JSDoc で記述します。`db` 引数を注入できる場合は、その引数についても記述します。
- 再利用可能な Astro component は `Props` の契約を説明し、関連するコードの変更時にはコメントも最新の状態にします。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使うと、これらのプラクティスに沿ってタスクを実行するために必要な情報を Copilot に伝えられます。

## 指示ファイル

Custom instruction を使うと、コンテキストや好みを Copilot に伝え、コーディング スタイルや要件を理解させられます。希望するコーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体に適用する instruction も、特定のファイル種別に適用する instruction も作成できます。

指示ファイルは次の 2 種類です。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイドラインを提供します。

> [!NOTE]
> ほかの instruction 形式への対応状況は、環境によって異なります。特定の形式を利用する前に、[custom instruction の対応状況に関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

作業を始めやすいよう、スターター プロジェクトには指示ファイルがいくつか含まれています。変更を加える前に、既存のファイルを確認して影響を把握しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この instruction は、このリポジトリで Copilot を使うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` field は、指示を適用するファイルを決める glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルやプロジェクト内のファイルへのリンクを確認します。これにより、大きな指示のセットを小さく再利用しやすいファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、まだ不足があります。新たに生成する TypeScript に TSDoc comment が追加されるよう、中心となる `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** セクションを探します。35 行目付近にあります。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの instruction を読み込みます。instruction を保存した後、フィルタリングのコンテキストを保ったまま会話を終了して再開し、新しいガイダンスを読み込ませます。

1. フィルタリングの会話が開いている場合は `/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートから `game-filters-cli` ブランチで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルタリングの会話を選び、`game-filters-cli` ブランチが checkout されていることを確認します。
3. 更新したガイダンスをコードに適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパス単位で適用される指示ファイルがエージェントにどう伝わるかを確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、lint とテストを一貫して実行するために、再利用可能な [quality-checks skill をカスタマイズして実行します][next-lesson]。

## リソース

- [Copilot CLI 用 custom instruction を追加する][instruction-files]
- [Custom instruction の対応状況][custom-instructions-support]
- [Custom instruction を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
