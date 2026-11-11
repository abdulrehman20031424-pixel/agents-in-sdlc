---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会受到技术债务困扰，例如未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot cloud agent 可自主执行更新代码和添加功能等任务。任务完成后，它会创建草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发进度，让开发人员专注于更重要的工作。

本练习将介绍如何使用 Copilot cloud agent：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有提供完善的文档，事实上，代码完全没有文档。团队希望首先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改善游戏发现体验。他们希望每款游戏的详情页显示相关游戏，也就是相同类别的其他游戏。目前不需要精致的实现，只需要可用于 UX 验收测试的版本。此任务目前受阻，但其他议题优先级更高。

这些任务很容易被降级到待办列表后面，因此适合交给 Copilot cloud agent。它可以异步处理任务，让开发人员继续完成其他工作，之后再回来审查 Copilot 的成果。

## 了解 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发人员一样在后台执行任务。你可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求跟踪进度、设置环境并开始工作。会话进行中或完成后，你都可以查看。解决方案准备好供你审查时，Copilot 会在拉取请求中提及你。

## 明确界定指令范围的重要性

GitHub Copilot 没有魔法，不能省略思考过程。即使看似简单的操作，也可能包含复杂细节，因此[应谨慎界定 Copilot cloud agent 的任务范围][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段工作、学习、尝试并不断调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果你已完成自定义指令练习，就已看到 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。将工作分配给 Copilot cloud agent 前，先只读查看存储库中的指令文件，稍后便能观察其影响。

在 GitHub 网页界面或正在运行的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 章节，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定在符合 `**/*.test.ts` 的文件。

下一节将 `Code lacks documentation` 议题分配给 cloud agent 时，请留意拉取请求中的 TSDoc 文档注释、TypeScript 规范和注释头。它们来自这些指令文件，后续练习还会再次介绍。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要准备特定环境并运行设置脚本，以确保环境就绪。将任务分配给 Copilot 时也是如此，因为它会以类似软件工程师的方式工作。

Cloud agent 使用 [GitHub Actions][github-actions] 作为工作环境。你可以通过 `.github/workflows/copilot-setup-steps.yml` 中的[专用设置工作流][setup-workflow]自定义环境，让它能使用所需开发工具和依赖项。项目已预先配置此工作流，以支持实验并提供学习机会。它会确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用所需的迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件类似，但有几点值得注意：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 工作流使用 `workflow_dispatch` 触发器，因此可以在存储库 **Actions** 选项卡中手动运行，测试工作流是否成功，而不必等待 Copilot 启动。

## 添加文档

虽然人人都知道文档很重要，但大多数项目中的文档要么过时，要么缺失。这类技术债务常被搁置，降低工作效率，也增加维护存储库或帮助新开发人员加入团队的难度。Copilot 擅长创建文档，因此很适合把此议题交给 Copilot cloud agent。它会在后台生成所需文档，你将在后续练习中审查结果。

1. 在新的浏览器选项卡中访问 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求 (PR)，其标题类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才能在 PR 中创建待办事项列表。它需要准备环境（运行前面介绍的工作流）、分析项目并确定处理问题的方案。

13. 查看 Copilot 即将完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 查看实时会话，了解 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并确定合适计划，以及编写代码。

这可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让我们可以专注于其他工作。接下来利用这一点：先向 Copilot cloud agent 分配另一项任务，然后编写代码，为应用添加功能。

## 在游戏详情页添加相关游戏

如前所述，GitHub Copilot cloud agent 的一大优势是可以拆分工作：你专注于一组任务，它处理另一组。即使为设计团队添加相关游戏部分不一定需要很长时间，交给智能体处理也能节省时间。现在将任务分配给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，你会在议题的第一条评论中看到一组 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求！它的工作方式类似软件工程师，无需主动持续监控，完成后再审查即可。现在可以创建并使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。可以将议题分配给 cloud agent，让它异步处理任务。你可以用 Copilot 处理技术债务、创建新功能，或协助从一个框架迁移到另一个框架。

本课探索了以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

Copilot cloud agent 正在后台工作，现在可以开始创建和使用自定义智能体。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并且能够读取前面模块介绍的自定义指令。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
