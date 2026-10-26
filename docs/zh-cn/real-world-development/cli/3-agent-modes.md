---
title: "第 3 课 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确定实施方案，使用 Autopilot 根据 issue 构建筛选功能，并使用 Interactive 检查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一项小功能。较大的改动需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的工作流程，帮助我们以正确的方式构建合适的功能。本课是系列课程的第一课，你将按照常见的代理驱动开发流程，先根据 issue 创建新功能，再确认代码有效且功能符合预期，最后将其合并到项目中。

> [!NOTE]
> 在整个功能工作流中，你将使用同一段对话和同一个分支。通常，不同类型的文件会使用不同分支或 PR；为了让你专注于核心概念，我们会简化流程。

在本课中，你将：

- 从 GitHub issue 开始新的 Copilot 对话。
- 在 Plan 模式下确定需求。
- 使用 Autopilot 模式实现新功能。
- 检查代码。
- 在转发的浏览器中手动验证功能。

继续开发此功能时，你将更新仓库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理，并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商筛选游戏。待办 issue 描述了这项功能，但在编写代码前，还需要就类别组合等细节达成一致。你将使用 Plan 模式确定这些细节，然后授权 Autopilot 在明确范围内实施。

## 背景

将人工智能 (AI) 编码代理引入开发流程并不会改变基本原则。事实上，这些原则变得更加重要。大多数开发者遵循类似的流程：

1. 从描述待办事项的已记录 issue 开始。
2. 制定实现计划。
3. 编写并检查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这一标准流程，可以确保 AI 生成的代码符合要求，并经过与手写代码相同的审核流程。

## 对话模式

**对话模式**用于控制代理的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可切换模式：

- **Interactive**：你和代理协同工作。代理提出更改建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，并且无法编辑项目文件。
- **Autopilot**：代理自主工作，编写代码、运行测试并不断迭代，直到认为任务已完成。

先使用 Plan 模式，检查计划后再使用 Autopilot 实施。

## 从 issue 开始

开始筛选功能之前，请返回 Codespace，确认仓库和终端已准备就绪。

1. 返回 Codespace。如果 Codespace 已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选工作创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用方向键突出显示标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中，并返回 **Session** 选项卡。

请注意，提示现在以 `#7`（或类似编号）开头。使用 `#` 可以将 GitHub issue 或拉取请求 (PR) 加入上下文。

## 规划筛选功能

规划可帮助你在交由 Copilot 实现功能或执行任务前确定方案。对于复杂任务，花些时间规划总是值得的。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的 issue 引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 将开始制定计划。它会先探索项目，然后确定合适的方案。

3. 过程中，Copilot 可能会询问筛选功能应如何工作。根据你的偏好回答即可，没有错误答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动检查计划。
6. 如果计划中的任何内容不符合你的决定，请让 Copilot 修改。

## 批准 Autopilot

计划编写并检查完毕后，就可以开始实施了。使用 Autopilot 让 Copilot 执行工作。

Autopilot 会让 Copilot 持续迭代，直到它认为问题已解决。

1. 选择 **Accept plan and build on autopilot (recommended)**，或已安装版本中名称相近的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何根据已确定的计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后，实施可能会立即开始，因此请先检查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 检查并验证实现

代码生成后，合并前仍需像检查其他代码一样进行检查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 将结果与 issue 以及规划时作出的决定进行比较。
4. 检查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 检查项目运行的各项检查结果，并让 Copilot 修复任何失败。

## 试用新功能

代码看起来不错，但它能运行吗？像之前一样启动应用，并通过 Codespaces 转发的端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及规划时商定的组合。
4. 确认重置和无结果时的行为符合 issue 和你的决定。
5. 返回 Codespace，并让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并检查了功能。在本课中，你：

- 从 GitHub issue 开始了新的 Copilot 对话。
- 在 Plan 模式下确定了需求。
- 使用 Autopilot 模式实现了新功能。
- 检查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，我们将[使用自定义说明][next-lesson]进一步了解代码生成方式，确保代码遵循文档中记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
