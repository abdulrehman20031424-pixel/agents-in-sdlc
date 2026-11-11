---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案，使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一个小功能。较大的更改则需要更严谨的流程。GitHub Copilot CLI 可以配合组织现有的工作流，帮助我们以正确的方式构建正确的功能。接下来的几课将介绍典型的智能体驱动开发流程：从议题开始生成新功能，确保代码有效、功能符合预期，最后成功合并到项目中。

> [!NOTE]
> 在整个功能工作流中，你会继续使用同一对话和分支。通常，不同类型的文件会使用不同分支或 PR；本研讨会为了聚焦核心概念而简化了流程。

本课将介绍如何：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

后续课程中，你将更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并打开功能 PR。

## 场景

Tailspin Toys 的商品目录不断扩大，访问者需要按类别和发行商筛选游戏。待办事项中的议题描述了该功能，但组合类别等细节需要在编码前确定。你将使用 Plan 模式处理这些决策，然后授权 Autopilot 在明确范围内实现功能。

## 背景

在开发流程中加入 AI 编码智能体，并不会改变软件开发的基本原则；这些原则反而更重要！大多数开发人员遵循的流程大致如下：

1. 从描述需求的已创建议题开始。
2. 制定实现方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述方式类似。

遵循这种标准流程，可以确保 AI 生成的代码满足需求，并像手写代码一样经过审查。

## 对话模式

**对话模式**决定智能体拥有多少自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive：** 你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan：** 智能体先制定计划，但不能编辑项目文件。
- **Autopilot：** 智能体自主工作，编写代码、运行测试并不断迭代，直至任务完成。

先从 Plan 模式开始，审查计划，然后使用 Autopilot 实现。

## 从议题开始

开始筛选工作前，先返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux)。
4. 更新 `main`，然后为筛选工作创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd>，打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示词，并返回 **Session** 选项卡。

此时提示词开头会显示 `#7` 或类似编号。`#` 可用于将 GitHub 上的议题或拉取请求 (PR) 添加到上下文中。

## 规划筛选功能

规划可以先确定实现功能或执行任务的方案，再交给 Copilot。在处理复杂任务前花些时间规划总是有益的。切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示词下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示词：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划：先探索项目，再确定最佳方案。

3. Copilot 可能会询问筛选功能的细节。根据你的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) 展开计划。
5. 上下滚动审查计划。
6. 如果计划中的任何内容与你的决定不符，请让 Copilot 修改。

## 批准 Autopilot

计划已编写并审查完毕，现在开始实现。使用 Autopilot 让 Copilot 执行计划。

Autopilot 会让 Copilot 持续处理问题，直至它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中类似的选项。
2. 确认提示词下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按照既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再判断检查是否完成。

## 审查并验证实现

代码生成后，和其他代码一样，合并前需要审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 根据议题和规划时的决定审查结果。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果；如有失败，请让 Copilot 修复。

## 试用新功能

代码看起来没问题，但实际运行情况如何？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**。
3. 测试类别筛选、发行商筛选，以及计划中确定的组合方式。
4. 确认重置和无结果时的行为符合议题及你的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结与后续步骤

你已使用不同对话模式来构建和审查功能。本课中，你：

- 从 GitHub 议题开始了新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，我们将深入了解如何让生成的代码遵循书面规范，并[使用自定义指令][next-lesson]。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
