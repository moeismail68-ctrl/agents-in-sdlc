---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果某项任务需要以特定方式完成，或 Copilot 应了解某些背景信息，就需要确保它能获取这些信息。[说明文件][instruction-files]用于提供此类指导，让 Copilot 不仅理解要做什么，也理解应该如何完成。

本练习将学习如何：

- 通过存储库自定义说明和按路径限定范围的说明文件，将项目上下文、编码指南和文档标准提供给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作流不同，此处不会比较提示修改前后的输出。Copilot 云智能体会在 GitHub 议题上异步工作，因此难以实时并排展示效果。稍后审查云智能体生成的拉取请求时，可以看到说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 有一套开发规范和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

借助说明文件，可确保 Copilot 了解这些规范，并据此完成任务。

### 自定义说明

自定义说明可向 Copilot 提供上下文和偏好设置，帮助它了解你的编码风格和要求。你可以指定编码规范、库，甚至希望代码包含的注释类型。可以为整个存储库创建说明，也可以针对特定文件类型提供任务级上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。它应包含适用于大多数 Copilot Chat 或 CLI 请求的项目级信息。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建的说明文件，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，这些说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准格式的说明文件，例如 AGENTS.md 和 CLAUDE.md，详情请参阅[自定义说明支持][custom-instructions-support]。

### 管理说明文件的最佳做法

完整讨论如何创建说明文件超出了本研讨会的范围。示例项目展示了一种有代表性的方法：

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供说明。
- 使用自然语言，保持指导清晰，并提供代码示例。

说明文件没有唯一的正确写法，AI 的使用方式也是如此。通过实践可以找到最适合项目的方案。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应维护一套完善的说明文件。查看本项目中的文件时，你会发现其中有用于 [UI 更新][ui-instructions]和 [Astro][astro-instructions]等任务的说明。
>
> Copilot 也可以帮助生成说明文件。不同界面提供的功能有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。本工作流会在相关课程中介绍这些功能。
>
> 若要查找模板或起点，请浏览包含说明文件、自定义智能体等资源的 [awesome-copilot][awesome-copilot] 存储库。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义说明文件

花些时间查看此存储库随附的说明文件：其中有一个核心文件 `copilot-instructions.md`，以及一组适用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中有 Astro、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段；它设置一个相对于存储库根目录的 glob，用于匹配适用的文件。这里会匹配 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看为本项目编写单元测试的专门说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明拆分成较小、可复用的文件，并提供可供 Copilot 参考的示例。路径相对于说明文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求添加代码文档。下一步将添加 TSDoc 文档注释和文件头注释的规则。

## 添加新的存储库标准

这是本课唯一需要编辑的内容：添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级头部注释。之后本工作流中分配给云智能体的任务都会采用这条规则。

编辑前，先创建并切换到新分支。云智能体会读取议题目标分支中的说明，但仍应在独立分支上提交更改：

1. 在 codespace 终端中创建并切换分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在更新存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到大约位于第 27 行的 **Code formatting requirements** 部分。该部分记录了项目的编码标准，但没有说明代码文档规则。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 前一课介绍了如何在存储库级别（`.github/copilot-instructions.md`）创建全局说明，或使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供说明。项目级文件适合放置诸如文档注释规则等全局标准。

## 提交、推送并合并说明更新

云智能体会读取议题目标分支中的说明。下一课分配议题时，Copilot 会从 `main` 创建分支，因此必须先将说明更改合并到 `main`，云智能体才能使用。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求，并合并。之后云智能体处理议题时就会读取这些说明。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处采用分支是为了模拟真实项目中的工作方式。

## 总结和下一步

你已了解 Copilot 如何从项目说明文件中获取上下文，并在 `.github/copilot-instructions.md` 中添加了一项存储库级标准。在后续练习中，可以观察云智能体生成的拉取请求是否遵循了该标准。

接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳做法][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
