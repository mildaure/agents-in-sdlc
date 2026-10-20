---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]可以创建专门的 AI 助手，用于开发工作流中的特定任务或领域。在存储库的 `.github/agents` 文件夹中通过 Markdown 文件定义智能体后，可以为 Copilot 提供针对性的说明、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识编码为可复用的智能体，例如用于执行 [WCAG][wcag] 合规检查的无障碍智能体、遵循安全编码实践的安全智能体，或用于维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局目录 `~/.copilot/agents` 中定义。每个文件都包含 YAML front matter，其中至少要有 `name` 和 `description`，之后是用于定义智能体行为、专业知识和说明的 Markdown 提示。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]在概念上有一些重叠。两者主要通过 Markdown 文件定义，并告知 AI 如何执行操作。最简单的区分方式是：**自定义智能体**是工作者，**技能**是工具。

自定义智能体拥有自己的上下文窗口，并且可以在工作过程中编排技能（甚至其他智能体）。本实验中，无障碍自定义智能体会依据无障碍指南审查并更新网站；在此过程中，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 自定义智能体没有唯一正确的编写方式。与 AI 的其他用法一样，应在不同环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，由于文字和背景颜色的对比度不足，一些用户觉得当前深色主题难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

由于无障碍功能非常重要，你希望尽快实现它，因此将使用自定义智能体生成此功能。

在本练习中，你将：

- 审查现有的无障碍自定义智能体；
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 审查无障碍自定义智能体

已经为你创建了一个无障碍自定义智能体。先审查其内容，了解它会如何引导 Copilot。

返回 Codespace，打开终端，并从 `main` 创建用于无障碍工作的新分支（将练习 3 的筛选功能 PR 与此分开）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML front matter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` front matter。

3. 浏览并审查后续部分，其中包括：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见问题和错误列表。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。可以选择自定义智能体，让聊天会话遵循该智能体的说明和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，先返回 Codespace 并打开 Copilot Chat 面板，然后选择 **New Chat** 开始新的对话。各练习所用的模式和模型可能不同，相关步骤会在需要时说明。

1. 如果智能体下拉列表中尚未选择 **Agent**，请先选择它。

   ![显示 Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 选择聊天视图底部的智能体下拉列表（其中显示当前使用的智能体；默认值为 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会开始工作：探索代码库、提出更改方案并编辑项目文件。每项编辑都会在聊天中以行内形式显示，包括文件路径和差异，供你审查。
6. 随着编辑进行，**Files changed** 指示器会更新，显示智能体修改的文件。

> [!NOTE]
> 这可能需要几分钟。Copilot 正在对存储库进行实际更改，会编辑 Astro 组件、CSS 和相关测试等现有文件。

下一练习将审查并引导这项正在进行的工作。

## 总结和后续步骤

本课程介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，这类智能体是面向特定任务和领域的专用 AI 助手。使用自定义智能体，可以将团队的专业知识和标准编码为可复用智能体，更有效地引导 Copilot 完成特定类型的工作。

本课程介绍了以下概念：

- 如何定义自定义智能体；
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来将[监视并引导智能体的工作][next-lesson]，审查正在进行的更改，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [Awesome Copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监视和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
