---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

前面为项目添加了一个小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可配合组织现有流程，确保以正确方式构建正确功能。本课将开始一组课程，介绍典型的智能体开发流程：从议题开始生成新功能，确认代码有效、行为符合预期，最后成功合并到项目。

> [!NOTE]
> 后续功能工作将沿用同一对话和分支。实际工作中，通常会为不同文件类型使用不同分支或 PR；本课程为了突出核心概念而简化了流程。

本课将学习如何：

- 从 GitHub 议题启动新的 Copilot 对话。
- 在 Plan 模式下确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，还会更新存储库说明、自定义现有 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩展，访问者需要按类别和发行商筛选。待办事项中的议题说明了功能，但诸如如何组合类别等细节需要在编码前确定。你将使用 Plan 模式解决这些问题，然后授权 Autopilot 在限定范围内完成实现。

## 背景

在开发流程中引入 AI 编码智能体并不会改变基本原则。大多数开发者的流程大致如下：

1. 从描述需求的已创建议题开始。
2. 规划需要构建的内容。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码审查完成且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述方式类似。

遵循标准流程可以确保 AI 生成的代码满足需求，并经历与手写代码相同的验证过程。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出更改并等待你的输入后再继续。
- **Plan**：智能体先创建计划，且不能编辑项目文件。
- **Autopilot**：智能体自主编写代码、运行测试并迭代，直至任务完成。

先在 Plan 模式中查看计划，再使用 Autopilot 实现。

## 从议题开始

开始筛选功能前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace；如果已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后创建筛选功能分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题加入提示并返回 **Session** 选项卡。

注意提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 议题或拉取请求（PR）加入上下文。

## 规划筛选功能

规划可以在交给 Copilot 实施前，先确定功能或任务的实现方式。对于复杂任务，花时间规划通常很有帮助。现在切换到计划模式并让 Copilot 创建计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定合适的实现方案并生成计划。

3. Copilot 可能会询问筛选行为的细节。根据偏好作答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看并审查计划。
6. 如果计划与决定不符，请要求 Copilot 修改相应部分。

## 批准 Autopilot

计划已编写并审查，现在可以实施。切换到 Autopilot，让 Copilot 根据计划迭代，直到认为任务完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中含义相近的选项。
2. 确认提示下方显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实施，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再判断检查是否完成。

## 审查并验证实现

代码生成后，与其他代码一样，合并前也需要审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff` 并检查筛选实现和测试。
3. 根据议题和规划时做出的决定检查结果。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果；如有失败，请让 Copilot 修复。

## 探索新功能

代码看起来没问题，但能否实际运行？像之前一样启动应用，通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中确定的组合方式。
4. 确认重置和无结果时的行为符合议题及你的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了功能。本课中你：

- 从 GitHub 议题启动了新的 Copilot 对话。
- 在 Plan 模式下确定了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，通过[使用自定义说明][next-lesson]了解代码生成，确保其遵循文档化的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
