---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "了解存储库指令，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应让 Copilot 能够获取相应指导。[指令文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。现在你已完成筛选功能，接下来将了解 Copilot 使用的指令、添加文档标准，并将其应用于代码。

本课将介绍如何：

- 了解存储库指令和按路径指定的指令文件如何传递给智能体。
- 更新指令文件以确保遵循编码标准。
- 查看指令文件对代码的影响。

## 场景

Tailspin Toys 制定了一系列开发规范和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值；如有可注入的 `db` 参数，也要说明。
- 可复用 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持最新。
- 保留现有格式设置和 lint 指南。

通过指令文件，你可以确保 Copilot 获取正确的信息，并按照这些实践完成任务。

## 指令文件

自定义指令可以向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以指定编码规范、库以及注释风格，从而获得更贴合需求的建议和代码片段。你可以创建适用于整个存储库的指令，也可以为特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送此单一指令文件。内容应是适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 不同环境支持的其他指令格式可能有所差异。使用特定格式前，请参阅[自定义指令支持参考][custom-instructions-support]。

## 查看项目中的自定义指令文件

初始项目已包含一组指令文件。先了解现有内容，再进行更改，以便观察效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 阅读文件，了解项目简介和编码指导。此文件适用于该存储库中与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有适用于 Astro 文件、Drizzle 数据层、测试等的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它指定一个 glob，用于决定这些指令适用于哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件和项目现有文件的链接。较大的指令集可以拆分为较小、可复用的文件，还可以链接到示例供 Copilot 参考。

## 更新指令以符合团队规范

现有文件已经提供了良好起点，但还有一项规范需要补充。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，大约位于第 35 行。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存指令后，退出并恢复筛选对话，以便 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如有提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查已更改的 TypeScript 文件。查看新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结与后续步骤

你了解了 Copilot CLI 如何读取指令文件中的上下文，并将新的规范应用到功能中。具体来说，你：

- 了解了存储库指令和按路径指定的指令文件如何传递给智能体。
- 更新了指令文件，以确保遵循编码标准。
- 观察了指令文件对代码的影响。

接下来，你将[自定义并运行可复用的 quality-checks 技能][next-lesson]，以确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
