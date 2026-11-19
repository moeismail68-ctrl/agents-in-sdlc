---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 应了解背景信息，就需要确保它能获取这些信息。提供特定上下文的方式有很多，其中重要的一种是[说明文件][instruction-files]，它可以为代码生成提供指导。

本练习将学习如何：

- 通过存储库自定义说明和按路径限定范围的说明文件，将项目上下文、编码指南和文档标准提供给 Copilot。
- 使用当前说明生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次运行相同提示，观察生成代码如何遵循新标准。
- 将说明更新和筛选代码切片提交到 `main`，供下一课使用。

> [!CAUTION]
> 生成的代码可能不完全遵循你设置的标准。Copilot 的行为具有概率性。本练习的重点是观察更新说明后行为的总体变化，而不是要求输出逐字符完全相同。

## 说明文件

### 场景

Tailspin Toys 有一套开发规范，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

借助说明文件，可以确保 Copilot 了解这些规范，并据此完成任务。

### 自定义说明

自定义说明可提供上下文和偏好，帮助 Copilot 了解编码风格和要求。你可以指定编码规范、库以及希望包含的注释类型。可以为整个存储库创建说明，也可以针对特定文件类型提供任务级上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。它应包含适用于大多数 Copilot Chat 或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可用于特定任务或文件类型，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，这些说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准格式的说明文件，例如 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳做法

完整讨论如何创建说明文件超出了本研讨会的范围。示例项目展示了一种有代表性的方法：

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供说明。
- 使用自然语言，保持指导清晰，并提供正反示例。

说明文件没有唯一的正确写法，AI 的使用方式也是如此。通过实践可以找到最适合项目的方案。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应维护一套完善的说明文件。项目中包含适用于 [UI 更新][ui-instructions]和 [Astro][astro-instructions]等任务的说明。
>
> Copilot 也可以帮助生成说明文件。不同界面提供的功能有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。
>
> 若要查找模板或起点，请浏览包含说明文件、自定义智能体等资源的 [awesome-copilot][awesome-copilot] 存储库。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义说明文件

此存储库包含一个核心 `copilot-instructions.md` 文件，以及一组适用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 指导。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有 Astro、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段设置一个相对于存储库根目录的 glob，用于匹配适用的文件；例如，符合 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看为本项目编写单元测试的专门说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾。注意其中指向 `unit-tests.instructions.md` 等说明文件和项目现有文件的链接。这样可以拆分并复用说明，还能提供示例。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求添加代码文档。下一步将添加 TSDoc 文档注释和文件头注释的规则。

## 创建更改分支

先创建分支来处理更改。

1. 返回上一课使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，以启动一个干净的对话。模式和模型选择因练习而异；每一步会在相关处说明。

先在当前说明文件生效时发送提示。随后更新说明并重新发送相同提示，比较两次结果。

1. 关闭之前练习遗留的编辑器标签页，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数及其测试。
6. 注意，建议的辅助函数是一个类型化函数：第一个参数为 `db` 客户端，返回类型化的发行商数组。这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层规范。
7. 注意建议代码**缺少** TSDoc 文档注释和文件级头部注释。

> [!CAUTION]
> Copilot 具有概率性，因此即使没有明确要求，也可能添加文档注释。如果发生这种情况也没关系；本练习重点是更新说明后行为的一致性改进。

## 添加存储库级标准

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约位于第 27 行）。注意其中没有代码文档规则，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 说明文件可放在存储库级别（`.github/copilot-instructions.md`）以提供全局指导，也可使用 `*.instructions.md` 为特定语言、文件类型或任务提供指导。存储库级文件适合放置刚添加的文档注释等项目规范。

## 再次运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清除缓冲区。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 关注正确文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 查看文件开头新增的注释块，例如：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 查看函数新增的 TSDoc 注释，例如：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用于下一课会继续使用的代码。

## 提交说明并推送分支

说明文件与存储库中的其他资产一样，需要通过源代码管理进行维护。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
2. 运行以下命令，确认说明更新和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 暂存并提交说明更新以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他配套更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并将其关联到待办事项中的文档标准议题。Copilot 可以使用已连接到项目的 GitHub 工具查找议题、创建关联 PR 并进行合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已配置 GitHub 工具，因此无需额外设置。Copilot 首次使用时，VS Code 可能会提示你登录 GitHub。后续课程会介绍此连接的工作方式（模型上下文协议）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器中选中 **Agent**。
3. 让 Copilot 查找说明文件更新议题，并从当前分支创建 PR，说明说明更新和新的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 系统提示 **Allow** Copilot 代表你执行 GitHub 操作时，请检查命令并酌情选择 **Allow**。
6. PR 创建后，使用以下提示要求 Copilot 合并 PR 并将分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 系统提示 **Allow** Copilot 执行 GitHub 操作和 shell 命令时，请检查命令并酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

你查看了 Copilot 如何从项目说明文件中获取上下文，并在 VS Code 中：

- 使用提示生成代码，并观察 Copilot 如何遵循*现有*说明。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次运行相同提示，观察建议的代码如何遵循新标准。
- 将说明更新和辅助函数基础提交到 `main`，供下一课继续使用。

接下来，在[智能体模式][next-lesson]中应用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳做法][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
