---
title: "练习 3 - 自定义智能体"
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

通过自定义智能体，你将了解：

- 如何定义自定义智能体；
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，由于文字和背景颜色的对比度不足，一些用户觉得当前深色主题难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

由于无障碍功能非常重要，你希望尽快实现它，因此将使用自定义智能体生成此功能。

## 审查无障碍自定义智能体

已经为你创建了一个无障碍自定义智能体。先审查其内容，了解它会如何引导 Copilot。

返回 Codespace，然后审查无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML front matter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` front matter。

3. 浏览并审查后续部分，其中包括：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见问题和错误列表。

## 创建并分配议题

任务控制中心是管理环境中所有智能体的中心位置。你可以在此向 Copilot 云智能体分配任务、监视任务，还可以重定向任务并提供其他指导。现在，先让 Copilot 使用自定义智能体实现高对比度模式。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

   ![云智能体分配截图，其中高亮显示自定义智能体和 Accessibility](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务。

## 总结和后续步骤

本课程介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，这类智能体是面向特定任务和领域的专用 AI 助手。使用自定义智能体，可以将团队的专业知识和标准编码为可复用智能体，更有效地引导 Copilot 完成特定类型的工作。

本课程介绍了以下概念：

- 如何定义自定义智能体；
- 如何将任务分配给自定义智能体。

Copilot 正在实现高对比度模式；接下来转到任务控制中心，[监视并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做好准备][org-custom-agents]
- [为企业使用自定义智能体做好准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监视和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
