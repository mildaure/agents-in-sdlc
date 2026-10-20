---
title: "课程 0 - 先决条件"
description: "创建 Tailspin Toys 的个人副本，并为 Copilot CLI 工作坊准备 GitHub Codespace。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

开始 Copilot CLI 课程前，需要先做好准备。你将创建自己的 Tailspin Toys 存储库副本，并启动一个 [Codespace][codespaces]。下一课会使用其中的集成终端安装并运行 Copilot CLI。

本课程将：

- 从模板创建 Tailspin Toys 项目的个人副本；
- 创建 Codespace 并确认项目已准备就绪。

## 设置实验室存储库

你将在自己的 Tailspin Toys 项目副本中工作。现在从 [Tailspin Toys 模板][tailspin-template]创建存储库。新存储库会包含实验所需的全部文件。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 选择 **Use this template**，然后选择 **Create a new repository**，创建自己的存储库副本。
3. 如果你正在参加由 GitHub 或 Microsoft 主办的活动，请遵循导师提供的说明。否则，请在你有权使用 GitHub Copilot 的组织中创建新存储库。
4. 记下新存储库的路径（`organization-or-user-name/repository-name`），后续工作坊会用到。

> [!NOTE]
> 从模板创建存储库时，系统会自动创建一组 GitHub 议题作为待办事项。整个工作坊都会处理这些议题，无需自行创建。

请使用一份全新的工作坊模板副本。模板中包含存储库说明、应用程序代码、测试、`quality-checks` 技能以及待办事项。如果使用较早的副本，请向活动组织者确认其中包含所需文件。

## 创建 Codespace

接下来将使用 Codespace 完成工作坊。

[GitHub Codespaces][codespaces] 是基于云的开发环境，让你可以直接在浏览器中编写、运行和调试代码。它提供功能完整的编辑器，并支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择 **Code**。
3. 选择 **Codespaces** 选项卡，然后选择 **Create codespace on main**。
4. 等待 Codespace 完成设置。模板会自动安装项目依赖项、Playwright Chromium 和本地数据库。
5. 如果系统询问 **Do you trust the authors of the files in this folder?**，请选中 **Trust Folder & Continue**。
6. 在存储库根目录打开终端并启动应用：

   ```bash
   npm run dev
   ```

7. Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**，确认 Tailspin Toys 网站已加载。
8. 返回终端并按 <kbd>Ctrl</kbd>+<kbd>C</kbd> 停止开发服务器。

## 总结和后续步骤

环境已准备就绪！本课程中，你：

- 从模板创建了 Tailspin Toys 项目的个人副本；
- 创建了 Codespace 并确认项目已准备就绪。

接下来在 Codespace 中[安装 GitHub Copilot CLI][next-lesson]，并使用 GitHub 账户完成身份验证。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
