---
title: "演習 2 - GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot のクラウドエージェントは、コードの更新や機能の追加などを自律的に実行するように設計されています。作業が完了すると、人間の開発者がレビューできるドラフトプルリクエスト (PR) を作成します。面倒な作業を任せて開発プロセスを加速し、開発者がより大きな課題に集中できるようになります。

このレッスンでは、Copilot クラウドエージェントを使って次のことを確認します。

- コード生成環境をカスタマイズする。
- 操作を安全に実行する。
- issue の範囲を明確にすることの重要性。
- issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した契約業者は、ドキュメントを十分に整備していませんでした。実際、ドキュメントはほとんどありません。まず、アプリケーション内のすべてのエクスポートされた関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示し、ユーザーがさらにゲームを閲覧できるようにします。現時点では完成度の高い実装は必要なく、ユーザーエクスペリエンス (UX) の受け入れテストに使える程度でかまいません。現在はブロッカーですが、優先度がより高い issue がほかにあります。

どちらも優先度が下がりがちなタスクの例であり、Copilot クラウドエージェントに適しています。クラウドエージェントに非同期で作業を任せれば、開発者はほかのタスクを進め、後から戻って Copilot の成果をレビューできます。

## GitHub Copilot クラウドエージェントの紹介

[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)は、人間の開発者と同様にバックグラウンドでタスクを実行できます。人間の開発者に依頼する場合と同じく、[GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、複数の方法で作業を依頼できます。割り当てると、Copilot は進捗を追跡するドラフトプルリクエストを作成し、環境を準備してタスクに取り組みます。作業の途中でも完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備が整うと、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法のような力はありません。考える作業を省けるわけではありません。一見簡単な操作でも、詳しく見ると複雑な場合があります。[Copilot クラウドエージェントに依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。人工知能 (AI) ペアプログラマーとして扱い、段階的に作業し、学び、試しながら調整します。生成 AI を使っても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と、それが Copilot の作業を導く仕組みを確認しました。すでにカスタム指示の演習を完了している場合は、`.github/copilot-instructions.md` の効果を確認しています。まだの場合は、ここで一度確認しましょう。Copilot クラウドエージェントに作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後でどのような影響があるかを見てください。

リポジトリの GitHub Web ユーザーインターフェイス (UI) または起動済みの codespace で、次のファイルを開きます。

1. `.github/copilot-instructions.md` を開き、**Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を確認します。
2. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` frontmatter を確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次の演習で `Code lacks documentation` issue をクラウドエージェントに割り当てるとき、これらの指示による TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーへの影響を確認してください。後の演習でプルリクエスト (PR) をレビューするときにも取り上げます。

## Copilot クラウドエージェントの開発環境を設定する

誰がコードを作成する場合でも、通常は開発環境やセットアップスクリプトを用意し、必要なツールや依存関係を整えます。Copilot にタスクを割り当てる場合も同様です。Copilot はソフトウェアエンジニアと同様に作業します。

クラウドエージェントは GitHub Actions を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定する[セットアップワークフロー][setup-workflow]を作成すると、作業を始める前に必要な開発ツールや依存関係を利用できるように環境をカスタマイズできます。ラボを進めやすくするため、この環境ではあらかじめ設定されています。Copilot が Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一 Astro アプリ用のマイグレーションとシード済み SQLite データベースを利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な最小限のアクセス許可を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` のアクセス許可が必要です。手順でクローンしない場合は、完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストに必要なデータを用意するため、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローファイルと同様ですが、重要な点がいくつかあります。

- `copilot-setup-steps` という単一のジョブが含まれています。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーにより、リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot が実行するのを待たずに、ワークフローが正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古かったり、ドキュメントが不足していたりします。このような技術的負債は放置されやすく、生産性を落とし、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成を得意としているため、クラウドエージェントに割り当てるタスクとして適しています。バックグラウンドで必要なドキュメントを生成し、後の演習で成果を確認します。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![issue への Copilot 割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` などです。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認できます。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかる場合があります。Copilot は環境を作成し（前述のワークフローを実行）、プロジェクトを分析し、問題に取り組む方法を検討しています。

13. todo リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が見つからない場合は、ウィンドウを更新してください。

16. ライブセッションをスクロールして、Copilot が問題を解決する様子を確認します。コードを調査して状態を理解し、解決策を検討して適切な計画を決め、コードを作成する過程を確認できます。

この作業には数分かかる場合があります。Copilot クラウドエージェントの主な目的の 1 つは、非同期にタスクを実行して開発者がほかの作業に集中できるようにすることです。この機能を活用し、別のタスクも Copilot に割り当ててから、アプリケーションに機能を追加するコードを書き始めましょう。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot クラウドエージェントの利点の 1 つは、作業を分担できることです。デザインチーム向けの関連ゲーム機能は時間がかからないかもしれませんが、その時間をほかのタスクに使うこともできます。クラウドエージェントに割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![issue への Copilot 割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![issue で Copilot が作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot クラウドエージェントはソフトウェアエンジニアと同様に作業するため、常に監視する必要はありません。完了後にレビューできます。それでは、カスタムエージェントの作成と使用に進みましょう。

## まとめと次の手順

このレッスンでは、[GitHub Copilot クラウドエージェント][copilot-agents]を確認しました。クラウドエージェントに issue を割り当てると、非同期で作業できます。技術的負債の解消、新機能の作成、別のフレームワークからのコード移行などを依頼できます。

次の概念を確認しました。

- コード生成環境をカスタマイズする方法を確認しました。
- 操作を安全に実行する方法を確認しました。
- issue の範囲を明確にすることの重要性を確認しました。
- issue を Copilot に割り当てました。

クラウドエージェントがバックグラウンドで作業している間に、カスタムエージェントの作成と使用に進みます。[Copilot クラウドエージェントではモデルコンテキストプロトコル (MCP) サーバーも使用でき][cloud-agent-mcp]、前の演習で確認したカスタム指示も利用できます。

## リソース

- [Copilot クラウドエージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
