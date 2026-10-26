---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云端代理分别处理了三项任务，目标是改善用户体验并添加功能。云端代理虽然可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云端代理的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个任务控制中心，你可以查看所有拥有未关闭拉取请求（PR）的代理任务、了解它们执行的操作，甚至引导正在进行的会话。

本课将：

- 浏览代理页面，监控云端代理任务；
- 引导正在进行的会话，要求添加额外功能。

## 场景

将高对比度模式任务分配给代理后，团队意识到也可以趁此添加浅色模式。网站样式和切换功能本来就在更新，因此一并添加浅色模式很合适。你将引导代理，确保它在添加高对比度模式的同时也实现浅色模式。

## 查看 Copilot 云端代理任务

查看当前分配给 Copilot 云端代理的所有任务状态。

1. 转到 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 留意主窗格和左侧窗格中的任务列表。你应该能看到分配给 Copilot 的任务，包括：
    - 更新代码文档；
    - 为游戏修改功能生成应用程序编程接口（API）；
    - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码；
    - 创建 Copilot 工作环境；
    - 设置模型上下文协议（MCP）服务器；
    - 执行完成已分配任务所需的各种操作。

> [!NOTE]
> 具体列出的步骤会因 Copilot 工作进度和采用的方法而异。

4. 另外留意右侧显示的 PR 窗格。你可以在此查看 PR 和已更改的文件，以便进一步监控。

## 引导云端代理

查看活动任务后，现在要求 Copilot 在处理高对比度模式时也添加浅色模式切换开关。

1. 选择与添加高对比度模式有关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示已完成设置并开始工作。出现类似以下消息时，就说明已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面中的云端代理任务截图，其中突出显示了 Steer active session while copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会开始处理更新后的任务！发送消息时，Copilot 正在执行的步骤完成后，就会将新请求纳入工作流程。

这也需要几分钟。你可以趁此机会暂停一下，回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 代理页面，这是监控和引导 GitHub Copilot 云端代理任务的中心位置。通过此任务控制中心，你可以跟踪所有活动和已完成的任务、查看正在执行的工作，甚至重定向正在进行的任务以调整范围或提供额外指导。

本课探索了以下概念：

- 浏览任务控制中心和代理页面，监控云端代理任务；
- 重定向正在进行的会话，要求添加额外功能。

Copilot 正在完成无障碍功能。接下来，[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
