---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーからフィルタリング機能を調べます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

すでに説明したように、コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が追加の自動化機能を利用できるようにする必要もあります。そのために MCP サーバーを使います。MCP サーバーを使うと、Copilot は CLI に組み込まれた機能を超えて、より多くのツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを理解します。
- Playwright MCP サーバーがまだ利用できない場合は追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を調べます。

## シナリオ

ユニット テストやエンドツーエンド テストは重要ですが、ユーザー インターフェイス (UI) の変更を検証するには実際に UI を操作する必要があります。Copilot が開発中の Web サイトをユーザーと同じように使えるようにし、変更をさらに自動化して、想定どおりに動くという確信を高めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、人工知能 (AI) エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムに通信できます。これにより、最新情報にアクセスし、ユーザーに代わって操作を実行できます。

これらのツールやリソースには、MCP サーバーを介してアクセスします。MCP サーバーは AI エージェントと外部ツールやサービスの間をつなぎます。それぞれの MCP サーバーは、エージェントが利用できる異なるツールやリソースのセットを表します。

既存の MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理するアプリケーション プログラミング インターフェイス (API) へのアクセスを提供します。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使用したブラウザー自動化機能を提供します。

利用できる MCP サーバーは他にもたくさんあります。GitHub は、エコシステムでの発見と貢献を促進するための [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーもプロジェクトの他の依存関係と同じように扱ってください。使用する前に、ソース コードを確認し、発行元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーをまだ利用できない場合は追加し、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Playwright MCP が Copilot CLI ですでに構成されている場合は、セットアップ手順をスキップして次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加のダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類が **STDIO** であることを確定し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って機能を調べるよう Copilot に依頼する

これまでは、機能が想定どおり動作することを手動で確認しました。次は Copilot で利用できる Playwright MCP サーバーを使って同じことを行います。

1. 次のプロンプトを使って、Playwright MCP サーバーで機能を検証するよう Copilot に伝えます。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot は自分で MCP サーバーの使用方法を判断するため、明示的に指示する必要はありません。ただし、使用するツールを伝えると、より一貫した結果が得られ、トークンも節約できます。

2. Copilot がブラウザーで機能を確認するために実行する手順を一覧表示する様子を確認します。
3. レポートを読んで、すべてが想定どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright を使って Web サイトを操作し、サーバーを停止してレポートを作成します。

## まとめと次のステップ

おめでとうございます。Copilot CLI から Playwright MCP サーバーを使って、実際のブラウザーで機能を確認しました。このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを学びました。
- Playwright MCP サーバーがまだ利用できない場合に追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を調べました。

次は、skill とブラウザー ツールをまとめて専門的な役割として使う[QA カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
