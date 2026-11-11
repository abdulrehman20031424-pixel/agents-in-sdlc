---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解一些背景信息，就应确保它能够获取这些上下文。可以通过多种方式向 Copilot 提供具体上下文，其中一种重要方式是使用[指令文件][instruction-files]，指导 Copilot 如何生成代码。

本练习将介绍如何：

- 通过存储库自定义指令和按路径指定的指令文件，向 Copilot 提供项目上下文、编码指南和文档标准。
- 在当前指令下生成第一个筛选数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的全局存储库规范。
- 再次运行相同提示词，观察生成的代码如何遵循新规范。
- 将指令更新和筛选代码切片提交到 `main`，让 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合某些规范。Copilot 的输出具有非确定性。本练习的目标是观察更新指令前后行为的总体变化，而不是要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 制定了一系列开发规范和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具备现代风格。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应有一段注释，说明文件的用途。

通过指令文件，你可以确保 Copilot 获取正确的信息，并按照这些实践完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以指定编码规范、库以及注释风格，从而获得更贴合需求的建议和代码片段。你可以创建适用于整个存储库的指令，也可以为特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送此单一指令文件。内容应是适用于大多数 Copilot 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，用于提供特定语言（如 TypeScript 或 Astro），或特定任务（如创建 UI 组件或单元测试）的指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

深入讨论如何创建指令文件超出了本研讨会的范围。示例项目展示了一种有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 中的指令聚焦于项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或任务提供指导。
- 使用自然语言，保持指导清晰，并提供代码示例说明正确和错误的写法。

创建指令文件没有唯一方式，正如使用 AI 也没有唯一方式。可以在自己的项目中尝试，找到适合的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的指令文件集合。查看本项目中的文件时，你会发现其中有用于 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务的指令。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式有所差异，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关课程会在合适的地方介绍操作方法。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

花些时间查看存储库自带的指令文件，其中包含一个核心 `copilot-instructions.md` 文件和一组用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 阅读项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等章节。在 **Code standards** 下查看嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有适用于 Astro 文件、Drizzle 数据层、测试等的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它指定一个相对于存储库根目录的 glob，用于决定这些指令适用于哪些文件。此处会匹配 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看项目中专门用于创建单元测试的指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它链接到其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件。这样可以将较大的指令集拆分为较小、可复用的文件，并为 Copilot 提供示例。（这些路径是相对于指令文件而不是存储库根目录的。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 章节记录了项目编码标准，但尚未要求在代码中添加文档注释。接下来会添加 TSDoc 文档注释和文件级注释头的规则。

## 为更改创建分支

遵循最佳实践，为更改创建一个分支。

1. 返回上一练习使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux)，打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，启动新的对话。不同练习所需的模式和模型可能不同，相关步骤会注明要求。

先在当前指令文件下发送提示词，再更新指令并重复同一个提示词，即可观察自定义指令的效果。

1. 关闭之前练习留下的编辑器选项卡，避免 Copilot 获得不需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 编辑文件。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数及其测试。
6. 注意生成的辅助函数是类型化函数：它以 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层规范。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有非确定性，因此它也可能在没有相关指令时添加文档注释。这没有问题；重点是观察更新指令后的一致性改进。

## 添加新的存储库规范

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在补充编码规范，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 章节（大约位于第 27 行）。这里记录了项目编码标准，但尚未要求代码中包含文档说明，这也是生成的辅助函数没有文档注释的原因。
3. 在现有规范下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 正如上一课所示，指令可以放在存储库级别的 `.github/copilot-instructions.md` 中，也可以针对特定语言、文件类型或任务放在 `*.instructions.md` 文件中。存储库级文件适合保存刚添加的文档注释等全局项目规范。

## 再次运行提示词并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除当前对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 关注正确的文件。
3. 发送与之前相同的提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意现在生成的文件开头包含类似以下注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意生成的函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目规范，并将它应用到下一练习会继续使用的代码中。

## 提交指令并推送分支

指令文件和其他资源一样，也需要使用源代码管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux)，打开新终端窗口。
2. 在终端中运行以下命令，确认指令和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新以及 Copilot 为辅助函数基础工作所更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求并将其关联到待办事项中的文档规范议题。你可以手动操作，但也可以让 Copilot 使用已连接到项目的 GitHub 工具。让 Copilot 查找议题、创建用于关闭该议题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一个让 Copilot 代表你操作 GitHub 的练习。项目模板已配置 GitHub 工具，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。请按照提示授权。后续练习会介绍此连接的工作方式（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) 打开 Copilot Chat 视图，并确保智能体选择器中选中 **Agent**。
3. 让 Copilot 查找与更新指令文件有关的议题，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 如果系统提示 **Allow** Copilot 代表你执行 GitHub 操作，请审查命令，并酌情选择 **Allow**。
6. PR 创建后，让 Copilot 合并 PR 并将本地分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果系统提示 **Allow** Copilot 执行 GitHub 操作和 shell 命令，请审查命令并酌情选择 **Allow**。

你已借助 GitHub Copilot 创建并合并拉取请求！

## 总结与后续步骤

你了解了 Copilot 如何读取项目指令文件中的上下文，并在 VS Code 中使用 Copilot Chat 完成以下任务：

- 发送代码生成提示词，观察 Copilot 在**现有**指令下的输出。
- 在 `.github/copilot-instructions.md` 中添加新的全局存储库规范。
- 再次运行相同提示词，观察生成的代码如何遵循新规范。
- 将指令更新和辅助函数基础工作提交到 `main`，供下一练习继续使用。

接下来，让这些指令在[智能体模式][next-lesson]中发挥作用，由 Copilot 跨存储库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个提示][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
