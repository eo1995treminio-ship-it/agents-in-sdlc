---
title: "演習 5 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom agents][previous-lesson] |
|:--|

GitHub Copilot を agent mode で使うと、コードベースを調べ、変更を提案し、file を編集してコマンドを実行するなど、自律的に作業します。自分のマシンでリアルタイムに実行されるため、VS Code の Copilot Chat ではツールの呼び出しや file の編集を確認できます。差分をインラインでレビューし、個々の変更を受け入れたり取り消したりできます。また、エディターを離れずに追加の prompt を送って、会話を調整したり作業を拡張したりできます。

この演習では、次のことを行います。

- アクセシビリティ エージェントの作業をリアルタイムで監視します。
- 提案された file の編集をレビューして受け入れます。
- セッション中にエージェントへ指示し、高コントラスト モードにライト モードを追加します。

## シナリオ

[演習 4][previous-lesson]で使ったアクセシビリティ カスタム エージェントは、高コントラスト モードを実装しています。作業中に、デザイン チームから追加の依頼がありました。既定のテーマが暗すぎると感じるユーザーのために、ライト モードも切り替えられるようにします。新しい会話を最初から始める代わりに、既存のセッションに指示を出して作業を拡張します。

## Copilot の進捗を監視する

Copilot Chat ビューには、エージェントが実行する各手順が表示されます。調査した file、実行したコマンド、提案された編集などです。各 file の編集は差分と **Keep** / **Undo** の操作とともにチャットに表示されます。

1. 演習 4 で使った Copilot Chat ビューに戻ります。
2. エージェントが作業している間、チャットをスクロールしてツールの呼び出しを確認します。file の読み込みや編集、実行を求められたターミナル コマンドが表示されます。
3. エージェントが file の編集を提案すると、差分がインラインで表示されます。
    - 変更を受け入れるには **Keep** を選択します。
    - 変更を取り消し、エージェントに再度作業させるには **Undo** を選択します。
4. チャットの上部にある **Files changed** を開くと、このセッション中にエージェントが変更した file をすべて確認できます。

> [!NOTE]
> ターミナル コマンドの実行などで許可が必要な場合、エージェントは一時停止し **Allow** を表示します。内容を確認し、許可してもよいコマンドだけを承認してください。

5. 高コントラスト モードの実装が終わるまで待ちます。最後に、変更内容の要約が表示されます。

## エージェントにライト モードの追加を依頼する

高コントラスト モードができたので、同じ会話でライト モードの切り替え機能も追加します。同じチャットを続けると、エージェントは前の作業で扱った component、スタイルの方法、ローカル ストレージのパターンなどのコンテキストを保持します。

1. Copilot Chat の入力欄に、次の追加 prompt を送ります。

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. エージェントは既存の変更を調べて、ライト モードの追加を提案します。
3. 高コントラスト モードのときと同じように、追加された編集をレビューします。各差分で **Keep** または **Undo** を選択します。
4. エージェントの方向性が希望と異なる場合（たとえば、好みでない方法で toggle UI をリファクタリングする場合）は、チャット入力欄の隣にある **Stop** ボタンで停止し、明確にするための追加メッセージを送ります。

> [!TIP]
> 進行中のセッションに指示するほうが、最初からやり直すより早く済みます。追加の prompt で「CSS を簡潔にして、selector の重複ではなく CSS variable を使ってください」のように調整したり、「toggle を増やさず、ライト モードを既存の設定パネルに加えてください」のように方向を修正したり、「両方の toggle にキーボード ショートカットも追加してください」のように範囲を広げたりできます。

## 作業内容をレビューする

commit する前に、エージェントが変更したすべての内容を確認します。

1. VS Code で **Source Control** ビューを開きます。
2. 変更された file の一覧を確認します。Astro component、スタイル、関連テストが更新されているはずです。
3. 変更された file をいくつか開き、差分を確認します。カスタム エージェントから指示したアクセシビリティのパターン（ARIA 属性、キーボード ナビゲーション、セマンティック HTML、ローカル ストレージへの保存）が反映されていることを確認します。
4. 問題がなければ、Source Control パネルから変更を stage して commit します。次のレッスンで公開します。

## まとめと次のステップ

Copilot Chat の agent mode で進行中のセッションを監視して方向付け、各 file の編集を確認し、変更を受け入れるか取り消し、コンテキストを保ったまま追加の prompt を送って作業を拡張しました。

このレッスンでは次のことを行いました。

- エージェントの作業をリアルタイムで監視しました。
- file の編集を個別に受け入れたり取り消したりしました。
- 進行中のセッションに追加の prompt を送り、作業の方向を調整しました。

[次のレッスン][next-lesson]では、アクセシビリティの作業をプルリクエストとして公開し、このラボを通じてローカルのカスタム エージェントが行った変更をレビューします。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [Agent mode の使用][agent-mode]

---

| [← 前のレッスン: Custom agents][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
