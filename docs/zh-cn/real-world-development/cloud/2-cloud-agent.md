---
title: "练习 2 - GitHub Copilot 云端代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

许多组织都面临技术债务问题，例如未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云端代理能够自主执行更新代码和添加功能等任务。完成工作后，它会创建一个草稿拉取请求，供开发人员审查。这样既能将繁琐任务交给代理处理、加快开发进度，也能让开发人员专注于更重要的工作。

本练习将探索 Copilot 云端代理的以下功能：

- 自定义代码生成环境；
- 确保操作安全地执行；
- 了解 issue 范围清晰的重要性；
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受聘创建网站的承包商没有妥善编写文档——实际上，代码完全缺少文档。首先，他们希望为应用程序中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望在每款游戏的详情页上显示相关游戏，也就是同一类别中的其他游戏，以便用户继续浏览。目前不需要精致的实现，只需要能用于用户体验验收测试的版本。这项工作目前是一个阻碍因素，但还有其他优先级更高的 issue。

这两项工作都很容易被降级处理，因此很适合分配给 Copilot 云端代理。云端代理可以异步处理它们，让开发人员继续完成其他任务，然后再回来审查 Copilot 的工作并确认结果符合预期。

## 认识 GitHub Copilot 云端代理

[GitHub Copilot 云端代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像人类开发人员一样在后台执行任务。与人类协作一样，你可以通过多种方式委派工作，包括[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建一个草稿拉取请求以跟踪进度、设置环境并开始处理任务。无论任务仍在进行还是已经完成，你都可以查看 Copilot 会话。方案准备好供你审查时，Copilot 会在拉取请求中通知你。

## 明确限定说明的范围

GitHub Copilot 没有魔法，不能让你跳过思考。看似简单的操作，深入分析后也可能变得复杂，因此[务必谨慎限定 Copilot 云端代理任务的范围][cloud-agent-best-practices]。将它视为人工智能（AI）结对编程伙伴：分阶段开展工作，不断学习、试验和调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义说明

前面的练习介绍了自定义说明及其对 Copilot 的指导作用。如果已经完成自定义说明实操，就会看到 `.github/copilot-instructions.md` 的实际效果；如果还没有，现在可以快速查看。在将工作分配给 Copilot 云端代理之前，只读查看此存储库中已有的说明文件，以便稍后观察它们的影响。

在存储库的 GitHub 网页界面中打开以下文件；如果已有正在运行的 codespace，也可以在那里打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，特别是 TypeScript 约定和 TSDoc 文档注释方面的要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` front matter，它将这些说明限定为适用于 `**/*.test.ts` 文件。

下一部分将把 `Code lacks documentation` issue 分配给云端代理。留意生成的拉取请求中的 TSDoc 文档注释、TypeScript 约定和注释标题；这些内容来自上述说明文件。稍后的练习还会再次提醒你检查这些内容。

## 为 Copilot 云端代理设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保一切就绪。将任务分配给 Copilot 时也一样；它会以类似软件工程师（SWE）的方式执行任务。

云端代理在执行任务时使用 [GitHub Actions][github-actions] 环境。你可以创建[专用设置工作流][setup-workflow]来自定义此环境，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，让它在云端代理开始工作前运行。这样可以为代理提供所需的开发工具和依赖项。实验已预先配置好此工作流，以便顺利完成操作并提供学习机会。它会确保 Copilot 能够使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单个 Astro 应用已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你在存储库的“Actions”选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设为执行*你的步骤*所需的最低权限。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果设置步骤不克隆存储库，步骤完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现已使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试使用其中的数据。
      - name: Set up the database
        run: npm run db:setup
```

这看起来与其他 GitHub 工作流文件相同，但有几个要点需要注意：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会先运行此作业。
- 留意 `workflow_dispatch` 触发器，它允许你在存储库的 **Actions** 选项卡中手动运行工作流。这样可以测试工作流是否成功运行，而不必等待 Copilot 触发它。

## 添加文档

大家都知道文档很重要，但多数项目要么信息过时，要么完全缺少文档。这类技术债务常常得不到处理，从而降低工作效率，也让维护代码库或引入新开发人员变得更困难。幸运的是，Copilot 擅长创建文档，因此这是一个非常适合分配给 Copilot 云端代理的 issue。它会在后台生成所需文档，之后的练习中你会回来审查结果。

1. 在新的浏览器选项卡中转到 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建 issue 对话框。
4. 选择 **Blank issue**，创建空白 issue。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），标题类似 `[WIP]: Code lacks documentation`。如果列表中没有出现新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应该会创建待办事项列表。

> [!NOTE]
> Copilot 生成待办事项列表可能需要几分钟。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方案。

13. 查看待办事项列表和 Copilot 将要完成的任务。
14. 向下滚动浏览拉取请求时间线，应该会看到 Copilot 已开始处理 issue 的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码和理解当前状态、暂停下来思考并确定合适的计划，以及编写代码。

这可能需要几分钟。Copilot 云端代理的主要目标之一是异步执行任务，让我们可以专注于其他工作。接下来就利用这一点：再给 Copilot 云端代理分配一项任务，然后把注意力转向为应用程序编写新功能。

## 在游戏详情页中添加相关游戏部分

如前所述，GitHub Copilot 云端代理的一大优势是可以拆分工作：你专注于一组任务，代理则处理另一组。为设计团队添加相关游戏部分或许不需要太长时间，但这段时间也可以用于其他工作。现在就把这项任务分配给 Copilot 云端代理。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建 issue 对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，issue 的第一条评论中会出现 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在认真处理第二项请求！Copilot 云端代理的工作方式类似 SWE，因此无需持续监视，等任务完成后再审查即可。接下来开始创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云端代理][copilot-agents]。你可以将 issue 分配给云端代理，让它异步处理任务。Copilot 可以帮助处理技术债务、创建新功能，或协助将代码从一种框架迁移到另一种框架。

本课探索了以下概念：

- 自定义代码生成环境；
- 确保操作安全地执行；
- 了解 issue 范围清晰的重要性；
- 将 issue 分配给 Copilot。

云端代理正在后台处理任务。现在可以转向创建和使用自定义代理。[Copilot 云端代理也可以使用模型上下文协议（MCP）服务器][cloud-agent-mcp]，并且可以使用前面模块介绍过的自定义说明。

## 资源

- [关于 Copilot 云端代理][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot 云端代理设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
