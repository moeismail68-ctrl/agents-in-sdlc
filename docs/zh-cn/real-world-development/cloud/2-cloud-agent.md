---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都面临技术债务：可能是尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。完成后，它会创建草稿 PR，供开发者审查。这样可以将繁琐任务交给智能体处理，让开发者专注于更宏观的工作。

本课将探索 Copilot 云智能体的以下用途：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初创建网站的承包商没有留下合适的文档，事实上，代码完全缺少文档。首先，他们希望为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改善游戏发现功能。他们希望游戏详情页显示相关游戏（同类别的其他游戏），让用户可以继续浏览。目前不需要精致的实现，只需要一个可用于验收测试用户体验的版本。虽然此任务目前受阻，但其他议题优先级更高。

这两项任务都可能很快被推迟，因此适合分配给 Copilot 云智能体。它可以异步处理任务，让开发者继续做其他工作，之后再审查结果并确认符合预期。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求跟踪进度、设置工作环境并开始处理。任务进行中或完成后都可以查看 Copilot 会话。解决方案准备好审查时，Copilot 会在拉取请求中提及你。

## 明确说明任务范围

GitHub Copilot 并非魔法，使用它并不能跳过思考过程。即使看似简单的操作，也可能包含复杂细节，因此要谨慎确定 Copilot 云智能体的任务范围，并遵循[云智能体最佳实践][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段工作、学习、尝试并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其如何指导 Copilot。如果已完成自定义说明练习，就会看到 `.github/copilot-instructions.md` 的作用；否则，现在可以快速查看。分配任务前，以只读方式检查存储库中已有的说明文件，之后即可观察它们的效果。

在 GitHub Web UI 中或已运行的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将说明限定到 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体。查看生成的 PR 时，注意 TSDoc 注释、TypeScript 规范和文件头注释；这些内容来自说明文件。后续练习会再次介绍审查 PR。

## 设置 Copilot 云智能体的开发环境

无论由谁编写代码，通常都需要准备特定环境并运行设置脚本。将任务分配给 Copilot 时也是如此，它会像软件工程师一样完成工作。

云智能体使用 [GitHub Actions][github-actions] 作为工作环境。可在 `.github/workflows/copilot-setup-steps.yml` 中创建[专用设置工作流][setup-workflow]，在 Copilot 开始工作前运行，以安装所需工具和依赖项。项目已预先配置此工作流，以便顺利完成实验。它会准备 Node.js、项目依赖项、端到端测试使用的浏览器，以及已迁移并填充数据的单一 Astro app 的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的 Actions 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为步骤所需的最低权限。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 contents: read 权限。如果未在设置步骤中克隆，步骤完成后 Copilot 会自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端 / app 设置 - Node.js（整个 app 现在基于 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件类似，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许你从存储库的 **Actions** 选项卡手动运行工作流。这有助于验证工作流，而不必等待 Copilot 启动它。

## 添加文档

尽管大家都知道文档的重要性，许多项目仍有过时的信息或完全缺少文档。这类技术债务经常无人处理，会降低效率，也让代码维护和新成员加入团队变得更困难。Copilot 擅长生成文档，因此适合将此类任务分配给云智能体。它会在后台生成文档，你可以在后续练习中审查结果。

1. 在新的浏览器标签页中转到 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新创建的 PR，标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中还没有新 PR，请等待一两分钟并刷新页面。
12. 几分钟后，Copilot 应会创建待办列表。

> [!NOTE]
> Copilot 创建待办列表可能需要几分钟。它正在创建工作环境（运行前面介绍的工作流）、分析项目并确定最佳方案。

13. 查看 Copilot 将要执行的任务列表。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session**。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，查看 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定计划，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让你专注于其他工作。接下来会利用这一点：先再分配一项任务，然后转而为应用编写其他功能。

## 在游戏详情页添加相关游戏部分

GitHub Copilot 云智能体的一大优势是可以拆分工作：你专注于一组任务，它同时处理另一项任务。即使为设计团队添加相关游戏部分不会花太久，也可以把时间用于其他工作。现在将任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
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

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，你会在议题的第一条评论中看到 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。它的工作方式类似软件工程师，因此无需一直监控；完成后再审查即可。现在可以转向创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给 Copilot 并让它异步处理，用于解决技术债务、创建新功能，或协助将代码迁移到其他框架。

你探索了以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

云智能体正在后台工作，现在可以开始创建和使用自定义智能体。[Copilot 云智能体也支持 MCP 服务器][cloud-agent-mcp]，并可使用前面模块介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
