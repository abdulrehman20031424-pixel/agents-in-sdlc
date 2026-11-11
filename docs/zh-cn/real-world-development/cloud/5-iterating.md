---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

本实验中，你与 GitHub Copilot 一起完成了多项任务，以改善用户体验并添加功能。你让 Copilot 为代码添加文档、实现相关游戏功能供设计团队迭代，并实现包括高对比度和浅色模式切换在内的无障碍功能。现在审查代码更改，并在需要时向 Copilot 提供反馈，改进其工作。

### 场景

正如多次提到的，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来，在为团队其他成员启用审查前，先检查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提供的建议。

## GitHub Copilot cloud agent 的安全性

Copilot cloud agent 会在无人监督的情况下异步执行任务，因此它受到一些安全限制，以确保操作安全：

- Copilot 对存储库只有读取权限，对其代码分支才有写入权限。
- Cloud agent 在 GitHub Actions 中运行，并创建独立的临时环境进行工作。
- 任何 GitHub Actions 工作流在运行前都需要经过人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个拉取请求 (PR)，即为代码添加文档。请使用 GitHub.com 的标准 PR 界面。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括调用外部 MCP 服务器。你可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 或更完整的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 会包含 **[WIP]** 标记。此时请等待 Copilot 完成工作，这可能需要几分钟。你可以休息片刻，或回顾目前学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加到代码中的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    查看更改时，留意 TSDoc 文档注释、TypeScript 规范和注释头。这些来自练习 2 开始时查看的自定义指令文件。

6. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 工作流应在 PR 的检查区域中排队运行。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向过程。你可以像在团队 PR 中提及其他成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论并启动新会话来处理。由于结果具有非确定性，我们无法提供固定的请求文本。可以要求 Copilot：

- 为每个代码文件添加简短说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 在评论中请求更改，并像提及用户一样标记 **@copilot**。你可以使用以上示例，也可以要求 Copilot 完成其他文档工作。
2. 选择 **View Session**，观察 Copilot 执行任务。注意 Copilot 会启动新会话来进行更新。
3. 选择 **Back to pull request** 返回 PR。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

可以继续迭代，直到满意为止。完成后，将 PR 从草稿转换为准备审查状态，并合并到 main 分支。

![将 PR 转为准备审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决游戏详情页显示相关游戏这一议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或更完整的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的提示。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应在 PR 的检查区域中排队运行。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
9. **可选：** 你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！与之前一样，你可以通过 cloud agent 迭代更新，例如调整显示的相关游戏数量，或要求添加注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义指令）。你可以在 **Conversation** 选项卡中添加评论，Copilot 会收到评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在 mission control 里追加的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或更完整的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 会包含 **[WIP]** 标记。此时请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，特别关注：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保存用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 用于管理模式切换的 JavaScript/TypeScript 代码。
6. 审查更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应在 PR 的检查区域中排队运行。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
10. **可选：** 可以在 Codespace 中切换到此分支，手动测试无障碍功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换功能，并确认页面重新加载后仍保留设置。

注意，自定义无障碍智能体帮助 Copilot 根据无障碍最佳实践实现了这些功能。如果发现任何无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 请求更新。

## 可选练习 — 继续委派任务

Cloud agent 最适合处理真实待办事项，让你能专注于其他工作。可以为存储库创建更多议题并分配给 Copilot，培养这种工作方式。例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 练习环境中的所有课程。通过这些课程，你：

- **检查了存储库自带的自定义指令文件**，以便之后观察其对 cloud agent 输出的影响。
- **将议题分配给 Copilot cloud agent**，并观察它设置环境、制定计划和异步执行任务。
- **创建并使用无障碍自定义智能体**，添加高对比度和浅色模式切换。
- **使用 Copilot agents 页面作为控制中心**，在无障碍任务进行中监控并指导会话。
- **审查并迭代 cloud agent 的拉取请求**，标记 `@copilot` 请求更改并批准工作流。

## 回顾与后续步骤

你已完成 Cloud agent 练习。如果想继续探索，其他练习环境可以拓展本次学习：

- 🖥️ **[VS Code 练习环境](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 练习环境](../../cli/)** — 在终端交付两个经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接。

在自己的存储库中，还可以尝试以下内容：

- 将重构或测试覆盖率议题分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
