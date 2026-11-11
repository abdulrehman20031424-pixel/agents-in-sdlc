---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可在云端异步运行 GitHub Copilot。你在 GitHub 上分配任务后，cloud agent 会在后台接手，探索存储库、进行更改并创建拉取请求，让你可以继续处理其他工作。

通过以下练习，你将添加 cloud agent 会遵循的自定义指令，然后分配 GitHub 议题并让它实现功能。你将审查并使用自定义智能体来引导其工作方式，在智能体仪表板中监控和指导会话，最后审查它创建的拉取请求并迭代优化结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加 cloud agent 会遵循的自定义指令 |
| [2. Cloud Agent][ex2] | 异步智能体 | 将议题分配给 Copilot cloud agent |
| [3. 自定义智能体][ex3] | 专用智能体 | 审查并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控和指导智能体会话 |
| [5. 迭代][ex5] | 审查 | 审查 PR、迭代 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本研讨会前，请确保具备以下条件：

- [ ] 拥有启用 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 已为帐户或组织启用 Copilot cloud agent

> [!TIP]
> 没有付费计划？通过 [GitHub Education][callout-student-plan-education] 验证的学生可以免费使用 GitHub Copilot。**Copilot Student** 计划包含本研讨会使用的智能体、MCP、代码审查和 Copilot CLI 功能，因此可用于完成所有工作环境的练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent 适用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise**，除 Copilot Free 外的所有计划。使用 Copilot Business 或 Enterprise 时，管理员必须为组织启用该功能。

> [!NOTE]
> 本练习环境不涉及 MCP。若想了解如何将 MCP 服务器与 Copilot 搭配使用，请查看 [CLI 练习环境](../cli/)或 [VS Code 练习环境](../vscode/)。

## 开始学习

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
