---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求前，应先确认它能在浏览器中正常运行。本练习将连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器测试功能，然后发布分支并创建 PR。

本练习将学习如何：

- 了解模型上下文协议（MCP）如何通过服务器为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 在浏览器中手动测试筛选功能。
- 发布分支并为筛选工作创建拉取请求。

## 什么是模型上下文协议（MCP）？

当智能体模式可以使用编辑器以外的工具时，其能力会显著增强。模型上下文协议（MCP）是 Copilot 与这些工具交互的标准方式。

![智能体模式及其与上下文、LLM 和工具（包括 MCP 服务器和 VS Code 扩展提供的工具）交互的示意图](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)让 AI 智能体能够与外部工具和服务通信。使用 MCP 后，智能体可以通过资源获取最新信息，并通过工具代表你执行操作。

智能体通过 MCP 服务器访问工具和资源。MCP 服务器充当 AI 智能体与外部工具、服务之间的桥梁，可连接现有 API 或 npm 包等本地工具。每个 MCP 服务器代表智能体可访问的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库的 API，让智能体创建存储库、更新内容并管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，让智能体导航网页、填写表单和选择按钮。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表](https://github.com/mcp)，帮助发现并参与生态系统。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前应仔细审查源代码、验证发布者并评估安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区可使用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你会看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板包含此 `github` 项，因此上一课中的 Copilot 可以读取待办事项议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 通过 HTTP 连接，并在 Copilot 首次使用工具时使用 GitHub 账户进行身份验证。

## 添加 Playwright MCP 服务器

接下来添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控浏览器，用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 项，与 `github` 并列：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记让 Playwright 在没有可见窗口的情况下运行浏览器，这对于没有桌面环境的 codespace 是必要的。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，通常已安装浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 项上方会出现 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任该服务器，请检查配置并选择信任，以便服务器启动。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步 Copilot 首次需要服务器工具时启动服务器并询问是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]让 Copilot 驱动真实浏览器。智能体可以打开页面、导航、应用筛选并读取结果，然后总结观察内容，无需离开对话即可快速验证功能行为。

Playwright MCP 服务器通过页面的[无障碍树][playwright-mcp-server]而不是截图操作。智能体基于结构化且带标签的元素（按钮、链接和列表项）进行推理，与辅助技术的工作方式相同，因此功能检查也能顺便进行简单的无障碍验证。

连接服务器并启动应用后，使用以下提示让 Copilot 测试筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步操作并报告结果。请根据议题验收标准审查其总结；如果发现异常，请继续询问或要求它修复代码，然后再创建 PR。

> [!NOTE]
> 测试时应用必须在 `http://localhost:4321` 运行。如果已停止开发服务器，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；若报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，即可创建拉取请求（PR）供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板并审查 Copilot 的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish** 将分支推送到存储库。

## 创建拉取请求

创建 PR 的方式有很多，例如通过 github.com 或 GitHub 命令行接口（CLI）。现在可以让 GitHub Copilot 创建 PR，并将其与相关议题关联。

1. 打开 Copilot Chat 面板，选择 **New Chat** 启动新会话。
2. 确认智能体下拉列表中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 发送以下提示：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 获取信息并执行操作。首次使用 GitHub 工具时，系统可能会要求你进行 GitHub 身份验证；按提示授权。
5. 查看 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接审查拉取请求，但暂时**不要合并**。

## 总结和后续步骤

恭喜！在本练习中，你：

- 了解了 MCP 如何通过服务器为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 在发布前使用它在浏览器中手动测试筛选功能。
- 发布分支并为筛选工作创建拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 究竟是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
