---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスを改善し、機能を追加するいくつかのタスクを GitHub Copilot と進めてきました。コードへのドキュメント追加、デザインチームが改善を重ねられる関連ゲーム機能の構築、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要であれば Copilot にフィードバックを送って改善しましょう。

### シナリオ

これまで何度も説明したとおり、生成系人工知能 (AI) を使ってもソフトウェア設計と開発・運用 (DevOps) の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チームのレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認します。

## GitHub Copilot クラウドエージェントのセキュリティ

Copilot クラウドエージェントは、監督なしで非同期にタスクを実行するため、安全を保つためのセキュリティ制約が設けられています。

- Copilot はリポジトリへの読み取りアクセス権を持ち、コードを作成するために使うブランチ**だけ**への書き込みアクセス権を持ちます。
- クラウドエージェントは GitHub Actions 内で実行され、作業用に別の一時的な環境を作成します。
- GitHub Actions のワークフローは、人間が承認するまで実行されません。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。モデルコンテキストプロトコル (MCP) サーバーも対象です。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot クラウドエージェントが作成した最初のプルリクエスト (PR) である、コードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールによってブロックされたという警告が表示される場合があります。Copilot は、外部 MCP サーバーへの呼び出しなど、外部リソースへのアクセスが既定で制限されているため、これは**想定された状態**です。必要に応じて、[Copilot クラウドエージェントのファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあるため、休憩を取ったり、ここまでに学んだ内容を振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新たに更新されたコードを確認します。TSDoc のドキュメントコメントなどが含まれます。変更の詳細は状況によって異なります。

    変更を確認するときは、TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーを探してください。これらは演習 2 の最初に確認したカスタム指示ファイルによるものです。

6. 更新内容をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. 承認待ちのワークフローがあることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. ワークフローが PR のチェックセクションに追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが合格します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するときは、一方的に変更を受け取るだけではありません。チームのメンバーと同じように、PR のコメントやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は非決定的なため、依頼文を固定することはできません。たとえば、次の変更を依頼できます。

- すべてのコードファイルの先頭に、その役割を簡単に説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. **@copilot** をユーザーと同じようにメンションし、生成されたドキュメントへの変更をコメントで依頼します。上記の例、またはコードベースに追加してほしい別のドキュメントを依頼してください。
2. **View Session** を選択して、Copilot の作業を確認します。Copilot が更新のための新しいセッションを開始します。
3. **Back to pull request** を選択して、プルリクエストに戻ります。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更を確認します。

満足できるまで、Copilot と繰り返し作業できます。問題がなければ、PR をドラフトからレビュー可能な状態にして `main` ブランチにマージできます。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次に、ゲーム詳細ページに関連ゲームを表示する issue を解決するため Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. 承認待ちのワークフローがあることを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. ワークフローが PR のチェックセクションに追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが合格します。完了まで数分かかることがあります。
9. **省略可能:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名（例: **copilot/fix-8**）に置き換えてください。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot クラウドエージェントに更新を依頼して作業を反復できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーや TSDoc のドキュメントコメントを追加したりできます。この issue はカスタム指示を更新する**前**に割り当てたことに注意してください。前と同じように、**Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントが実装した機能を確認します。この PR には演習 3 で依頼したハイコントラストモードと、演習 4 のミッションコントロールから追加を依頼したライトモードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を保持するローカルストレージの使用
   - ハイコントラストモードとライトモード用のカスケーディングスタイルシート (CSS) またはスタイル変更
   - アクセシビリティ属性 (Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作など)
   - モードの切り替えを管理する JavaScript/TypeScript コード

6. 更新内容をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. 承認待ちのワークフローがあることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. ワークフローが PR のチェックセクションに追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが合格します。完了まで数分かかることがあります。
10. **省略可能:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名に置き換えてください。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次に、アプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードの切り替えをテストします。ページを再読み込みした後も設定が保持されることを確認します。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに従って機能を実装するよう Copilot を導いたことを確認できます。アクセシビリティ上の懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** に変更を依頼できます。

## 省略可能な演習 — 作業をさらに依頼する

クラウドエージェントは、実際のバックログ項目を任せて別の作業に集中できるときに役立ちます。練習として、リポジトリにいくつか issue を追加して Copilot に割り当ててみてください。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者の関心フォームを作成する。
- ゲーム一覧ページにページングを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

クラウドエージェントの環境を完了しました。各レッスンを通して、次のことを行いました。

- このリポジトリに含まれるカスタム指示ファイルを確認し、後でクラウドエージェントの出力にどのように影響するかを把握しました。
- issue を Copilot クラウドエージェントに割り当て、環境の準備、計画、非同期の実行を確認しました。
- アクセシビリティ用カスタムエージェントを作成して使い、ハイコントラストとライトモードの切り替えを追加しました。
- Copilot エージェントページをミッションコントロールとして使い、アクセシビリティセッションを監視して途中で誘導しました。
- Copilot クラウドエージェントのプルリクエストをレビューして反復し、変更を依頼するために `@copilot` をメンションし、ワークフローを承認しました。

## レビューと次の手順

クラウドエージェントの環境を完了しました。引き続き学ぶ場合は、ほかの環境で今回の内容をさらに試せます。

- 🖥️ **[VS Code 環境](../../vscode/)** — IDE から直接 Copilot Chat のエージェントモードと MCP 統合を確認します。
- 💻 **[コマンドラインインターフェイス (CLI) 環境](../../cli/)** — Plan と Autopilot モード、エージェントスキル、カスタム品質保証 (QA) エージェント、Playwright MCP、Agent Merge、クラウドエージェントに戻る省略可能な `/delegate` ブリッジを使い、ターミナルから 2 つの変更をレビューして提供します。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテストカバレッジに関する issue をクラウドエージェントに割り当てる。
- 別の分野のカスタムエージェントを作成する。
- 別の技術スタック用に `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot クラウドエージェントのファイアウォールを設定する][agent-firewall]

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
