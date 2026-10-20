---
title: "练习 4 - 监视和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云智能体处理了三个不同的任务，用于改善用户体验并添加功能。云智能体可以异步自主运行，但监视这些任务仍然很重要。

你可以使用多种工具来管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心可以查看所有带有未关闭拉取请求（PR）的智能体任务。你可以查看已执行的操作，也可以引导正在运行的会话。

在本课程中，你将：

- 浏览智能体页面，监视云智能体任务；
- 引导正在运行的会话，请求添加其他功能。

## 场景

将实现高对比度模式的任务分配给智能体后，团队意识到此时也适合添加浅色模式。既然已经在更新网站样式并添加切换功能，把浅色模式也包括进来就很合理。你将引导智能体，确保它除了高对比度模式外也添加浅色模式。

## 审查 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应该能看到已分配给 Copilot 的任务，例如：
   - 更新代码文档；
   - 为修改游戏生成 API；
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，审查 Copilot 已执行的操作。这些操作可能包括：
   - 从存储库检出代码；
   - 为 Copilot 创建环境；
   - 设置 MCP 服务器；
   - 执行完成所分配任务的各项步骤。

> [!NOTE]
> 列出的具体步骤会因 Copilot 的工作状态和所采用的方法而异。

4. 同时留意右侧显示的拉取请求（PR）窗格。可以在此查看 PR 和更改的文件，以便进一步监视。

## 引导云智能体

现在已经查看了正在进行的任务，接下来让 Copilot 在实现高对比度模式时也添加浅色模式切换功能。

1. 选择与添加高对比度模式有关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 监视会话几分钟，直到状态显示 Copilot 已完成设置并开始工作。出现类似下方的消息时，就表示已开始处理。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中的云智能体任务截图，其中高亮显示“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会开始处理更新后的任务！发送消息时，Copilot 会先完成正在进行的具体步骤，然后将新请求加入工作流程。

与之前一样，这需要几分钟。此时可以暂停一下，回顾迄今为止学到和探索的内容。

## 总结和后续步骤

本课程介绍了 Copilot 智能体页面，它是监视和引导 GitHub Copilot 云智能体任务的中心位置。在这个任务控制中心，你可以跟踪所有正在进行和已完成的任务、审查智能体正在执行的工作，还可以重定向正在运行的任务以调整范围或提供其他指导。

本课程介绍了以下概念：

- 使用任务控制中心和智能体页面监视云智能体任务；
- 重定向正在运行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能的实现；接下来审查[它创建的拉取请求并进行迭代][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代完善 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
