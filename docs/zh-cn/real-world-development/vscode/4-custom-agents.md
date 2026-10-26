---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是专为开发工作流中特定任务或领域打造的人工智能（AI）助手。你可以在存储库的 `.github/agents` 文件夹中通过 Markdown 文件定义代理，为 Copilot 提供专门的说明、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理成可重复使用的代理，例如确保符合《Web 内容无障碍指南》（WCAG）要求的无障碍代理、遵循安全编码实践的安全代理，或维持一致测试模式的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局路径 `~/.copilot/agents` 中定义。每个文件都包含 YAML front matter，至少要有 `name` 和 `description` 字段，后面则是定义代理行为、专业知识和说明的 Markdown 提示。

### 自定义代理与代理技能的比较

自定义代理和[代理技能][agent-skills-concept]有一些相似之处。两者主要都通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理**负责执行工作，**技能**则提供工具。

自定义代理有自己的上下文窗口，并且可以在工作时编排技能（甚至其他代理）。本实验中的无障碍自定义代理会根据无障碍指南审查并更新网站；在此过程中，它可以调用拉取请求工作流技能，或用于运行和管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一的“正确”方法。与 AI 的其他用法一样，应在自己的环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，由于文本与背景颜色对比度不足，有些用户难以阅读当前的深色主题。为解决这一无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

无障碍功能至关重要，因此需要尽快实现。你将使用自定义代理生成此功能。

本练习将：

- 查看现有的无障碍自定义代理；
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 查看无障碍自定义代理

我们已为你创建了无障碍自定义代理。现在查看其内容，了解它将如何指导 Copilot。

返回 codespace，然后打开终端，从 `main` 创建一个用于无障碍功能的新分支（让练习 3 中的筛选拉取请求（PR）保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意其中包含 `name` 和 `description` 字段的 YAML front matter。

> [!CAUTION]
> `name` 和 `description` front matter 是自定义代理的必需项。

3. 接着浏览以下部分，了解其中的内容：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - 超文本标记语言（HTML）、层叠样式表（CSS）和 JavaScript 代码示例；
   - 常见问题和错误列表。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉列表中显示 `.github/agents` 中定义的所有自定义代理。你可以选择某个自定义代理，让聊天会话使用该代理的说明和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。不同练习使用的模式和模型可能不同；相关步骤会在需要时说明。

1. 如果 Chat 视图的代理下拉列表中尚未选中 **Agent**，请先选择它。

   ![显示 Chat 视图代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉列表（其中显示当前代理，默认值为 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 将开始工作：探索代码库、提出更改并编辑项目文件。每项编辑都会在聊天中显示文件路径和差异，供你审查。
6. 随着编辑逐步完成，**Files changed** 指示器会更新，显示代理修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 正在对存储库进行实际更改，工作期间会编辑 Astro 组件、CSS 和相关测试等现有文件。

下一练习将审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，也就是专为特定任务和领域打造的 AI 助手。借助自定义代理，可以将团队的专业知识和标准整理成可重复使用的代理，更有效地指导 Copilot 完成特定类型的工作。

本课探索了以下概念：

- 自定义代理的定义方式；
- 在 Copilot Chat 代理模式中使用自定义代理。

接下来，[监控并引导代理的工作][next-lesson]：查看实时更改，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义代理][custom-agents]
- [在集成开发环境（IDE）中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright 模型上下文协议（MCP）服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
