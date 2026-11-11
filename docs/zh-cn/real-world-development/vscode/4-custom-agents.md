---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

[GitHub Copilot 中的自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域的专用 AI 助手。通过在存储库的 `.github/agents` 文件夹中创建 Markdown 文件来定义智能体，可以为 Copilot 提供专注的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理成可复用的智能体，例如执行 [WCAG][wcag] 合规要求的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹或全局 `~/.copilot/agents` 中的 Markdown 文件定义。每个文件都包含 YAML frontmatter，至少指定 `name` 和 `description`，后续 Markdown 提示词则定义智能体的行为、专业知识和指令。

### 自定义智能体与智能体技能的比较

自定义智能体和[智能体技能][agent-skills-concept]在概念上有些重叠。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体是执行工作的主体，技能是它使用的工具**。

自定义智能体拥有自己的上下文窗口，并负责协调技能（甚至其他智能体）完成工作。本实验中，无障碍自定义智能体会根据无障碍指南审查并更新网站；它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的正确方式。与 AI 的其他用法一样，应在自己的环境和场景中测试并不断迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好的影响。近期用户反馈指出，当前暗色主题的文字与背景颜色对比度不足，部分用户难以阅读。为解决此无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

由于无障碍至关重要，我们希望尽快实现此功能，因此将使用自定义智能体生成相关功能。

本练习将介绍如何：

- 审查现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 查看无障碍自定义智能体

项目已经为你创建了一个无障碍自定义智能体。先查看文件，了解它将如何引导 Copilot。

返回 Codespace，在终端中从 `main` 分支创建用于无障碍工作的独立分支（将练习 3 的筛选 PR 保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览并审查以下章节：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 示例。
   - 常见错误清单。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，该聊天会话会使用其指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，启动新的对话。不同练习所需的模式和模型可能不同，相关步骤会注明要求。

1. 如果 Chat 视图的智能体下拉列表中尚未选中 **Agent**，请先选择它。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉列表（默认显示当前智能体 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示词：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作，探索存储库、提出更改建议并编辑项目文件。每项编辑都会以内联方式显示在聊天中，并包含文件路径和差异供你审查。
6. 随着编辑发生，Chat 顶部的 **Files changed** 指示器会更新，显示智能体修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会修改存储库中的真实文件，包括 Astro 组件、CSS 和相关测试。

下一练习中，你将查看并指导正在进行的工作。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 中的自定义智能体][custom-agents]，也就是针对特定任务和领域设计的专用 AI 助手。自定义智能体可以将团队专业知识和规范整理成可复用的配置，指导 Copilot 更有效地完成特定类型的工作。

本课探索了以下内容：

- 如何定义自定义智能体。
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，你将[监控并指导智能体的工作][next-lesson]，边审查更改边为同一会话添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
