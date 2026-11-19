---
title: "第 4 课 - 使用自定义说明指导 Copilot"
description: "查看存储库说明、添加文档标准，并将其应用到筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就要让 Copilot 能够获取相应指导。[说明文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。完成筛选功能后，现在可以查看 Copilot 使用的说明、添加文档标准并应用到代码中。

本课将学习如何：

- 查看存储库说明和按路径限定范围的说明文件如何提供给智能体。
- 更新说明文件以确保遵循编码标准。
- 了解说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发规范和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值（包括可注入的 `db` 参数）。
- 可复用 Astro 组件应说明其 `Props` 接口，并在相关代码变更时更新注释。
- 保留现有格式和 lint 指南。

通过说明文件，可以确保 Copilot 了解这些规范。

## 说明文件

自定义说明能提供上下文和偏好，帮助 Copilot 了解编码风格和要求。可以指定规范、库以及注释风格，也可以为整个存储库或特定文件类型创建说明。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件，适合存放大多数请求都相关的项目级信息。
- `.github/instructions/*.instructions.md`：用于特定语言、文件类型或任务的指南。

> [!NOTE]
> 不同环境支持的其他说明格式可能有所不同。使用特定格式前，请查看[自定义说明支持参考][custom-instructions-support]。

## 查看项目中的自定义说明文件

起始项目已包含一组说明文件。先查看现有文件，再修改它们以观察影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。它们适用于对此存储库进行的所有 Copilot 交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中有 Astro、Drizzle 数据层、测试等说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段设置一个 glob，用于确定适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他说明文件和项目文件。这有助于拆分和复用说明，并提供示例。

## 更新说明文件以符合团队规范

现有文件是不错的起点，但还缺少一项要求。更新核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，大约位于第 35 行。
2. 将以下内容添加为最后一条：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话启动时加载存储库说明。保存更新后，退出并恢复筛选会话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 将其关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查更改过的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何从说明文件中获取上下文，并将新标准应用到功能代码中。具体来说，你：

- 查看了存储库说明和按路径限定范围的说明文件如何提供给智能体。
- 更新了说明文件以确保遵循编码标准。
- 观察了说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳做法][instructions-best-practices]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
