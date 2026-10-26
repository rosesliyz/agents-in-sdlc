---
title: "演習 1 - カスタム指示 (クラウドエージェント)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot クラウドエージェント →][next-lesson]

生成系人工知能 (AI) を使うときは、コンテキストが重要です。タスクの進め方に決まりがある場合や、Copilot に背景情報を伝える必要がある場合は、その情報にアクセスできるようにします。[指示ファイル][instruction-files]を使うと、Copilot に作業内容だけでなく、作業方法も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- リポジトリ全体に適用する新しい標準を `.github/copilot-instructions.md` に追加します。

> [!NOTE]
> VS Code や CLI の環境とは異なり、ここではプロンプトの実行前後を比較しません。Copilot クラウドエージェントは GitHub issue に基づいて非同期に動作するため、リアルタイムで並べて影響を示すのが難しいためです。この環境の後半で、クラウドエージェントが作成したプルリクエストを確認するときに、指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- データ層には常に単体テストが必要です。
- ユーザーインターフェイス (UI) はダークモードで、現代的な印象にする必要があります。
- TSDoc のドキュメントコメントを使って、コードにドキュメントを追加する必要があります。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加する必要があります。

指示ファイルを使って Copilot に適切な情報を伝え、これらのプラクティスに沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを得られます。推奨するコーディング規約やライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体に適用する指示や、特定のファイルを対象にした指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発内容の概要、ベストプラクティスなど、ほとんどのチャットや CLI のリクエストに関係するプロジェクトレベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に使う指示ファイルです。TypeScript や Astro などの言語や、UI コンポーネントや単体テストの作成といったタスクに関するガイドラインを記述できます。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat でコードを生成するときに使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot コマンドラインインターフェイス (CLI)、Copilot クラウドエージェントは、リポジトリレベルの指示と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルにも対応しています。[指示ファイルのサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルを作成する方法をすべて説明することは、このワークショップの範囲外です。ここでは、サンプルプロジェクトにある指示ファイルを例として、基本的な考え方を確認します。

- `copilot-instructions.md` には、開発内容やプロジェクト構成、全体的なコーディング標準など、プロジェクトレベルのガイダンスを記述します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ層などのファイル形式や特定のタスクに関する指示を記述します。
- 自然な言葉で、明確なガイダンスとコードの良い例・悪い例を示します。

指示ファイルの作り方や AI の使い方に決まった方法はありません。試行錯誤しながら、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用のファイルがあります。
>
> Copilot に指示ファイルの生成を手伝ってもらうこともできます。操作方法は利用する環境によって異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`）。該当するレッスンで説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェントなどのリソースを集めたリポジトリ [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。基本となる `copilot-instructions.md` と、さまざまなタスク用の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明に加えて、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot とのやり取り全般に適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、リポジトリのルートを基準とする glob で、指示の適用対象を指定します。たとえば `**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトで単体テストを作成するときの指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分割し、Copilot がコードを生成するときに参考例を示せます。パスはリポジトリのルートではなく、指示ファイルを基準にしています。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントについてはまだ要件がありません。次の手順で、TSDoc のドキュメントコメントとファイルヘッダーの規則を追加します。

## 新しいリポジトリ標準を追加する

ここで行う編集は 1 つです。コードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーを追加するプロジェクト全体の規則を設定します。後の演習で issue を割り当てるときに、クラウドエージェントがこの規則を参照します。

編集を始める前に、作業ブランチを用意します。クラウドエージェントは issue の対象ブランチにある指示を読み込みますが、編集内容を整理してコミットできるよう、ここではブランチを作成します。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は、Copilot にプロジェクトレベルの情報を伝えるファイルです。コーディング標準を追加して、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コーディング標準が記載されていますが、コード内のドキュメントの規則はまだありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準のすぐ下に次の Markdown 行を追加し、ファイルコメントヘッダーと TSDoc のドキュメントコメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベルの `.github/copilot-instructions.md` に作成して全体に適用するか、特定の言語、ファイル形式、タスク用の `*.instructions.md` に作成できます。今回追加したドキュメントコメントの規則のようなプロジェクト全体の標準は、リポジトリレベルのファイルに記載します。

## 指示の変更をコミット、プッシュ、マージする

クラウドエージェントは issue の対象ブランチにある指示を読み込みます。次の演習で Copilot に割り当てる issue は `main` からブランチを作成するため、指示の変更を `main` に取り込む必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成し、マージします。次の演習でクラウドエージェントが issue に取り組むとき、この指示を読み込みます。

> [!TIP]
> このワークショップでは `main` で直接作業してもかまいません。その場合はブランチを作成せず、`main` に直接コミットできます。ここでブランチを使うのは、実際のプロジェクトでの作業方法に沿うためです。

## まとめと次の手順

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。この環境の後半で、クラウドエージェントが作成するプルリクエストに、この標準が反映されているか確認します。

次に、[Copilot クラウドエージェントに初めて issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
