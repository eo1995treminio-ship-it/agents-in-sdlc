---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で進め方に合意し、Autopilot で Issue に基づくフィルタリングを実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に、小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりとしたプロセスが必要です。GitHub Copilot CLI は、組織で使われている既存の流れに合わせて作業できるため、正しい方法で必要な機能を構築できます。このレッスンから、エージェントを使った一般的な開発プロセスを始めます。Issue から新機能を作り、コードの妥当性と機能の動作を確認し、最後にプロジェクトへ merge します。

> [!NOTE]
> 機能開発を続ける間、同じ会話とブランチを使います。通常は作業対象ごとに別のブランチや PR を作りますが、ここでは主な概念に集中するため手順を簡略化します。

まず、このレッスンでは次のことを行います。

- GitHub Issue から Copilot の新しい会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

機能の作業を続けながら、リポジトリの instruction を更新し、既存の `quality-checks` skill をカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、利用者はカテゴリや発行者でゲームを絞り込む必要があります。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細があります。Plan モードで決定し、範囲を定めた実装を Autopilot に許可します。

## 背景

AI コーディング エージェントを開発プロセスに導入しても、基本は変わりません。むしろ、基本がさらに重要になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を記述した Issue から始めます。
2. 構築する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行し、コードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト（PR）を作成します。
7. コードがレビューされ、継続的インテグレーションの処理が成功したら、コードを merge します。

> [!NOTE]
> 詳細な手順はチームや組織によって異なりますが、多くの場合は上記の流れを調整したものです。

この標準的な方法に従うと、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビュー プロセスを経ることを確認できます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前にユーザーの入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントはコードを書き、テストを実行し、タスクが完了するまで反復して自律的に作業します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルタリング機能に着手する前に Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR が merge されたことを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押します。
4. `main` を更新し、フィルタリング作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7`（または同様の番号）で始まることを確認します。`#` を使うと、GitHub 上の Issue やプルリクエスト（PR）をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、機能の実装やタスクの実行を Copilot に任せる前に、そのアプローチを決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan モードに切り替え、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を検討して計画を作成します。

3. フィルタリングの動作について質問された場合は、希望に沿って回答します。どの回答にも間違いはありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）を押して計画を展開します。
5. 計画全体を確認します。
6. 決定した内容と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしました。次は計画を実装します。Autopilot を使って Copilot に作業を任せます。

Autopilot を使うと、完了したと判断するまで Copilot が問題に取り組み、反復して改善します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンに表示される同様の選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まることがあります。先に計画をレビューしてください。依存関係が不足している、または port が競合していると Copilot が報告した場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードの生成後は、ほかのコードと同じように、merge する前にレビューします。コードを確認し、サイトを起動して問題がないか検証しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルタリング機能とテストを確認します。
3. 結果を Issue と計画時に決めた内容と照らし合わせます。
4. コードのレビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードに問題がないことを確認しました。次は実際に動かします。前と同じようにアプリを起動し、Codespaces で転送された port からサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces で port `4321` が利用可能になったら **Open in Browser** を選択します。
3. 計画で決めたカテゴリと発行者によるフィルタリング、およびその組み合わせを試します。
4. reset と結果が空の場合の動作が、Issue と決定した内容に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

複数の会話モードを使って機能を構築し、レビューしました。このレッスンでは次のことを行いました。

- GitHub Issue から Copilot の新しい会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、文書化されたプラクティスに沿ってコードを生成できるよう、[custom instruction を使います][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
