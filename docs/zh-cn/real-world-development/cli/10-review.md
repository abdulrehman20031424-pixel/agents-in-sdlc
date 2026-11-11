---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和后续资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的持续工作流中使用了 GitHub Copilot CLI。你完成了以下工作：

- 准备 Codespace、安装 Copilot CLI、探索项目，并找到预置的筛选议题。
- 添加星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中实现，并在 Interactive 模式中审查。
- 使用自定义指令引导智能体，随后定制现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并在真实浏览器中探索筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，评估需求、覆盖率、技能结果和浏览器证据。
- 审查完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 交付成果

本研讨会包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 分支创建独立分支：

1. **星级评分：** 在游戏卡片上显示现有的 `starRating`，并明确显示尚未评分的状态。
2. **筛选和质量工作流：** 实现筛选，更新指令并将其应用于功能，定制 `quality-checks` 报告，创建 QA 配置，并加入相关测试。

从规划筛选到创建 PR，你始终使用同一个对话和分支。为了简化研讨会，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你通过自动化测试、亲自检查浏览器，以及 Copilot 使用 MCP 探索浏览器等方式检查了代码。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 在 PR 创建前汇总了这些结果，并审查需求和测试覆盖情况。

新增测试应解决实际缺口；QA 运行后不需要新增测试，也可能是合理结果。授权合并前，应审查代码和证据；更改后应更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、自定义了技能、创建了 QA 配置并设置了 MCP 服务器。可以在多个对话中复用这些自定义项，并根据团队需求调整。指令规定标准，技能描述可重复的任务，自定义智能体定义专门角色，MCP 服务器连接外部工具。应审查实际更改和工具结果，而不只是智能体的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 在开始构建前思考方案，使用 **Interactive** 参与范围明确的更改，使用 **Autopilot** 处理定义清晰的任务。常规编辑可选择更快的模型，复杂工作则选择能力更强的模型。

上下文和基础设施同样重要。清晰说明要构建的**内容**、**原因**和**方式**，会显著影响输出。

## 进一步探索

你已完成核心工作流。还可以了解一些有用的 CLI 功能：

- `/review`：让代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获得另一种观点。
- `/fleet`：并行协调相互独立的子任务。
- `/worktree`：隔离一项单独任务。
- `/delegate`：将任务交给 Copilot cloud agent。

## 后续步骤

持续使用任何工具，都是提升技能的最佳方式！可以用它处理生产代码、个人项目，或多年来一直想开发却还没开始的小应用。与团队分享经验，也向团队成员学习。别忘了继续探索文档。

如果想从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot cloud agent 研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
