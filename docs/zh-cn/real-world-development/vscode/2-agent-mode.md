---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发人员，你通常需要找到所有相关文件、进行更改、运行测试、调试、找出遗漏的文件，再进行更新……如此反复。

这正是 Copilot 代理模式的用武之地。

Copilot 代理模式旨在集成开发环境（IDE）中更自主地工作。它的行为类似开发人员：先探索现有项目结构，再进行必要更新、运行测试等任务，并自动修复发现的问题。现在了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云端代理名称相似，但面向两种不同的使用场景。代理模式在 IDE 中执行任务，便于快速反馈和交互。云端代理则像团队成员一样异步工作，通过 issue 和拉取请求与你协作。

本练习将介绍：

- Copilot 代理模式如何探索项目、确定相关文件并协调多处更改；
- GitHub Copilot 代理模式如何跨用户界面（UI）和数据层实现新功能；
- 在将代码合并到代码库之前，如何审查 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望允许用户按类别和出版社筛选。上一练习中已经添加了出版社辅助函数，现在将使用 Copilot 代理模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先探索 Tailspin Toys 网站，了解它当前的功能。

该网站是一个带有开发者主题的桌游众筹平台。用户可以浏览游戏列表和游戏详情。整个网站是一个 Astro 应用，在构建时将页面渲染为静态超文本标记语言（HTML）。页面会在 frontmatter 中通过 Drizzle 对象关系映射（ORM）直接查询本地 SQLite 数据库；没有单独的后端应用程序编程接口（API）或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过使用标准文档对象模型（DOM）API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器，让网站更容易运行。请按以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，你应该已切回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到用于筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，应该会显示包含 URL 的横幅，类似如下内容：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现对话框，提示你打开 `http://localhost:4321` 浏览器窗口，请选择 **x** 关闭它。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，从 Codespace 终端选择 localhost URL 链接会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向当前托管 Web 服务器的 codespace 的私有隧道！

### 浏览网站

网站运行后，可以浏览其功能。主要功能包括：

- **主页**：显示桌游列表及其标题、图片和说明。
- **游戏详情页**：选择某款游戏后，会打开详情页，显示游戏标题、说明、出版社和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。不同练习使用的模式和模型可能不同；相关步骤会在需要时说明。

网站的初始实现可以正常使用，但我们还想添加新功能。先查看待办事项。根据模板设置项目时，系统已自动创建一组 issue；现在让 GitHub Copilot 显示这些事项。

1. 在 Chat 视图的代理下拉列表中选择 **Agent**。**Agent** 代理可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示 Chat 视图代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会的作者并不表示偏好某个模型。编写本研讨会时使用的是 Claude Sonnet 4.5，因此说明中列出了该模型，希望这样能让代码建议相对一致，确保使用体验良好。不过，由于大型语言模型（LLM）的结果具有概率性，你收到的建议可能与研讨会中的示例不同。这完全正常，也是预期情况。

> [!NOTE]
> 由于大型语言模型的结果具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示，询问待办 issue：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有 issue 的命令。
5. 查看生成的 issue 列表。

留意 Copilot 还根据它认为对用户最有用的程度为事项排定了优先级。

## 查看说明文件

让代理开始生成代码前，先查看用于向 Copilot 提供工作上下文的说明文件。接下来将使用 [UI 说明文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中包含如何为网站添加功能的背景信息。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意文件中关于如何添加功能的整体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍原则；
   - 指向多种文件类型专用说明的链接，包括：
     - Astro；
     - Tailwind CSS（层叠样式表）；
     - Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置，因此可以重复使用内容、将复杂说明拆分成更易管理的小块，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用程序进行三项更新：

- 在数据层（`src/lib/`）中添加或完善剩余的筛选逻辑；
- 添加或更新筛选行为的测试；
- 更新游戏列表页，添加筛选 UI。

此外，合并到代码库之前还需要运行并通过测试。Copilot 代理模式可以完成这些任务！现在添加此功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，再选择 **ui** 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉列表中仍选中 **Agent**。

   ![显示 Chat 视图代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选中模型 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办事项中的相关 issue 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目，查找与目标功能有关的文件。它应该会找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 按钮，因此不需要在工作进行时选择它们。

7. 根据 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中提示用户确认运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至会看到测试失败。这没有问题！Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且没有检测到错误。

   ![完整的 Copilot 代理模式 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他人工智能（AI）工具生成的代码。

10. 返回正在运行网站的浏览器，探索新功能！
11. 确认一切正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能。你了解了：

- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能；
- Copilot 代理模式如何探索项目、确定相关文件并协调多处更改；
- 如何在将代码合并到代码库之前审查 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索练习 — 实现分页

随着游戏列表不断增长，需要启用分页。运用本练习学到的技能，提示 Copilot 更新网站并实现分页。设计实现时可以考虑：

- 遵循现有最佳实践，包括使用现有说明文件；
- 考虑分页方式，例如让用户选择每页大小，还是将其设为固定值；
- 编写提示时，提供 Copilot 按照预期实现功能所需的指导；
- 可能需要与 GitHub Copilot 迭代，多次请求更改并提供上下文。这是与 Copilot 协作的正常流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：各自的功能和适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
