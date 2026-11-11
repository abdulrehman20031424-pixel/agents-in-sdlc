---
title: "练习 1 - 自定义指令（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解一些背景信息，就应确保它能够获取这些上下文。[指令文件][instruction-files]可以提供这些指导，让 Copilot 不仅了解你想让它做什么，也了解你希望它如何完成。

本练习将介绍如何：

- 通过存储库自定义指令和按路径指定的指令文件，向 Copilot 提供项目上下文、编码指南和文档标准。
- 在 `.github/copilot-instructions.md` 中添加新的全局存储库规范。

> [!NOTE]
> 与 VS Code 和 CLI 练习环境不同，本练习不会比较提示词执行前后的效果。Copilot cloud agent 会异步处理 GitHub 议题，因此难以实时对照。稍后审查 cloud agent 创建的拉取请求时，你会看到指令文件的影响。

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

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送此单一指令文件。内容应是适用于大多数 Copilot 聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
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

## 添加新的存储库规范

本练习中你只需进行一项编辑：添加全局项目规范，要求在代码中使用 TSDoc 文档注释和文件级注释头。后续练习中，cloud agent 处理分配给它的议题时会遵循此规范。

编辑前，先创建一个分支（cloud agent 会从议题目标分支读取指令，但仍建议干净地提交这些编辑）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并指令更新

Cloud agent 会读取议题目标分支中的指令。下一练习中，将从 `main` 分支创建任务，因此指令更改必须先合并到 `main`，cloud agent 才能使用。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。下一练习中，cloud agent 就会读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main` 分支，可以跳过创建分支，直接提交到 `main`。此处创建分支是为了演示真实项目中的工作方式。

## 总结与后续步骤

你了解了 Copilot 如何读取项目指令文件中的上下文，并在 `.github/copilot-instructions.md` 中添加了新的全局规范。后续练习中，可以在 cloud agent 创建的拉取请求里看到此规范的效果。

接下来，你将[把第一个议题分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个提示][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
