---
title: "练习 5 - 监视和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

在智能体模式下使用 GitHub Copilot 时，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作会实时在本机执行，VS Code 中的 Copilot Chat 会显示每次工具调用和文件编辑。你可以在聊天中查看每个差异、接受或拒绝单项更改，并通过后续提示引导对话，以完善或扩展工作，无需离开编辑器。

在本练习中，你将：

- 实时监视无障碍智能体的工作；
- 审查并接受建议的文件编辑；
- 在会话进行期间引导智能体，在高对比度模式之外添加浅色模式切换。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作尚未完成时，设计团队又提出了一个要求：还希望添加浅色模式切换，为觉得默认主题太暗的用户提供选择。你不必从头开始新对话，而是可以引导现有会话继续完成这项工作。

## 监视 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和建议的编辑。每项文件编辑都会在聊天中以行内差异形式显示，并提供 **Keep** / **Undo** 控件。

1. 返回练习 4 中打开的 Copilot Chat 视图。
2. 在智能体工作时滚动聊天记录，查看每次工具调用。你会看到文件读取、文件编辑以及智能体请求运行的终端命令。
3. 智能体建议编辑文件时，差异会以内联形式显示：
   - 选择 **Keep** 接受更改；
   - 选择 **Undo** 撤销更改，让智能体再次尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在此会话中修改的全部文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你确认安全的命令。

5. 等待智能体完成高对比度模式的实现。它应该会发送一条总结所做更改的最终消息。

## 引导智能体添加浅色模式

高对比度模式完成后，可以在同一对话中继续添加浅色模式切换。由于使用的是同一聊天会话，智能体会保留之前工作的上下文，包括修改过的组件、所用的样式方法以及已设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 智能体会接续之前的工作，先探索已经进行的更改，然后提出添加浅色模式的修改。
3. 与高对比度模式一样，逐项审查新编辑，并使用 **Keep** 或 **Undo**。
4. 如果智能体的处理方向不符合预期，例如尝试以不合适的方式重构切换 UI，可以选择聊天输入框旁的 **Stop** 按钮停止，然后发送澄清后的后续消息。

> [!TIP]
> 引导正在运行的会话比从头开始更快。可以使用后续提示请求调整（“简化 CSS，改用 CSS 变量，不要重复选择器”）、纠正方向（“不要添加第三个切换控件，将浅色模式集成到现有设置面板中”），或扩展范围（“也为两个切换控件添加键盘快捷键”）。

## 审查工作文件集

提交工作前，快速审查智能体修改的全部文件。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查所有更改文件。应该能看到 Astro 组件、样式和相关测试等内容的更新。
3. 打开几个更改的文件并查看差异。确认其中体现了自定义智能体的无障碍模式，包括 ARIA 属性、键盘导航、语义化 HTML，以及通过本地存储保存设置。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 智能体模式监视并引导了正在进行的会话：逐项审查文件编辑、接受或拒绝更改，并发送后续提示来扩展工作，同时保留上下文。

本课程介绍了以下概念：

- 实时监视智能体的工作；
- 接受和拒绝单项文件编辑；
- 在会话进行期间通过后续提示引导工作。

在[下一课][next-lesson]中，你将把无障碍功能作为拉取请求发布，并审查本实验中本地自定义智能体完成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代完善 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
