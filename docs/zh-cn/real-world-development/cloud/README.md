---
slug: real-world-development/cloud
title: "GitHub Copilot 云端代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[**GitHub Copilot 云端代理**](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)可以让 GitHub Copilot 在云端异步工作。你可以在 GitHub 上分配任务，云端代理会在后台处理：探索存储库、进行更改并创建拉取请求，而你可以继续处理其他事情。

在这些练习中，你将添加云端代理要遵循的自定义说明，然后分配 GitHub issue 让它实现任务。你将查看并使用自定义代理来指导其工作方式，通过代理面板监控和引导会话，最后审查拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 准备工作][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加云端代理要遵循的自定义说明 |
| [2. 云端代理][ex2] | 异步代理 | 将 issue 分配给 Copilot 云端代理 |
| [3. 自定义代理][ex3] | 专业代理 | 查看并使用自定义代理 |
| [4. 管理代理][ex4] | 监控 | 监控并引导代理会话 |
| [5. 迭代改进][ex5] | 审查 | 审查拉取请求（PR）、迭代 Copilot 的工作并选择后续步骤 |

## 准备工作

参加本研讨会前，请确保具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 已为你的帐户或组织启用 Copilot 云端代理

> [!TIP]
> 没有付费计划？通过验证的学生可以通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本研讨会使用的代理、模型上下文协议（MCP）、代码审查和 Copilot 命令行界面（CLI）功能，因此可以使用此计划完成所有环境的练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot 云端代理适用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise**，不适用于 Copilot Free。在 Copilot Business 或 Enterprise 中，管理员必须为组织启用该功能。

> [!NOTE]
> 本环境不介绍 MCP。要了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 环境](../cli/)或 [VS Code 环境](../vscode/)。

## 开始

**[从练习 0：准备工作开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
