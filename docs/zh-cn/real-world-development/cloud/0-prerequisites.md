---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot cloud agent 练习前，需要完成准备工作。你将创建 Tailspin Toys 存储库的副本，并启动一个 [Codespace][codespaces]，用于编辑指令文件和审查 cloud agent 的工作。

## 设置实验存储库

通过[模板存储库][template-repository]创建自己的代码副本。新的存储库包含实验所需文件，你将在整个练习中使用它。

1. 在新的浏览器窗口中，转到 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面选择 **Use this template** 按钮，然后选择 **Create a new repository**。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果在 GitHub 或 Microsoft 主办的活动中参加本研讨会，请遵循导师提供的说明。否则，可在有权使用 GitHub Copilot 的组织中创建新存储库。

    ![填写存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下所创建的存储库路径 (**organization-or-user-name/repository-name**)，后续实验会用到该路径。

## 创建 Codespace

接下来，使用 Codespace 完成本实验。

[GitHub Codespaces][codespaces] 是一种基于云的开发环境，可直接在浏览器中编写、运行和调试代码。它提供功能完整的 IDE，支持多种编程语言、扩展和工具。

1. 转到刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 按钮创建新的 Codespace。

    ![创建新的 Codespace](../../../_images/ex0-create-codespace.png)

创建 Codespace 需要几分钟，但比手动安装所有服务快得多。等待期间，可以探索 GitHub Copilot 的其他功能，接下来就会介绍这些内容。

> [!CAUTION]
> 后续练习会再次使用此 Codespace。目前请在浏览器选项卡中保持它打开。

> [!NOTE]
> 本研讨会可在 Codespace 或本地 [dev container][dev-containers] 中运行。两者都已安装所需的先决条件。如果想在本地运行，请在 VS Code 中打开克隆的存储库，并在收到提示时选择 **Reopen in Container**。VS Code 将构建与 Codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

恭喜，你已创建实验存储库的副本，并开始创建 Codespace。后续练习会在其中配合 Copilot cloud agent 开展工作。

## 下一步

接下来，为 cloud agent 添加自定义指令。继续学习[练习 1 - 自定义指令][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [通过模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
