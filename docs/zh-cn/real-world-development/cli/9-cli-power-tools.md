---
title: "第 9 课 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话以及可选云委托的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀 CLI 工具一样，GitHub Copilot CLI 提供了许多用于交互的斜杠命令。这些命令可以访问高级功能、后台信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在来看看其他实用命令。

在本课中，你将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、worktree 和委托给云代理的可选命令。

## 探索 Copilot CLI 上下文

处理较大或较复杂的任务时，你可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话，你也可以使用斜杠命令检查或手动压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从仓库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前令牌用量，以及上下文在系统说明、工具、消息和空闲空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话已经很短，变化可能不明显。

> [!NOTE]
> 上下文窗口逐渐填满时，Copilot CLI 会自动压缩上下文。想自行选择时机时，可以使用 `/compact`。切换到无关任务并开始新对话时，可以使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有所长，不同开发者也有不同偏好。Copilot CLI 支持列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作并分享经验有助于所有人更好地使用人工智能 (AI) 工具。`/share` 命令可以将会话导出为 Markdown 或超文本标记语言 (HTML) 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看已安装版本中的 `/share` 选项。
2. 如果要共享此会话，请选择合适的目标位置，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送或发布导出内容前先检查内容。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队不打算共享的仓库或对话内容。

## 可选：扩展或委托工作

核心工作坊流程已经完成。Copilot CLI 还提供了适用于较大任务的命令：

- `/fleet` 可以将彼此独立的子任务分配给多个子代理并行执行。
- `/worktree` 可以为另一项任务创建隔离的 Git worktree。
- `/delegate` 可以将任务发送给 Copilot 云代理；它会异步工作，并可能打开拉取请求。

这些命令是可选的，因为它们可能创建其他 worktree 或启动远程工作。尝试之前，先开始一项范围明确的新任务，并按常规工作流检查结果。如果想进一步了解代理的异步工作，请继续学习[云代理工作坊][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以配置工具、共享会话并了解后台运行情况。在本课中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看了可用模型。
- 了解了 `/share` 如何导出或共享会话。
- 探索了用于并行工作、worktree 和委托给云代理的可选命令。

Copilot CLI 还有许多其他斜杠命令。最后，我们将[回顾所学内容][next-lesson]以及后续学习步骤。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 上下文管理][context-management]
- [关于 Copilot 云代理][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
