---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并运行测试。开发者需要找到所有相关文件、进行修改、运行测试、调试，再找出遗漏的文件并继续更改。Copilot 智能体模式可以帮助处理这类工作。

Copilot 智能体模式能在 IDE 中更自主地工作。它会先探索项目结构，进行必要更新，运行测试等任务，并自动修复发现的问题。下面将使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体名称相似，但适用于不同的体验。智能体模式在 IDE 中执行任务，便于快速反馈和交互；云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将学习如何：

- 让 Copilot 智能体模式探索项目、识别相关文件并协同修改。
- 使用智能体模式跨 UI 和数据层实现新功能。
- 在合并到代码库前审查 Copilot 生成的更改和测试。

## 场景

游戏列表不断增加，因此需要让用户按类别和发行商筛选。上一课已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成其余数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行更改之前，先了解 Tailspin Toys 网站当前的功能。它是一个开发者主题的桌游众筹平台，可列出游戏并显示详细信息。该网站是一个 Astro app，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能由使用标准 DOM API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供 `npm run dev` 任务，用于启动单个 Astro 开发服务器。可在 GitHub Codespace 中按以下步骤运行：

1. 返回上一课结束后所在的 `main` 分支。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开 Codespace 终端。
3. 创建筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器启动后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 Astro 服务器地址 `http://localhost:4321`。

> [!NOTE]
> 在 codespace 中选择终端的 localhost 链接时，系统会自动将其转发到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向当前运行 Web 服务器的 codespace 的私有隧道。

### 探索网站

网站运行后，可以探索其功能：

- **主页**：显示桌游列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后，会打开显示标题、说明、发行商和类别等详细信息的页面。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，以启动一个干净的对话。模式和模型选择因练习而异；每一步会在相关处说明。

网站的初始实现已可正常运行，但还可以添加更多功能。先查看待办事项。使用模板创建项目时，系统已自动创建一组议题；让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。Agent 可自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用了 Claude Sonnet 4.5，因此在说明中列出该模型，希望由此获得相对一致的体验。不过，大语言模型具有概率性，收到的建议可能与说明不同，这是正常且预期的情况。

> [!NOTE]
> 由于大语言模型具有概率性，Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看生成的议题列表。

注意 Copilot 还按它认为对用户最有用的顺序排列了议题。

## 查看说明文件

让智能体生成代码之前，先查看将用于提供上下文的说明文件。`.github/instructions/ui.instructions.md` 包含如何为网站添加功能的指导。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括：
   - 项目架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向 Astro、Tailwind CSS 和 Drizzle 等文件类型专门说明的链接。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置。这样可以复用内容、拆分较大的说明，并为 Copilot 提供可遵循的示例。

## 实现筛选功能

完成筛选至少需要对应用进行三项更新：

- 在数据层（`src/lib/`）添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面以加入筛选 UI。

此外，合并前需要运行并通过测试。Copilot 智能体模式可以帮你完成这些任务。

1. 继续当前 Copilot 对话，或选择 **New Chat** 启动新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选择 **Agent**。
4. 确认模型仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并查找相关文件，包括数据层辅助函数、UI 和测试。然后它会修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试，这可能需要几分钟。过程中，编辑器中可能会出现 **Keep** 和 **Undo** 按钮。完成后，每项更改都会显示这两个选项；工作尚未完成时无需选择。

7. Copilot 提示时选择 **Continue** 运行测试。

   ![Copilot Chat 面板中的测试确认对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败，这是正常的。Copilot 会在生成代码和运行测试之间迭代，直到完成任务且未检测到错误。

   ![Copilot 智能体模式完整聊天会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 请务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，探索新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能。你了解到：

- Copilot 智能体模式可跨 UI 和数据层实现新功能。
- Copilot 智能体模式可探索项目、识别相关文件并协同修改。
- 合并前应审查智能体模式生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]并为其创建拉取请求。

### 延伸练习 - 实现分页

游戏列表增长后，需要支持分页。运用本练习中的技能，让 Copilot 为网站添加分页。考虑以下事项：

- 遵循现有最佳实践，包括使用已有说明文件。
- 决定分页实现方式，例如是否允许用户选择每页大小，或将其设为固定值。
- 编写提示时提供足够指导，让 Copilot 按照你的预期实现功能。
- 你可能需要与 GitHub Copilot 多次迭代、提出更改并补充上下文。这是正常的协作流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
