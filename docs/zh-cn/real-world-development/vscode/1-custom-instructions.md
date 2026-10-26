---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能够获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中重要的一种是[说明文件][instruction-files]，它们可为代码生成提供指导。

本练习将：

- 了解如何通过存储库自定义说明和按路径限定范围的说明文件，向 Copilot 提供项目专属上下文、编码准则和文档标准；
- 在当前说明生效时，生成用于筛选数据的第一个数据切片（出版社辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加一项新的存储库级标准；
- 再次运行相同的提示，观察生成的代码如何采用新标准；
- 将说明更新和筛选数据切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全符合你设置的标准。Copilot 的结果具有非确定性。本练习要观察的是更新说明后行为的*趋势变化*，而不是让输出与示例逐字相同。

## 说明文件

### 场景

Tailspin Toys 和其他优秀的开发团队一样，有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- 用户界面（UI）应采用深色模式，并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

借助说明文件，可以确保 Copilot 获取正确的信息，并按照上述实践完成任务。

### 自定义说明

自定义说明可以向 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。这项功能可以引导 Copilot 提供更贴合需求的建议和代码片段。你可以指定偏好的编码约定、库，甚至喜欢的注释类型。说明既可以适用于整个存储库，也可以针对特定文件类型或任务。

说明文件有两种：

- `.github/copilot-instructions.md` 是一个说明文件，会随该存储库的**每个**请求发送给 Copilot。此文件应包含项目级信息，即大多数 Copilot Chat 或命令行界面（CLI）请求都适用的上下文，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 文件可针对特定任务或文件类型提供指导。例如，可以为 TypeScript 或 Astro 文件，以及创建 UI 组件或新单元测试等任务编写说明。

> [!NOTE]
> 在集成开发环境（IDE）中，说明文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云端代理在生成代码时，都会使用存储库级说明文件以及带有 `applyTo` front matter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理说明文件的最佳实践

完整探讨如何创建说明文件超出了本研讨会的范围。不过，示例项目展示了有代表性的做法，主要包括：

- 在 `copilot-instructions.md` 中集中放置项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（如单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，确保指导清晰，并提供代码应有和不应有的示例。

创建说明文件没有唯一的方法，使用 AI 也是如此。通过实践，你会找到适合自己项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的说明文件。查看此项目中的说明文件时，你可能会发现它们覆盖了许多任务类型，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成说明文件。不同使用界面的操作方式有所不同（例如 VS Code 中的**配置聊天 → 生成代理说明**，或 Copilot CLI 中的 `/init`）；本环境的相关课程会在适用时介绍具体操作。
>
> 如果需要模板或起点，可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量说明文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览此项目中的自定义说明文件

花些时间阅读此存储库附带的说明文件：其中有一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。请在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简述以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。留意针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置了一个相对于存储库根目录的 glob，用于确定说明适用的文件。此处会匹配任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）。
5. 留意此项目中有关创建单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成较小且可重用的文件，并为 Copilot 提供生成代码时可参考的示例。（这些路径相对于说明文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前还没有要求在代码中添加文档。接下来的步骤会添加 TSDoc 文档注释和文件注释标题的规则。

## 为更改创建分支

遵循最佳实践，为这些更改创建一个分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明之前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。不同练习使用的模式和模型可能不同；相关步骤会在需要时说明。

要观察自定义说明的效果，先在当前说明文件生效时发送提示。之后更新说明文件，再次发送相同提示以比较结果。

1. 关闭之前练习中打开的编辑器选项卡，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应该放在哪里。
3. 在 Chat 视图的代理下拉列表中选择 **Agent**，以便 Copilot 应用文件更改。

   ![显示 Chat 视图代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会涉及辅助函数文件和相应测试。
6. 留意生成的辅助函数是一个带类型的函数：它将 `db` 客户端作为第一个参数，并返回带类型的出版社数组。这来自 `.github/instructions/drizzle.instructions.md` 中针对数据层的约定（该文件适用于 `src/lib/*.ts`）。
7. 留意生成的代码**缺少** TSDoc 文档注释和文件级注释标题。

> [!CAUTION]
> Copilot 的结果具有概率性，因此即使没有明确要求，它也可能添加文档注释。如果发生这种情况也没关系；本练习的重点仍然是观察更新说明后行为一致性的提升。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在将编码标准写入该文件，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（通常在第 27 行附近）。留意其中记录了项目编码标准，但还没有关于代码内文档的规则，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释标题和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，说明文件可以位于存储库级别（`.github/copilot-instructions.md`），为项目提供全局指导；也可以使用 `*.instructions.md` 文件针对特定语言、文件类型或任务提供指导。存储库级文件适合放置项目级标准，例如刚添加的文档注释规则。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空对话内容。
2. 再次选中 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意生成的文件现在以类似以下内容的注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的出版社数据访问辅助函数。
    * 提供从数据库检索出版社信息的函数。
    */
   ```

5. 留意生成的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有出版社的 ID 和名称列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为出版社对象数组的 Promise。
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用于实际代码；下一练习将在此基础上继续。

## 提交说明并推送分支

说明文件和其他存储库资源一样，使用相同的源代码管理方式进行管理。现在将更改提交并推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改均已完成：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新，以及 Copilot 为辅助函数基础实现所更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他支持性更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，创建拉取请求（PR），并将其关联到待办事项中的文档标准 issue。也可以手动操作，不过现有 GitHub 工具已连接到项目，因此可以让 Copilot 代为创建 PR。让它查找 issue、创建用于关闭该 issue 的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上执行操作。项目模板已将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用这些工具时，VS Code 可能会提示你登录 GitHub。按照提示完成授权。之后的练习将介绍此连接的工作原理（模型上下文协议（MCP））。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认代理选择器中已选中 **Agent**。
3. 让 Copilot 查找与更新说明文件有关的 issue，并从当前分支创建 PR，说明其中包含说明更新和新的出版社辅助函数基础实现：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 将开始查找 issue 并创建 PR。
5. 如果 Copilot 提示你允许它代表你执行 GitHub 操作，请检查命令，并在适当时选择 **Allow**。
6. 创建 PR 后，让 Copilot 合并 PR 并将本地分支切回 `main`，提示如下：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 提示你允许它代表你执行 GitHub 操作和运行 shell 命令，请检查命令并选择 **Allow**。

现在，你已经借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

你了解了 Copilot 如何从项目说明文件获取上下文，并在 VS Code 中使用 Copilot Chat 完成了以下任务：

- 发送代码生成提示，观察 Copilot 根据*现有*说明生成的内容；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准；
- 再次发送相同提示，观察生成的代码如何采用新标准；
- 将说明更新和辅助函数基础实现提交到 `main`，以便下一练习在此基础上继续。

接下来，在[代理模式][next-lesson]中应用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更优质 Copilot 自定义说明的 5 条建议][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
