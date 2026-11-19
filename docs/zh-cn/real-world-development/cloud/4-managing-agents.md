---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两课中，你已让 Copilot 云智能体处理了三项改进用户体验和添加功能的任务。云智能体虽然会异步自主工作，监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有包含开放拉取请求（PR）的智能体任务，查看它执行的操作，甚至引导正在进行的会话。

本课将学习如何：

- 使用智能体页面监控云智能体任务。
- 在会话进行时引导智能体添加新功能。

## 场景

将任务分配给智能体实现高对比度模式后，团队决定同时添加浅色模式。由于已经在更新网站样式并添加切换功能，加入浅色模式很合适。你将引导智能体，同时实现这两种模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的任务状态：

1. 转到 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。你应能看到分配给 Copilot 的任务，包括：
   - 为代码库补充文档。
   - 生成修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 执行的操作，例如检出代码、创建工作环境、设置 MCP 服务器以及完成任务的各个步骤。

> [!NOTE]
> 列出的具体步骤会因 Copilot 当前状态和实现方式而异。

4. 还可以查看右侧的拉取请求面板，监控 PR 和已更改文件。

## 引导云智能体

查看活动任务后，要求 Copilot 在实现高对比度模式时加入浅色模式切换开关。

1. 选择与高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，等待它完成设置并开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![突出显示引导活动会话对话框的智能体页面截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务！它会先完成发送消息时正在执行的步骤，再将新请求加入流程。

处理可能需要几分钟。等待时，可以回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，即监控和引导 GitHub Copilot 云智能体任务的中心位置。借助这个任务控制中心，可以跟踪所有进行中和已完成的任务、审查工作并引导正在进行的任务。

你探索了以下内容：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 引导正在进行的会话添加功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
