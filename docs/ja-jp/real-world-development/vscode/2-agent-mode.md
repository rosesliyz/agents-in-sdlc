---
title: "演習 2 - Copilot Agent Mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数ファイルの変更やテストの実行が必要です。開発者は必要なファイルを探し、変更を加え、テストを実行し、問題をデバッグし、変更し忘れたファイルを探して、さらに更新を加えることになります。作業は次々に増えていきます。

こうした作業に役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、統合開発環境 (IDE) 内でより自律的に動作します。開発者と同様に、プロジェクトの構成を調べ、必要な更新を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードとクラウドエージェントは異なる用途向けです。エージェントモードは IDE 内で作業し、すばやくフィードバックを得ながら対話できます。クラウドエージェントはチームの一員のように非同期で作業し、issue とプルリクエストを通じてやり取りします。

この演習では、次の方法を学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数ファイルを調整して変更する方法。
- GitHub Copilot Agent Mode がユーザーインターフェイス (UI) とデータ層にまたがる新機能を実装する方法。
- Copilot Agent Mode が生成した変更とテストをレビューしてから、コードベースにマージする方法。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリと発行元で絞り込めるようにします。前の演習で発行元ヘルパーを追加しました。ここでは Copilot Agent Mode を使って、残りのデータ層、ユーザーインターフェイス (UI)、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握します。

この Web サイトは、開発者向けのテーマを使ったボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。単一の Astro アプリで、ページはビルド時に静的なハイパーテキストマークアップ言語 (HTML) としてレンダリングされます。ページの frontmatter から Drizzle オブジェクトリレーショナルマッパー (ORM) を使ってローカルの SQLite データベースを直接クエリするため、別のバックエンドアプリケーションプログラミングインターフェイス (API) やクライアント側の UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、必要な対話機能は標準のドキュメントオブジェクトモデル (DOM) API を使った小さなスコープ付き Astro `<script>` で実装します。

### Web サイトを起動する

Web サイトを簡単に実行できるように、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. 前の演習を終えて `main` に戻った状態で、codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルウィンドウを開きます。
3. フィルター機能の作業用ブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが準備できると、次のような URL のバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を選択するときに、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押したままにして Web サイトを開きます。

> [!NOTE]
> codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは、Web サーバーをホストしている codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、現在の機能を確認します。主な機能は次のとおりです。

- **ホームページ**: ボードゲームのタイトル、画像、説明の一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な場合は手順で説明します。

Web サイトの初期実装は動作しますが、機能を拡張します。まず、バックログを確認しましょう。テンプレートからプロジェクトを作成したときに issue のバックログが自動的に作成されました。GitHub Copilot に一覧を表示するよう依頼します。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。**Agent** エージェントは、複数ファイルにわたる変更の計画と実装、ターミナルコマンドの実行、ツールの呼び出しを自律的に行います。

   ![Chat ビューのエージェント選択画面のスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使ったため、手順に記載しています。よい体験を得られるよう、比較的一貫したコード提案を期待しています。ただし、大規模言語モデル (LLM) は確率的に動作するため、手順の説明と異なる提案が表示されることがあります。これは正常であり、想定される動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる Model Context Protocol (MCP) コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信し、issue のバックログについて質問します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーにとって最も役立つと思われる項目を優先順位付けしたことも確認できます。

## 指示ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを伝える指示ファイルを確認します。Web サイトに機能を追加する方法についての情報を含む [ユーザーインターフェイス (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) ファイルを使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - ファイル形式ごとの指示へのリンク。次のものが含まれます。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所を基準にします。これにより、指示を再利用できる小さなファイルに分割し、コード生成の参考例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、アプリケーションに少なくとも次の 3 つの更新が必要です。

- データ層 (`src/lib/`) の残りのフィルター処理を追加、または改善する。
- フィルター動作のテストを追加、または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

また、コードベースにマージする前にテストを実行し、合格する必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. 現在の Copilot との会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルの選択例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択画面のスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 引き続き **Claude Sonnet 4.5** がモデルとして選択されていることを確認します。
5. バックログ内の関連 issue の要件に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要な機能に関係するファイルを見つける様子を確認します。データ層のヘルパー、UI、テストを探し、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトファイルを調査する様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによって、数分かかる場合があります。作業中は、コードエディターに **Keep** と **Undo** が表示されることがあります。作業中に選択する必要はありません。完了すると、すべての変更について **Keep** または **Undo** を選択できます。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテストの実行を確認するダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はタスクが完了し、エラーを検出しなくなるまでコード生成とテストを繰り返します。

   ![Copilot Agent Mode の完了した Chat セッションのスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動したブラウザーに戻り、新機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次の手順

この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次のことを行いました。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数ファイルを調整して変更する方法を確認しました。
- GitHub Copilot Agent Mode を使って、UI とデータ層にまたがる機能を実装しました。
- Copilot Agent Mode が生成した変更とテストをレビューしてから、コードベースにマージする方法を確認しました。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の探索 — ページングを実装する

ゲームの一覧が増えると、ページングが必要になります。この演習で学んだスキルを使い、Copilot にページングの実装を依頼してください。次の点を考慮します。

- 既存の指示ファイルを使うなど、既存のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定値にするかなど、ページングの方法を検討します。
- 希望する実装に必要なガイダンスを Copilot に伝えるプロンプトを作成します。
- Copilot と反復して、変更を依頼したりコンテキストを追加したりする必要がある場合があります。Copilot と作業するときの通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
