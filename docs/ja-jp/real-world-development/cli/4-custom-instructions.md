---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成系の人工知能 (AI) を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合は、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]には、作成するコードの内容だけでなく、その構成方法も記述できます。フィルタリング機能を作成したので、Copilot が使用した指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス スコープ付き指示ファイルがエージェントに届く方法を確認します。
- コーディング標準を確実に適用するため、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

優れた開発チームである Tailspin Toys には、開発プラクティスのガイドラインと要件があります。たとえば、次のとおりです。

- コメントはコードを繰り返すのではなく、意図や明白でない判断の理由を説明する必要があります。
- `db/` と `src/lib/` のエクスポート関数には、該当する場合は注入可能な `db` 引数を含め、目的、パラメーター、戻り値を TSDoc/JSDoc で記述する必要があります。
- 再利用可能な Astro コンポーネントでは `Props` コントラクトを文書化し、関連コードが変更されたときはコメントを最新の状態に保つ必要があります。
- 既存のフォーマットと lint のガイダンスを維持する必要があります。

指示ファイルを使い、Copilot がこれらのプラクティスに沿ってタスクを実行するために必要な情報を提供します。

## 指示ファイル

カスタム指示は Copilot にコンテキストや設定を提供し、コーディング スタイルや要件を理解しやすくします。好みのコーディング規則、ライブラリ、コメントの記述方法などを指定することで、より適切な提案やコード スニペットを得られます。リポジトリ全体、または特定のファイルやタスクに対する指示を作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリのすべての要求で Copilot に送信される単一の指示ファイルです。多くの要求に関連するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイルの種類、タスクに関するガイドラインを提供します。

> [!NOTE]
> 他の形式の指示への対応状況は環境によって異なります。特定の形式を前提とする前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すでにいくつかの指示ファイルが含まれています。変更を加えて効果を確認する前に、既存のファイルを確認しましょう。

1. Codespace に戻ります。
2. ターミナルではなく Codespaces エディターで `.github/copilot-instructions.md` を開きます。
3. ファイルを確認し、プロジェクトの簡単な説明とコーディング ガイダンスを読みます。この指示は、このリポジトリで Copilot を使うすべての操作に適用されます。
4. `.github/instructions` フォルダーを開いてファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、指示の適用対象となるファイルを決める glob パターンを設定していることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存のプロジェクト ファイルへのリンクを確認します。大きな指示を再利用可能な小さなセットに分割し、参考にするファイルを示すことができます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、指針が 1 つ不足しています。中核となる `copilot-instructions.md` を変更し、新しい TypeScript に TSDoc コメントを追加するようにします。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新した指示を適用する

Copilot CLI は会話を開始するときにリポジトリの指示を読み込みます。指示を保存した後、フィルタリングの会話を終了して再開します。そうすると、機能のコンテキストを失わずに Copilot が新しい指針を読み込みます。

1. フィルタリングの会話が開いたままの場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチをチェックアウトした状態にして、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新した指針を適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しく生成された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの指示とパス スコープ付き指示ファイルがエージェントに届く方法を確認しました。
- コーディング標準を確実に適用するため、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` skill を[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
