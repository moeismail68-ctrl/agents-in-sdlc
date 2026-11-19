---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。通过在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，可以提供专门的说明、最佳实践、编码模式和领域知识，帮助 Copilot 更有效地完成特定工作。团队可以将专业知识整理成可复用智能体，例如执行 [WCAG][wcag] 合规检查的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可全局存放在 `~/.copilot/agents`。每个文件都包含至少具有 `name` 和 `description` 的 YAML frontmatter，后接定义智能体行为、专长和说明的 Markdown 提示。

### 自定义智能体与智能体技能的比较

自定义智能体和[智能体技能][agent-skills-concept]有些相似。两者通常都使用 Markdown 文件定义，并告诉 AI 如何完成工作。最简单的区别是：**自定义智能体是执行工作的角色，技能是工具**。

自定义智能体拥有自己的上下文窗口，可在工作时编排技能甚至其他智能体。本实验中的无障碍智能体会根据无障碍指南审查并更新网站；它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 自定义智能体没有唯一的正确写法。应在自己的环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

你将探索以下内容：

- 自定义智能体的定义方式。
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视力能力或偏好的影响。用户反馈指出，当前暗色主题的文本和背景对比度不足，导致部分用户难以阅读。设计团队因此要求添加可开关的高对比度模式。你将使用自定义智能体尽快生成这一功能。

## 查看无障碍自定义智能体

项目已创建无障碍自定义智能体。先查看它如何指导 Copilot。

1. 在 codespace 中打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 浏览其余部分，了解其中介绍的核心职责、无障碍最佳实践、HTML/CSS/JavaScript 示例，以及常见陷阱。

## 创建并分配议题

任务控制中心是管理环境中所有智能体的中心位置。可以将任务分配给 Copilot 云智能体、监控任务并提供额外指导。现在创建高对比度模式议题并分配给 Copilot。

1. 转到你的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![突出显示自定义智能体和无障碍选项的云智能体分配截图](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即针对特定任务和领域定制的 AI 助手。借助自定义智能体，可以将团队专业知识和标准整理成可复用角色，指导 Copilot 更有效地完成工作。

你探索了以下内容：

- 自定义智能体的定义方式。
- 如何将任务分配给自定义智能体。

Copilot 正在后台实现高对比度模式。接下来，[在任务控制中心监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [在组织中使用自定义智能体的准备工作][org-custom-agents]
- [在企业中使用自定义智能体的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
