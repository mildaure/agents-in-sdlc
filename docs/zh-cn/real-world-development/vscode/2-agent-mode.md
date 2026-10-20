---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发人员的工作流程通常包括：找到所有相关文件、进行更改、运行测试、调试、找出遗漏的文件、再次更新……这样的工作还会持续下去。

这正是 Copilot 智能体模式可以发挥作用的地方。

Copilot 智能体模式的设计目标是在 IDE 中更自主地工作。它的行为类似开发人员：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。现在来了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但适用于两种不同的体验。智能体模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解如何：

- 使用 Copilot 智能体模式探索项目、识别相关文件并协调更改；
- 使用 GitHub Copilot 智能体模式在 UI 和数据层中实现新功能；
- 在将代码合并到代码库前，审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望让用户按类别和发行方筛选。上一练习已经添加了 publishers helper，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行更改前，先探索 Tailspin Toys 网站，了解现有功能。

这是一个以开发人员为主题的棋盘游戏众筹平台。用户可以浏览游戏列表并查看详情。网站是一个单体 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API，也没有客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能则通过使用标准 DOM API 的小型作用域 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器，方便运行网站。按以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。完成上一练习后，你应该已切换回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到用于筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似下面的 URL 横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，提示为 `http://localhost:4321` 打开浏览器窗口，请选择 **x** 关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中，从终端选择 localhost 链接会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是用于连接当前 Web 服务器的 Codespace 私有隧道。

### 探索网站

网站启动后，可以浏览现有功能。网站的主要功能包括：

- **主页**：显示棋盘游戏列表，包括标题、图像和说明。
- **游戏详情页**：选择游戏后会打开详情页，其中包含标题、说明、发行方和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，先返回 Codespace 并打开 Copilot Chat 面板，然后选择 **New Chat** 开始新的对话。各练习所用的模式和模型可能不同，相关步骤会在需要时说明。

网站的初始实现已经可以运行，但还可以添加更多功能。先查看待办事项。根据模板创建项目时，系统已经自动创建了一组议题；让 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示 Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并未偏好某个模型。制作本工作坊时使用的是 Claude Sonnet 4.5，因此在说明中列出了该模型，希望代码建议能相对一致并提供良好体验。不过，LLM 的输出具有概率性，因此你收到的建议可能与工作坊示例不同，这是正常且预期的情况。

> [!NOTE]
> 由于 LLM 输出具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示，询问存储库中的待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 审查生成的议题列表。

注意，Copilot 还根据预计对用户的价值为议题排定了优先级。

## 审查说明文件

让智能体生成代码前，先查看将用于向 Copilot 提供上下文的说明文件。本练习将使用[用户界面（UI）说明](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看其中有关如何添加功能的总体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍原则；
   - 指向特定文件类型说明的链接，包括：
     - Astro；
     - Tailwind CSS；
     - Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置。这样可以复用内容、将复杂指导拆分为更小的部分，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，应用至少需要进行三项独立更新：

- 在数据层（`src/lib/`）添加或完善剩余的筛选逻辑；
- 添加或更新筛选行为测试；
- 更新游戏列表页面，添加筛选 UI。

此外，在合并之前必须运行测试并确保全部通过。Copilot 智能体模式可以代为完成这些工作，现在开始添加功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui**，将其设为说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选择 **Agent**。

   ![显示 Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 查看 Copilot 如何先探索项目并查找与功能相关的文件。它应该会找到数据层 helper、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项任务，例如探索项目、修改文件和运行测试。根据任务复杂度和代码库规模，可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 按钮，因此工作进行期间无需选择它们。

7. 根据 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中询问是否运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。这没有问题！Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且不再检测到错误。

   ![完整的 Copilot 智能体模式 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认一切正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜你！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。你了解到：

- GitHub Copilot 智能体模式可以在 UI 和数据层中实现新功能；
- Copilot 智能体模式可以探索项目、识别相关文件并协调更改；
- 应在将代码合并到代码库前审查 Copilot 智能体模式生成的更改和测试。

接下来使用 Playwright MCP 服务器[测试功能][next-lesson]并创建拉取请求。

### 奖励探索练习 — 实现分页

随着游戏列表不断增长，需要支持分页。运用本练习中学到的技能，提示 Copilot 更新网站并实现分页。编写代码时可以考虑：

- 遵循现有最佳实践，包括使用现有说明文件；
- 考虑如何实现分页，例如允许用户选择每页大小，还是固定每页大小；
- 编写提示时，为 Copilot 提供必要指导，以便它按预期实现功能；
- 可能需要与 GitHub Copilot 迭代，提出修改请求并提供上下文。这是使用 Copilot 的正常工作流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：作用及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
