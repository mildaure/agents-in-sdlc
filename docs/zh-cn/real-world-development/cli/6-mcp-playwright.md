---
title: "课程 6 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是编写代码。还需要处理数据、外部服务，甚至让 Copilot 使用额外的自动化工具。MCP 服务器可以扩展 Copilot CLI 中内置的功能，为它提供更多工具和服务。

本课程将：

- 了解模型上下文协议（MCP）是什么，以及 Copilot CLI 如何使用它；
- 如果尚未配置 Playwright MCP 服务器，则添加该服务器；
- 让智能体驱动浏览器并体验筛选功能。

## 什么是模型上下文协议（MCP）？

[模型上下文协议（MCP）][mcp-blog-post]让 AI 智能体能够与外部工具和服务通信。借助 MCP，AI 智能体可以实时访问最新信息并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具、服务之间的桥梁。每个 MCP 服务器都提供一组智能体可以访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，帮助社区发现并贡献 MCP 服务器。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样谨慎使用 MCP 服务器。使用前请审查源代码、验证发布者并评估安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，可以添加它，让 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已经配置了 Playwright MCP，请跳过这些设置步骤并继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 体验功能

之前你已手动确认功能按预期运行，现在使用 Copilot 可访问的 Playwright MCP 服务器完成同样的操作。

1. 使用以下提示，让 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，它会自行判断。不过，既然你知道它应该使用什么工具，明确指出也有帮助。这样可以让结果更一致，还能节省一些 token。

2. 查看 Copilot 在浏览器中执行的步骤，确认功能是否正常。
3. 阅读报告，确认各项行为符合预期。

Copilot 会启动服务器，通过 Playwright 与网站交互，停止服务器并提供报告。

## 总结和后续步骤

恭喜你从 Copilot CLI 中使用 Playwright MCP 服务器在真实浏览器中体验了功能！回顾一下，你已经：

- 了解模型上下文协议（MCP）是什么，以及 Copilot CLI 如何使用它；
- 如果尚未配置 Playwright MCP 服务器，则添加该服务器；
- 让智能体驱动浏览器并体验筛选功能。

接下来创建一个 [QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专门的角色中。

## 资源

- [MCP 是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
