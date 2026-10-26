---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起检查筛选功能、说明、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证 (QA) 配置文件和测试都保存在同一个分支中。现在可以一起检查这些内容并打开拉取请求了。星级评分拉取请求 (PR) 是你自己合并的；这次将由 **Agent Merge** 管理流程。

> [!NOTE]
> 通常我们会将功能、说明更新、技能更新和 QA 代理拆分成多个 PR。为了简化工作坊，你在同一段对话和同一个分支中完成了整个筛选和质量工作流，并将这些更改放进这个 PR。

在本课中，你将：

- 了解 Agent Merge 是什么，以及它如何自动处理合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 仅在检查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选工作流中，你使用 Copilot 规划、实现并验证了一项功能。现在，Tailspin Toys 希望自动处理 PR 的剩余工作，同时由开发者掌控合并授权。

## Agent Merge 简介

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会解决阻碍 PR 的问题：修复失败的持续集成 (CI) 检查、回复审查意见，并在需要时进行变基。然后，如果仓库允许，Copilot 会启用 GitHub 自动合并。

到目前为止，你都是自己选择 **Merge pull request**。Agent Merge 可以接手这项工作。启用之前，请检查更改并确认已准备好合并。

## 使用 Agent Merge 管理 PR

代码已经全部完成。现在检查代码、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开代理对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监视 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示了类似 `PR #14 was squash-merged successfully.` 的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、仓库设置或缺少的权限。如果流程受阻，请阅读报告的原因；仓库允许时，手动完成已经检查过的合并。

## 总结和后续步骤

你已自动处理开发流程中的多个环节，包括生成代码、测试和验证，以及现在的拉取请求流程。你：

- 了解了 Agent Merge 是什么，以及它如何自动处理合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在检查后启用了 Agent Merge，并确认 PR 已合并。

接下来，你将[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，用于处理上下文、模型、共享和可选的云委托。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
