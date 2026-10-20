---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求前，应在浏览器中确认功能确实可以运行。无需亲自点击应用进行测试，可以连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器并为你测试功能，然后发布分支并创建 PR。

在本练习中，你将：

- 了解模型上下文协议（MCP）是什么，以及 MCP 服务器如何使用新工具扩展 Copilot；
- 将 Playwright MCP 服务器添加到工作区；
- 让 Copilot 使用该服务器，在浏览器中手动测试筛选功能；
- 发布分支并为筛选功能创建拉取请求。

## 什么是模型上下文协议（MCP）？

当智能体模式可以访问编辑器之外的工具时，功能会强大得多。模型上下文协议（MCP）支持 Copilot 与这些工具交互，是智能体与外部工具和服务通信的标准方式。

![显示智能体模式内部工作方式及其与上下文、LLM 和工具的交互的示意图，其中包括由 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)提供了一种让 AI 智能体与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时访问最新信息，并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具、服务之间的桥梁，负责管理 AI 智能体与外部工具（如现有 API 或 NPM 包等本地工具）之间的通信。每个 MCP 服务器都提供一组智能体可以访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库的 API，让 AI 智能体可以创建存储库、更新现有存储库、管理议题和拉取请求等。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能，让 AI 智能体可以访问网页、填写表单和选择按钮等。

还有许多其他 MCP 服务器，提供不同的工具和资源。GitHub 托管了 [MCP 注册表](https://github.com/mcp)，帮助社区发现并贡献 MCP 服务器。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样谨慎使用 MCP 服务器。使用前请仔细审查其源代码、验证发布者并评估安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 审查 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 可以看到已经配置好的 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

项目模板中已包含 `github` 条目，因此上一练习中 Copilot 才能读取待办事项议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容。VS Code 会通过 HTTP 连接服务器，并在 Copilot 首次使用其工具时让你通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好可以用于测试新功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列，使文件内容如下：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是一个**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 参数表示 Playwright 在没有可见窗口的情况下运行浏览器。这适用于没有桌面环境可显示窗口的 Codespace。

> [!NOTE]
> Tailspin Toys 项目已经使用 Playwright 运行端到端测试，因此通常已安装浏览器。如果 Copilot 之后报告缺少浏览器，让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需启动** MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问你是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选步骤。如果跳过，VS Code 会在下一步中 Copilot 首次需要相应工具时启动服务器，并提示你信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的真实浏览器。无需亲自点击应用检查工作，智能体可以打开页面、导航、应用筛选条件并读取结果，然后总结观察到的内容。无需离开对话即可快速确认功能是否符合预期。

Playwright MCP 服务器实际上是根据页面的[无障碍树][playwright-mcp-server]而非屏幕截图运行的。这意味着智能体会像辅助技术一样，依据结构化且带标签的元素（按钮、链接和列表项）进行判断。因此，快速功能检查也可以作为简单的无障碍检查。

服务器已连接且应用正在运行后，让 Copilot 测试刚刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐步执行检查并报告结果。根据议题中的验收标准审查报告；如果发现问题，可以提出后续问题，或让 Copilot 在创建拉取请求前修复代码。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请先重新启动，再发送提示。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器。如果它报告缺少浏览器，让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板并审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示更改列表的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

创建拉取请求有多种方式，包括通过 github.com 和 GitHub 命令行界面（CLI）操作。不过，既然已经在使用 GitHub Copilot，不如让它代为创建 PR！它可以查找相关议题，并创建与议题关联的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新的会话。
2. 在智能体下拉列表中选择 **Agent**，确保 Copilot 可以使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，系统可能会提示你进行 GitHub 身份验证；按照提示完成操作。
5. 注意 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接查看拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！在本练习中，你：

- 了解模型上下文协议（MCP）是什么，以及 MCP 服务器如何使用新工具扩展 Copilot；
- 将 Playwright MCP 服务器添加到工作区；
- 使用该服务器在浏览器中手动测试筛选功能；
- 发布分支并创建筛选功能拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
