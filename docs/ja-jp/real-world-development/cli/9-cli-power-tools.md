---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、クラウドへの任意の委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

優れた CLI ツールである GitHub Copilot CLI には、操作に使えるスラッシュ コマンドが多数用意されています。これらのコマンドを使うと、高度な機能、内部情報、追加の構成オプションを利用できます。すでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。他の便利なコマンドも確認しましょう。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使い、利用できるモデルを調べます。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、クラウド エージェントへの委任に使う任意のコマンドを確認します。

## Copilot CLI のコンテキストを調べる

大規模または複雑なタスクに取り組んでいると、モデルの最大コンテキスト ウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュ コマンドを使って自分で確認または圧縮することもできます。

1. Codespace に戻り、Copilot CLI がまだ起動していない場合はリポジトリのルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域に分割されたコンテキストを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話がすでに短い場合、大きな変化がないことがあります。

> [!NOTE]
> Copilot CLI はコンテキスト ウィンドウがいっぱいになると自動的に圧縮します。タイミングを自分で決める場合は `/compact` を使います。無関係なタスクに切り替えて新しい会話を開始する場合は、`/clear` または `/new` を使います。

## モデルを選択する

モデルごとに得意なことが異なり、開発者によって好みも異なります。Copilot CLI では利用可能なモデルを一覧表示し、使用するモデルを選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、誰もが人工知能 (AI) ツールをより上手に使えるようになります。`/share` コマンドを使うと、セッションを Markdown またはハイパーテキスト マークアップ言語 (HTML) ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown ファイルにエクスポートする `/share file` など、目的に合う出力先を選びます。
3. エクスポートした内容を送信または公開する前に確認します。セッションのエクスポートにはプロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## オプション: 作業を拡張または委任する

ワークショップの中心的な作業は完了です。Copilot CLI には、より大きなタスクに使えるコマンドもあります。

- `/fleet` は独立したサブタスクを複数のサブエージェントに分割し、並列に実行できます。
- `/worktree` は別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は Copilot cloud agent にタスクを送信できます。cloud agent は非同期で動作し、プルリクエストを開く場合があります。

これらのコマンドを使うと、追加の worktree が作成されたり、リモートで作業が行われたりするため、使用は任意です。試す前に、新しい範囲の明確なタスクを開始し、通常のワークフローで結果を確認してください。エージェントによる非同期作業を詳しく知りたい場合は、[クラウド エージェントのワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、設定を変更し、セッションを共有し、内部の動作を確認できます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使い、利用できるモデルを調べました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、クラウド エージェントへの委任に使う任意のコマンドを確認しました。

Copilot CLI には他にも多くのスラッシュ コマンドがあります。最後に、[学んだことを振り返り][next-lesson]、次に学ぶ内容を確認しましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
