---
title: "第 10 课 - 回顾和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你使用 GitHub Copilot CLI 完成了 Tailspin Toys 的连续工作流。你：

- 准备了 Codespace，安装了 Copilot CLI，探索了项目并找到了预先创建的筛选 issue。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并了首个拉取请求 (PR)。
- 从筛选 issue 开始，在 Plan 模式下确定方案，在 Autopilot 模式下构建，并在 Interactive 模式下检查。
- 使用自定义说明引导代理，自定义了现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加了 Playwright Model Context Protocol (MCP) 服务器，并通过它在真实浏览器中探索筛选功能。
- 创建并选择了质量保证 (QA) 自定义代理，以评估需求、覆盖率、技能结果和浏览器证据。
- 检查了完整的筛选更改，并授权 Agent Merge 处理筛选 PR。
- 探索了用于上下文、模型、共享和可选云委托的斜杠命令。

## 交付内容

本工作坊包含两个 PR 阶段，每个阶段都在从更新后的 `main` 创建的独立分支上完成：

1. **星级评分**：在游戏卡片上显示已有的 `starRating`，并明确显示未评分状态。
2. **筛选和质量工作流**：实现筛选功能，更新说明并将其应用于功能，自定义 `quality-checks` 报告，创建 QA 配置文件，并添加相关测试。

从规划筛选功能到打开 PR，你一直使用同一段对话和同一个分支。为了简化工作坊，我们将这些工作合并到一个 PR 中。

## 不同的验证方式

你通过多种方式检查了代码：自动化测试、自己在浏览器中检查，以及使用 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 在创建 PR 前，将这些结果与需求和测试覆盖率检查整合起来。

新增测试应该填补实际缺口；QA 运行后认为无需添加测试也可能是正确的。授权合并前，请检查代码和证据；更改后请更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作方式。在本工作坊中，你更新了说明、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在不同对话中重复使用这些自定义项，并根据团队需求进行调整。说明用于设定标准，技能用于描述可重复执行的任务，自定义代理用于定义专门角色，MCP 服务器用于连接外部工具。请检查实际更改和工具结果，而不只是代理的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 在开始构建前思考方案，使用 **Interactive** 跟进范围明确的更改，使用 **Autopilot** 处理定义清楚的任务。常规编辑可以选择更快的模型，复杂工作则可以选择能力更强的模型。

上下文与基础设施同样重要。清楚说明你要构建*什么*、*为什么*以及*如何*构建，会显著影响结果。

## 探索更多内容

你已经完成核心工作流。还可以了解一些 CLI 功能：

- `/review`：让代码审查代理分析更改。
- `/rubber-duck`：讨论问题并获得另一种看法。
- `/fleet`：并行协调彼此独立的子任务。
- `/worktree`：隔离另一项任务。
- `/delegate`：将任务发送给 Copilot 云代理。

## 后续步骤

提升工具使用能力的最佳方式就是持续使用。你可以将它用于生产代码、个人项目，或那个多年来一直想做却没做的小应用。与团队分享经验，也向其他人学习。别忘了继续探索文档。

如果想从终端扩展 Tailspin Toys，请继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他环境，请探索 [VS Code 工作坊][vscode]、[GitHub Copilot 应用工作坊][app]或 [Copilot 云代理工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
