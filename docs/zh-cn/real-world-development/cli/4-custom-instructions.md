---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "探索仓库说明，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式人工智能 (AI) 时，上下文至关重要。如果任务需要以特定方式完成，就应该让 Copilot 获取相应的指导。[说明文件][instruction-files]不仅描述你想要*什么*代码，也描述代码应如何组织。现在筛选功能已经完成，你将探索 Copilot 使用的说明、添加文档标准，并将其应用于代码。

在本课中，你将：

- 探索仓库说明和路径范围说明文件如何传递给代理。
- 更新说明文件，确保遵循编码标准。
- 查看说明文件对代码的影响。

## 场景

作为优秀的开发团队，Tailspin Toys 制定了开发实践准则和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值，并在存在可注入的 `db` 参数时说明该参数。
- 可复用的 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持最新。
- 应保留现有格式和 lint 指导。

通过使用说明文件，你可以确保 Copilot 获得必要信息，按照上述实践完成任务。

## 说明文件

自定义说明可以向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。你可以指定编码约定、库以及偏好的注释类型，从而引导 Copilot 提供更合适的建议和代码片段。你可以为整个仓库创建说明，也可以为特定文件类型或任务创建说明。

说明文件分为两种：

- `.github/copilot-instructions.md`：单个说明文件，会随仓库中的每个请求发送给 Copilot。应包含大多数请求都需要的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的文件。

> [!NOTE]
> 对其他说明格式的支持因环境而异。依赖特定格式之前，请查看[自定义说明支持参考][custom-instructions-support]。

## 探索此项目中的自定义说明文件

入门项目已经包含一组说明文件。先探索已有内容，再进行更改并查看效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器中（而不是终端中）打开 `.github/copilot-instructions.md`。
3. 探索该文件，了解项目简介和编码指导。这些说明适用于在此仓库中与 Copilot 进行的每次交互。
4. 打开 `.github/instructions` 文件夹并查看其中的文件。这里包括针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它设置了一个 glob，用于决定说明适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他说明文件和现有项目文件的链接。这样可以将较大的说明集拆分为较小、可复用的文件，并提供参考示例。

## 更新说明文件以符合团队指导

现有文件是不错的起点，但还缺少一项指导。我们将修改核心文件 `copilot-instructions.md`，确保新编写的 TypeScript 包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中，找到 **Code formatting guidance** 部分，位置应在第 35 行附近。
2. 将以下项目添加到列表末尾：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载仓库说明。保存说明后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在仓库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并检查已更改的 TypeScript 文件。查看新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和后续步骤

你探索了 Copilot CLI 如何从说明文件获取上下文，并将新标准应用于功能。具体来说，你：

- 探索了仓库说明和路径范围说明文件如何传递给代理。
- 更新了说明文件，确保遵循编码标准。
- 查看了说明文件对代码的影响。

接下来，你将[自定义并运行可复用的 `quality-checks` 技能][next-lesson]，确保 lint 和测试能够一致地运行。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建有效自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot — 说明文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
