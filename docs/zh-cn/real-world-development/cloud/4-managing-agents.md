---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前两项练习中，你让 Copilot cloud agent 承担了三项不同任务，用于改进用户体验和添加功能。Cloud agent 虽然可以异步自主工作，但监控任务进展仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心可以查看所有拥有未关闭拉取请求 (PR) 的智能体任务，查看它执行的操作，甚至可以指导正在进行的会话。

本课将介绍如何：

- 使用智能体页面监控 cloud agent 任务。
- 在会话进行中指导智能体添加功能。

## 场景

分配高对比度模式任务后，团队意识到也应该添加浅色模式。网站样式和切换功能正在更新，正好可以一并实现。你将指导智能体，让它同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的任务状态。

1. 转到 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码。
    - 创建 Copilot 工作环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的各项步骤。

> [!NOTE]
> 列出的具体步骤会因 Copilot 的工作进度和采用的方案而异。

4. 同时查看右侧显示的 PR 面板，以便监控 PR 和更改的文件。

## 指导 cloud agent

现在已了解当前任务情况，接下来要求 Copilot 在实现高对比度模式的同时，也添加浅色模式切换功能。

1. 选择有关添加高对比度模式的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![Mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它完成环境设置并开始工作。出现类似下方的消息时，就表示已经开始。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示词：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中突出显示 Steer active session while Copilot is working 对话框的截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 注意 Copilot 会确认收到提示词，并将它加入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！它会先完成发送消息时正在执行的步骤，再将新请求加入工作流程。

这可能需要几分钟。等待时，可以回顾并思考目前学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot agents 页面，它是监控和指导 GitHub Copilot cloud agent 任务的中心位置。通过这个控制中心，你可以跟踪所有进行中和已完成的任务、审查工作内容，甚至调整正在进行任务的范围或提供额外指导。

本课探索了以下内容：

- 使用控制中心和智能体页面监控 cloud agent 任务。
- 调整进行中的会话以请求添加功能。

Copilot 正在完成无障碍功能，接下来可以[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
