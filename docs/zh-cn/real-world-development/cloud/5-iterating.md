---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 协作完成了多项任务，以改善用户体验并添加功能：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式等无障碍功能。现在检查代码更改，并在必要时向 Copilot 提供反馈。

### 场景

引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终需要审查生成的代码并遵循正常 DevOps 流程。现在审查 Copilot 为文档、相关游戏功能和无障碍功能提出的更改，然后再允许团队其他成员进行审查。

## 安全和 GitHub Copilot 云智能体

Copilot 云智能体会在无人监督的情况下异步执行任务，因此设有安全限制：

- Copilot 对存储库只有读取权限，对工作分支才有写入权限。
- 云智能体在 GitHub Actions 中运行，并在单独的临时环境中工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先通过 GitHub.com 的常规 PR 界面查看 Copilot 云智能体创建的第一个拉取请求（PR），其中包含代码文档。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如有需要，可以[自定义或禁用云智能体防火墙][agent-firewall]。

1. 在浏览器中返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 查看拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含 **[WIP]**。此时请等待 Copilot 完成，可能需要几分钟。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新增的 TSDoc 文档注释和其他文档。实际更改可能有所不同。检查是否包含 TSDoc 注释、TypeScript 规范和文件头注释；这些要求来自练习 2 中查看的自定义说明文件。
6. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流。
8. 如果有，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 检查区域排队。顺利的话，单个 Astro app 的项目检查会全部通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向过程。你可以像在 PR 中标记团队成员一样，通过一般评论或代码行内评论标记 Copilot。Copilot 会读取评论并启动新会话来处理请求。由于结果具有非确定性，无法提供唯一固定的提示文本；可以要求 Copilot：

- 为每个代码文件开头添加简短说明其用途的注释。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建介绍 Astro app 结构的 README。

1. 在 PR 中添加评论并标记 **@copilot**，提出文档更改请求。可以使用上述建议，也可以提出其他期望。
2. 选择 **View Session** 观察 Copilot 工作。Copilot 会启动新会话来应用更改。
3. 选择 **Back to pull request** 可返回 PR。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会显示新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直到满意。准备就绪后，可将草稿 PR 标记为可审查，然后合并到 main 分支。

![将 PR 标记为可审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

返回 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流。
7. 如果有，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 查看 PR 检查区域中的工作流。顺利的话，单个 Astro app 的项目检查会全部通过；这可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开终端，将 `<branch-name>` 替换为 Copilot 创建的分支名称（例如 **copilot/fix-8**），然后运行：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能。与之前一样，可以迭代改进结果，例如调整显示的相关游戏数量，或要求添加文件头注释和 TSDoc 文档注释（请记住，分配该议题时尚未更新自定义说明）。在 **Conversation** 选项卡添加评论，Copilot 会看到并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 在任务控制中心提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含 **[WIP]**。请等待任务完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查实现时，重点关注：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保留用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流。
8. 如果有，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 查看 PR 检查区域中的工作流。顺利的话，单个 Astro app 的项目检查会全部通过，这可能需要几分钟。
10. **可选：**在 Codespace 中切换到该分支并手动测试无障碍功能。将 `<branch-name>` 替换为 Copilot 创建的分支名称：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换开关，并确认页面重新加载后设置仍保留。

注意无障碍自定义智能体如何帮助 Copilot 按最佳实践实现功能。如果发现无障碍问题或改进点，可像之前一样在评论中标记 **@copilot** 并要求更新。

## 可选练习 - 继续委派工作

云智能体适合处理真实待办任务，让你同时专注于其他工作。可创建更多议题并分配给 Copilot，例如：

- 在游戏详情页添加支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体工作流。本系列课程中，你：

- **检查了存储库附带的自定义说明文件**，以便稍后观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、规划并异步执行任务。
- **创建并使用无障碍自定义智能体**，添加高对比度和浅色模式切换开关。
- **使用 Copilot agents 页面作为任务控制中心**，监控并引导无障碍会话。
- **审查并迭代云智能体的拉取请求**，通过标记 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

云智能体工作流已完成。其他工作流可以补充你学到的内容：

- 🖥️ **[VS Code 工作流](../../vscode/)** — 在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 工作流](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的存储库中，还可以尝试：

- 将重构或测试覆盖议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
