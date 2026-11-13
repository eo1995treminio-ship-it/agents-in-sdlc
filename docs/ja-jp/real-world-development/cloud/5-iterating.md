---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善や機能追加を目的に、GitHub Copilot と複数のタスクに取り組みました。コードへのドキュメント追加、デザイン チームが改善を検討できる関連ゲーム機能、高コントラスト モードとライト モードを含むアクセシビリティ機能を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送り、チーム全体のレビューを有効にする前に改善しましょう。

### シナリオ

何度も確認したとおり、生成 AI を利用してもソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに従います。まず、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案をレビューし、その後、チームのレビューを有効にします。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は監督なしで非同期に作業するため、安全を保つためのセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスはコード用に使用するブランチに**限定**されます。
- Cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions workflow の実行には、人間による承認が必要です。
- MCP サーバーなど、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト（PR）、コードへのドキュメント追加を確認します。github.com の標準の PR インターフェイスを使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は、外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは**想定内**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** など、ドキュメントに関するタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が付いています。表示されている場合は Copilot の作業が完了するまで待ちます。数分かかることがあるため、その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく更新されたコードと、追加された TSDoc コメントなどのドキュメントを確認します。変更内容は状況によって異なります。

    変更を確認するときは、演習 2 の最初に見たカスタム指示ファイルから適用される TSDoc コメント、TypeScript の規約、コメント ヘッダーを探してください。

6. 更新内容をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. workflow が承認待ちになっていることを示す表示を探します。
8. workflow が承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. プルリクエストの checks セクションに workflow が登録されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot とプルリクエストで協力する方法は、一方的に変更を受け取ることだけではありません。チームのほかのメンバーと同じように、プルリクエストまたはコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確定的ではないため、必ずこう依頼すべきという文面はありません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの役割を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 前述の例を参考に、コードベースのドキュメントについて変更を依頼するコメントを追加し、ほかのユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot が作業する様子を確認します。更新のために新しいセッションが開始されます。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、新しいコミットがプルリクエストに表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで、変更の依頼とレビューを繰り返してください。問題がなければ、ドラフト PR をレビュー可能な状態に変更して、main ブランチにマージできます。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲームの詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、関連ゲームに関するタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. workflow が承認待ちになっていることを示す表示を探します。
7. workflow が承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. プルリクエストの checks セクションに workflow が登録されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名（例: **copilot/fix-8**）に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

関連ゲーム機能が実装されました。前回と同じように、Copilot cloud agent に更新を依頼できます。たとえば、表示する関連ゲームの数を調整したり（この Issue はカスタム指示の更新前に割り当てたため）、コメント ヘッダーや TSDoc コメントを追加するよう依頼したりできます。前と同じく、**Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントが実装した機能を確認します。この PR には、演習 3 で依頼した高コントラスト モードと、演習 4 で mission control から追加を依頼したライト モードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、高コントラスト モードに関するタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が付いています。表示されている場合は、Copilot が作業を完了するまで数分待ちます。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認するときは、特に次の点に注目してください。
   - モードを切り替える UI component
   - ユーザー設定を保持するためのローカル ストレージの使用
   - 高コントラスト モードとライト モードの CSS またはスタイルの変更
   - アクセシビリティ属性（ARIA label、キーボード ナビゲーションなど）
   - モードの切り替えを管理する JavaScript/TypeScript コード

6. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. workflow が承認待ちになっていることを示す表示を探します。
8. workflow が承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. プルリクエストの checks セクションに workflow が登録されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーで高コントラスト モードとライト モードを切り替えます。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ カスタム エージェントは、アクセシビリティのベスト プラクティスに沿って機能を実装するよう Copilot を導きました。懸念点や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

Cloud agent は、実際のバックログ項目を任せて別の作業に集中できるときに効果を発揮します。練習として、リポジトリにいくつか Issue を作成し、Copilot に割り当ててみてください。たとえば、次のような作業があります。

- ゲームの詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス helper に入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。これらのレッスンで、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、後で cloud agent の出力にどう反映されるかを見ました。
- **Issue を Copilot cloud agent に割り当て**、環境を設定し、計画を立て、非同期に作業する様子を確認しました。
- アクセシビリティ向けの**カスタム エージェントを作成して使い**、高コントラスト モードとライト モードの切り替え機能を追加しました。
- **Copilot agents ページを mission control として使い**、進行中のアクセシビリティ セッションを監視して指示を出しました。
- **cloud agent のプルリクエストをレビューして反復し**、`@copilot` に変更を依頼し、workflow を承認しました。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。さらに学びたい場合は、ほかのハーネスで今回の内容を発展させられます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat の agent mode と MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、agent skill、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` bridge を使い、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けのカスタム エージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent setup workflow のベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
