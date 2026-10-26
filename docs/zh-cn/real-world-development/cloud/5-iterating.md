---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作成果

在本实验中，你与 GitHub Copilot 一起处理了多项改善用户体验和添加功能的任务：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包含高对比度和浅色模式切换开关的无障碍功能。现在查看代码更改，并在必要时向 Copilot 提供反馈以改进结果。

### 场景

如前所述，引入生成式人工智能（AI）并不会改变软件设计和开发运维（DevOps）的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。因此，在向团队其他成员开放审查之前，先查看 GitHub Copilot 为文档、相关游戏功能和无障碍功能提出的更改。

## 安全性与 GitHub Copilot 云端代理

由于 Copilot 云端代理会在无人监督的情况下异步执行任务，因此设置了一些安全限制，确保操作安全：

- Copilot 对存储库只有读取权限，对代码仅有其工作分支的写入权限。
- 云端代理在 GitHub Actions 中运行，并会创建独立的临时环境来执行工作。
- 运行任何 GitHub Actions 工作流之前，都需要人工批准。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，模型上下文协议（MCP）服务器也包括在内。

## 审查生成的文档

首先，查看 GitHub Copilot 云端代理创建的第一个拉取请求（PR），其中为代码添加了文档。你将使用 GitHub.com 标准 PR 界面完成此任务。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括调用外部 MCP 服务器。如有需要，可以[自定义或禁用 Copilot 云端代理的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求，标题也可能更具体。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作，这可能需要几分钟。你可以稍作休息，或回顾迄今为止学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包含新添加的 TSDoc 文档注释和其他文档。具体更改可能有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和注释标题。这些要求来自你在练习 2 开始时查看的自定义说明文件。

6. 审查更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到有工作流正在等待批准的指示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会在拉取请求的检查部分中排队。如果一切正常，单个 Astro 应用的项目检查应该会通过。这可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 协作并非单向过程。你可以像提及团队其他成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动新会话来处理请求。由于结果具有非确定性，我们无法规定你应该提出的具体内容。可以要求 Copilot 更新的内容包括：

- 在每个代码文件顶部添加简短说明文件用途的注释标题。
- 在 TypeScript 和 Astro 文件中添加 TSDoc 文档注释。
- 创建一份 README，介绍 Astro 应用结构。

1. 添加评论并提及 **@copilot**，请求更改生成的文档，就像提及其他用户一样。可以使用上述任一建议，也可以提出其他希望在代码库中看到的文档改进。
2. 选择 **View Session**，查看 Copilot 执行更改的过程。留意 Copilot 如何启动新会话来更新内容。
3. 选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

你可以继续迭代，直到满意为止。满意后，可以将 PR 从草稿状态改为就绪状态，然后合并到主分支。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

返回 Copilot 为解决“在游戏详情页显示相关游戏” issue 创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR，标题也可能更具体。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
6. 你应该会看到有工作流正在等待批准的指示。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会在拉取请求的检查部分中排队。如果一切正常，单个 Astro 应用的项目检查应该会通过。这可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到此分支，手动测试相关游戏功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经构建了相关游戏功能！和之前一样，你可以通过迭代向 Copilot 云端代理请求更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或确保添加注释标题和 TSDoc 文档注释（请记住：分配此任务时，你还没有更新自定义说明）。和之前一样，可以在 **Conversation** 选项卡中添加新评论来提出请求，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义代理实现的功能。此 PR 应包含练习 3 中分配的高对比度模式，以及在练习 4 的任务控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR，标题也可能更具体。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 查看实现，特别留意以下内容：
   - 用于切换模式的用户界面（UI）组件；
   - 用于保存用户偏好的本地存储；
   - 高对比度和浅色模式对应的层叠样式表（CSS）或样式更改；
   - 无障碍富互联网应用（ARIA）属性（标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到有工作流正在等待批准的指示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会在拉取请求的检查部分中排队。如果一切正常，单个 Astro 应用的项目检查应该会通过。这可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用程序，并在浏览器中测试高对比度和浅色模式切换开关，确认它们能按预期工作，并且在重新加载页面后仍然保留设置。

留意无障碍自定义代理如何引导 Copilot 遵循无障碍最佳实践来实现这些功能。如果发现无障碍方面的问题或改进之处，可以像处理之前的 PR 一样，在评论中提及 **@copilot** 请求更新。

## 可选练习 — 继续委派任务

如果能把真正的待办任务交给云端代理并转而处理其他工作，就能充分发挥其优势。为养成这种习惯，可以在存储库中创建几个 issue 并分配给 Copilot。以下是一些建议：

- 在游戏详情页中创建支持者兴趣表单。
- 在游戏列表页中实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云端代理环境中的练习。通过这些课程，你：

- **检查了此存储库附带的自定义说明文件**，了解它们对云端代理输出的影响。
- **将 issue 分配给 Copilot 云端代理**，并观察它如何设置环境、制定计划和异步执行任务。
- **创建并使用了无障碍自定义代理**，添加高对比度和浅色模式切换开关。
- **使用 Copilot 代理页面作为任务控制中心**，监控并引导正在进行的无障碍会话。
- **审查并迭代了云端代理创建的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云端代理环境中的练习。如果想继续探索，其他环境可以补充你在此处实践的内容：

- 🖥️ **[VS Code 环境](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[命令行界面（CLI）环境](../../cli/)** — 在终端中通过 Plan 和 Autopilot 模式、代理技能、自定义质量保证（QA）代理、Playwright MCP、Agent Merge 以及可选的 `/delegate` 云端代理桥接功能，交付两项经过审查的更改。

在自己的存储库中，可以尝试以下后续任务：

- 将重构或测试覆盖率 issue 分配给云端代理。
- 为其他领域创建自定义代理。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot 云端代理设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云端代理防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
