---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローに取り組みました。このワークショップでは、次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを調べ、あらかじめ登録されたフィルタリングの issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの issue から開始し、Plan モードで方針を定義し、Autopilot モードで実装して、Interactive モードで確認しました。
- カスタム指示でエージェントを導き、既存の `quality-checks` skill をカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 要件、カバレッジ、skill の結果、ブラウザーの証拠を評価する QA カスタム エージェントを作成して選択しました。
- フィルタリングの変更全体を確認し、フィルタリングの PR に対して Agent Merge を許可しました。
- コンテキスト、モデル、共有、クラウドへの任意の委任に使うスラッシュ コマンドを確認しました。

## リリースした内容

このワークショップには、更新済みの `main` から作成した個別のブランチで進める 2 つの PR の節目があります。

1. **星評価**: 既存の `starRating` と、評価がない場合の明確な状態をゲームカードに表示します。
2. **フィルタリングと品質ワークフロー**: フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルを作成して、関連するテストを含めます。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を使った Copilot によるブラウザー操作という複数の方法でコードを確認しました。`quality-checks` skill はユニット テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は PR の前に、要件とテスト カバレッジの確認をその結果にまとめました。

追加するテストは実際に不足している部分を補うものにします。QA の実行で新しいテストが不要という判断が正しい場合もあります。マージを許可する前にコードと根拠を確認し、変更後は影響を受ける根拠を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、エージェントの作業を左右します。このワークショップでは、指示を更新し、skill をカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整します。指示は標準を定め、skill は反復可能なタスクを示し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールを接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選択します。実装前に方針を考えるには **Plan**、範囲を限定した変更に関与し続けるには **Interactive**、明確に定義されたタスクには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。作りたいものが*何か*、*なぜ*必要か、*どのように*作りたいかを明確に伝えると、結果が大きく変わります。

## さらに確認する

中心となるワークフローを完了しました。Copilot CLI のその他の機能も確認してみましょう。

- `/review` でコード レビュー エージェントに変更の分析を依頼します。
- `/rubber-duck` で問題について話し合い、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることが上達への近道です。製品コード、趣味のコード、何年も作ろうと思っていた小さなアプリに使ってみましょう。学んだことをチームと共有し、ほかの人の知識も学びます。そして、いつでもドキュメントを確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリのワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認します。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
