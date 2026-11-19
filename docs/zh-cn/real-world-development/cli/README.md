---
slug: zh-cn/real-world-development/cli
title: "GitHub Copilot CLI"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** 将 GitHub Copilot 智能体编码助手带入终端。它可以探索代码库、生成代码、运行命令并连接外部工具，让你无需切换到图形化编辑器即可保持工作流畅。

本系列练习将先安装并验证 Copilot CLI，然后在游戏卡片上添加星级评分，再使用 Plan 和 Autopilot 模式根据议题实现筛选功能。之后，你将更新自定义说明、使用 `quality-checks` 技能、连接 Playwright MCP 服务器并创建 QA 自定义智能体。最后，审查并合并功能 PR，探索用于管理上下文、模型、共享和云端委派的斜杠命令。还可以学习[使用 GitHub Copilot CLI 和 Foundry 的可选系列课程][foundry]，准备模型、构建并部署托管智能体，再将其集成到网站中。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 codespace |
| [1. 安装 Copilot CLI][ex1] | 安装 | 安装并验证 Copilot CLI，查找筛选议题 |
| [2. 添加星级评分][ex2] | 构建 | 在游戏卡片中显示星级评分并合并第一个 PR |
| [3. 智能体模式：Plan 和 Autopilot][ex3] | 规划与实现 | 从议题开始，规划并构建筛选功能 |
| [4. 使用自定义说明指导 Copilot][ex4] | 上下文 | 添加文档标准并应用到筛选代码 |
| [5. 智能体技能][ex5] | 技能 | 自定义并运行可复用的质量检查 |
| [6. 使用 Playwright MCP 验证功能][ex6] | 外部工具 | 在浏览器中检查筛选行为 |
| [7. 创建并使用 QA 智能体][ex7] | 智能体 | 创建 QA 配置并根据需求审查功能 |
| [8. 创建并合并功能 PR][ex8] | 审查 | 检查功能和证据，然后使用 Agent Merge |
| [9. Copilot CLI 斜杠命令][ex9] | CLI 功能 | 管理上下文、模型、共享和可选的云端委派 |
| [10. 总结和后续步骤][ex10] | 总结 | 回顾工作流并了解更多资源 |
| [可选：集成 Foundry][foundry] | 托管智能体 | 准备模型、构建并部署 Backer Concierge，再将其连接到网站 |

## 先决条件

参加本研讨会前，请确保具备以下条件：

- [ ] 拥有 GitHub 账户，并已启用 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划
- [ ] 对终端或命令行有基本了解
- [ ] 已安装并配置 Git

> [!TIP]
> 没有付费计划？已验证学生可通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本研讨会使用的智能体、MCP、代码审查和 Copilot CLI 功能，因此可完成所有工作流。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> 如果使用 Copilot Business 或 Copilot Enterprise，请确认管理员已启用 Copilot CLI。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
