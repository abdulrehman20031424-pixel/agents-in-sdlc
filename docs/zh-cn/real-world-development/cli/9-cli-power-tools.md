---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云端委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令可以访问高级功能、幕后信息和更多配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。接下来了解其他实用命令。

本课将介绍如何：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索用于并行工作、工作树和可选云端委派的命令。

## 场景

核心 CLI 工作流已完成。现在了解其他功能：管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot cloud agent][about-cloud-agent]。

## 查看 Copilot CLI 上下文

处理大型或复杂任务时，可能会接近模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；你也可以通过斜杠命令自行查看或压缩。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 使用量，以及上下文在系统指令、工具、消息和可用空间之间的分配。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能并不明显。

> [!NOTE]
> 上下文窗口接近上限时，Copilot CLI 会自动压缩上下文。若要自行选择时机，请使用 `/compact`。切换到无关任务并希望开启新对话时，可使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发人员也有不同偏好。Copilot CLI 可列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型及其使用信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作并分享经验，可以帮助大家更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布为 GitHub gist。

1. 输入 `/help`，查看当前版本中的 `/share` 选项。
2. 如果要共享此会话，请选择合适的目标，例如 `/share file`，将会话导出为本地 Markdown 文件。
3. 发送给他人或发布前，请审查导出内容。会话导出可能包含提示词、回复和项目详情。

发布链接或 gist 是可选的。不要发布团队无意公开的存储库或对话内容。

## 可选：扩展并行工作或委派任务

核心研讨会已完成。Copilot CLI 还提供了适用于较大任务的命令：

- `/fleet`：将相互独立的子任务分配给子智能体并行执行。
- `/worktree`：为单独任务创建隔离的 Git 工作树。
- `/delegate`：将任务交给 Copilot cloud agent，由其异步处理并可能创建拉取请求。

这些命令是可选的，因为它们可能创建额外工作树或触发远程工作。请先创建新的、范围明确的任务，再尝试这些命令，并按照常规工作流审查结果。如果想深入了解异步智能体工作，可继续学习 [cloud agent 研讨会][cloud-workshop]。

## 总结与后续步骤

Copilot CLI 中的斜杠命令可以帮助你配置工具、共享会话，并查看幕后工作。本课中，你：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解了 `/share` 如何导出或共享会话。
- 探索了用于并行工作、工作树和云端委派的可选命令。

Copilot CLI 还有更多斜杠命令和功能可供探索！接下来，[回顾已学内容][next-lesson]并了解后续学习方式。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
