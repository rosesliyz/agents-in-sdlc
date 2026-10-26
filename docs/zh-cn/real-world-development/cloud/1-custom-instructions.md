---
title: "练习 1 - 自定义说明（云端代理）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：Copilot 云端代理 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能够获取这些上下文。[说明文件][instruction-files]可以提供这些指导，让 Copilot 不仅了解要做什么，也了解应该如何完成。

本练习将：

- 了解如何通过存储库自定义说明和按路径限定范围的说明文件，向 Copilot 提供项目专属上下文、编码准则和文档标准；
- 在 `.github/copilot-instructions.md` 中添加一项新的存储库级标准。

> [!NOTE]
> 与 VS Code 和命令行界面（CLI）环境不同，此处不会运行前后对比提示。Copilot 云端代理会在 GitHub issue 上异步工作，因此很难实时并排展示效果。稍后审查此环境中云端代理创建的拉取请求时，你会看到说明文件产生的影响。

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

- `.github/copilot-instructions.md` 是一个说明文件，会随该存储库的**每个**请求发送给 Copilot。此文件应包含项目级信息，即大多数 Copilot Chat 或 CLI 请求都适用的上下文，例如所用技术栈、项目概述、最佳实践和全局指导。
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

## 添加新的存储库标准

接下来是本练习中唯一需要编辑的内容：添加一项项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释标题。之后在本环境中分配 issue 时，云端代理会采用这项规则。

编辑前，先创建一个分支（云端代理会从 issue 目标分支读取说明；无论如何，先在单独的分支上提交更改会更整洁）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并说明更新

云端代理会读取 issue 目标分支中的说明文件。下一练习将 issue 分配给 Copilot 时，Copilot 会从 `main` 创建分支，因此需要先将说明更新合并到 `main`，云端代理才能使用它。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。下一练习中处理分配给它的 issue 时，云端代理就会读取这些说明。

> [!TIP]
> 如果想在本研讨会中直接使用 `main`，可以跳过分支步骤，直接提交到 `main`。这里创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何从项目说明文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了一项新的存储库级标准。之后在此环境中审查云端代理创建的拉取请求时，可以看到这项标准的实际效果。

接下来，将[第一个 issue 分配给 Copilot 云端代理][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更优质 Copilot 自定义说明的 5 条建议][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
