---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

很少有组织完全没有技术债务。这些债务可能是尚未解决的安全问题、需要更新的旧代码，或因为一直没有时间实现而滞留在待办事项中的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。智能体完成工作后，会创建一个草稿 PR，供开发人员审查。这样可以将繁琐的任务委派出去、加快开发流程，让开发人员专注于更重要的工作。

你将探索如何使用 Copilot 云智能体：

- 自定义代码生成环境；
- 确保操作安全执行；
- 理解清晰定义议题范围的重要性；
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受雇开发网站的承包商没有妥善处理文档，实际上，代码完全缺少文档。团队希望先为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队准备改善游戏发现体验。他们希望在每款游戏的详情页面上显示相关游戏，也就是同一类别中的其他作品，让用户可以继续浏览。此时不需要完善的实现；团队只需要可以用于 UX 验收测试的初步功能。目前这项任务受其他优先级更高的议题阻塞。

这些任务很容易被推迟，很适合分配给 Copilot 云智能体。云智能体可以在后台异步处理，让开发人员先完成其他工作，之后再回来审查 Copilot 的结果并确认符合预期。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像人类开发人员一样在后台执行任务。与人类开发人员协作的方式类似，你可以通过多种方式使用它，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建一个草稿拉取请求来跟踪进度，设置环境并开始处理任务。会话仍在运行时或完成后，你都可以查看 Copilot 的会话。解决方案准备好供你审查时，Copilot 会在拉取请求中标记你。

## 明确定义任务范围的重要性

GitHub Copilot 没有魔法，使用它并不意味着可以跳过思考。看似简单的操作深入分析后也可能很复杂，因此[应谨慎定义 Copilot 云智能体任务的范围][cloud-agent-best-practices]。将它视为 AI 结对程序员：分阶段开展工作、学习、试验并持续调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其引导 Copilot 的方式。如果已经完成自定义说明练习，就会看到 `.github/copilot-instructions.md` 的实际效果；否则，现在可以快速查看。在向 Copilot 云智能体分配任务前，先只读查看此存储库中的说明文件，以便之后观察它们的影响。

在 GitHub Web UI 中打开以下文件；如果已启动 Codespace，也可以在其中查看：

- `.github/copilot-instructions.md` — 查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md` — 注意其 `applyTo` front matter，它将这些说明限定为适用于 `**/*.test.ts` 文件。

在下一部分将 `Code lacks documentation` 议题分配给云智能体时，留意生成的 PR 中是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。它们来自这些说明文件。稍后的练习会再次介绍审查 PR 时需要关注的内容。

## 设置 Copilot 云智能体的开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保一切就绪。将任务分配给 Copilot 时也是如此，因为它的工作方式类似软件工程师（SWE）。

云智能体使用 [GitHub Actions][github-actions] 环境执行任务。可以创建一个[专用设置工作流][setup-workflow]，配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，让它在云智能体开始工作前运行，从而为它提供所需的开发工具和依赖项。本实验已经预先配置了此工作流，帮助课程顺利进行。它会确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用中已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可以从存储库的“Actions”选项卡手动测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设置为步骤所需的最低级别。Copilot 会获得自己的操作令牌。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 contents: read 权限。如果不在设置步骤中克隆存储库，Copilot 会在设置步骤完成后自动执行此操作。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

它看起来与其他 GitHub 工作流文件相同，但有几个要点需要注意：

- 其中包含名为 `copilot-setup-steps` 的单个作业。在 Copilot 开始处理拉取请求前，GitHub Actions 会先运行此作业。
- 注意 `workflow_dispatch` 触发器，它允许你从存储库的 **Actions** 选项卡手动运行工作流。这样可以在等待 Copilot 运行前先测试工作流是否成功。

## 添加文档

虽然所有人都知道文档的重要性，但大多数项目不是文档过时，就是完全缺少文档。这种技术债务往往一直得不到处理，从而降低工作效率，并使代码库维护或新开发人员入职更加困难。Copilot 擅长生成文档，因此这是分配给 Copilot 云智能体的理想议题。它会在后台生成所需文档。稍后的练习会回来审查其工作。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

   ![Copilot 议题分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的 PR，标题应类似于 `[WIP]: Code lacks documentation`。如果列表中尚未显示新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应该会创建一个待办事项列表。

> [!NOTE]
> PR 中可能需要几分钟才会显示 Copilot 创建的待办事项列表。Copilot 正在创建环境（运行前文介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 审查待办事项列表及 Copilot 将要完成的任务。
14. 向下滚动 PR 时间线，应该会看到 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 查看实时会话，了解 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并确定合适的计划，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是让它异步执行任务，使我们可以专注于其他工作。现在可以利用这项能力：再分配一个任务给 Copilot 云智能体，然后转而编写应用程序代码，添加新功能。

## 在游戏详情页面添加相关游戏

如前所述，GitHub Copilot 云智能体的一大优势是能够拆分工作，你可以处理一组任务，同时让它处理另一组任务。为设计团队添加相关游戏部分可能不需要很长时间，但也可以把这段时间用来完成其他任务。现在将此任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
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

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，你应该会看到议题的首条评论中出现一组 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理任务](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理第二个请求！Copilot 云智能体的工作方式与软件工程师类似，因此无需持续监视；等工作完成后再审查即可。现在将注意力转向创建和使用自定义智能体。

## 总结和后续步骤

本课程介绍了 [GitHub Copilot 云智能体][copilot-agents]。使用云智能体可以将议题分配给 Copilot，让它异步处理工作。你可以使用 Copilot 处理技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

本课程介绍了以下概念：

- 自定义代码生成环境；
- 确保操作安全执行；
- 明确定义议题范围的重要性；
- 将议题分配给 Copilot。

云智能体正在后台工作，现在可以转向创建和使用自定义智能体。[Copilot 云智能体还可以使用 MCP 服务器][cloud-agent-mcp]，并且可以使用之前模块中介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

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
