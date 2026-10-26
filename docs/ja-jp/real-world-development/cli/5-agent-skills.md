---
title: "レッスン 5 - quality-checks skill をカスタマイズして使う"
description: "既存の quality-checks skill を確認し、レポート形式をカスタマイズして、フィルタリングの検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことだけが開発ではありません。コードが動作することを手動で検証し、指示ファイルを使って標準に従うようにしました。では、テストや lint、継続的インテグレーション (CI) の他の部分はどうでしょうか。

このような作業には、**エージェント スキル**が適しています。スキルを使うと、Copilot はこのような操作を正しく実行する方法を理解できます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` skill を確認します。
- 結果の形式をカスタマイズします。
- skill を再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニット テスト、lint、型チェックに `quality-checks` skill を使っています。チームは結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルは、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめたもので、エージェントが必要に応じて読み込みます。基本的には、スキル名のフォルダーに Markdown ファイル `SKILL.md` を置いたものです。Markdown には、スキルの名前と説明を含む frontmatter、スキルの概要、使用するタイミングのガイダンスが記載されています。フォルダーには、スクリプトなどのリソースを含むサブフォルダーも追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` skill はプロジェクトにある既存のコマンドを使うため、`SKILL.md` だけで構成されています。

スキルは、チームで共有して再利用できるプロジェクトの `.github/skills` フォルダー、またはユーザーのスキル フォルダー `~/.copilot/skills` に配置できます。

## skill を確認する

Tailspin Toys のチームがユニット テスト、lint、型チェックの実行用に作成した `quality-checks` skill を確認しましょう。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイルの冒頭にある `name` と `description` を読みます。説明は、Copilot がいつ skill を使うかを判断するのに役立ちます。
3. 指示を読み、テストと lint の実行を Copilot にどのように案内するかを確認します。
4. skill に **Results output formatting** セクションがまだないことを確認します。

## 変更前に skill を実行する

スキルは Copilot CLI から直接呼び出すことも、自然言語で呼び出すこともできます。skill の 3 つのチェックを実行するよう Copilot に依頼しましょう。

1. Interactive モードのフィルタリング会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールが実際に報告した内容がわかるレポートに改善します。そのレポートを作成するよう skill を更新しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. 次のセクションをファイルの末尾に追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新した skill を実行する

変更を加えたので、動作を確認しましょう。Copilot CLI は会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後のレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使いました。このレッスンでは、次のことを行いました。

- ユニット テスト、lint、型チェック用の `quality-checks` skill を確認しました。
- 結果の形式をカスタマイズしました。
- skill を再読み込みして実行しました。

この変更は、機能のプルリクエスト (PR) にフィルタリング作業とともに含まれます。次は、[Playwright MCP サーバーを使って][next-lesson] Copilot がサイトを直接操作できるようにします。

## その他の skill の例

コミュニティによる次の例は参考資料であり、追加のタスクではありません。

- [Agent Skills の仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート スクリプトのセット: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
