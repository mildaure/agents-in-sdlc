---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能够访问这些上下文。有多种方式可以与 Copilot 分享特定上下文，其中重要的一种是[说明文件][instruction-files]，可用于指导代码生成。

在本练习中，你将：

- 了解如何通过存储库自定义说明和按路径限定范围的说明文件，为 Copilot 提供项目上下文、编码准则和文档标准；
- 在应用当前说明的情况下，生成筛选功能的第一块数据切片（publishers helper）；
- 向 `.github/copilot-instructions.md` 添加一项新的全库标准；
- 重新运行相同提示，查看生成的代码如何遵循新标准；
- 将说明更新和筛选功能切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能与部分标准不完全一致。Copilot 的输出具有非确定性。本练习的目标是观察更新说明后行为变化的*趋势*，而不是要求输出逐字符完全一致。

## 说明文件

### 场景

和优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式，并具有现代感。
- 应使用 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应有一段注释，说明文件的用途。

使用说明文件可以确保 Copilot 获得所需信息，并按照这些实践要求完成任务。

### 自定义说明

自定义说明可以为 Copilot 提供上下文和偏好，帮助它更好地理解编码风格和需求。这项功能可以引导 Copilot 给出更相关的建议和代码片段。你可以指定偏好的编码约定、库，甚至希望添加的注释类型。可以为整个存储库创建说明，也可以针对特定文件类型创建说明，为特定任务提供上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：单个说明文件，针对该存储库的**每个**请求发送给 Copilot。此文件应包含项目级信息，也就是大多数发给 Copilot 的聊天或 CLI 请求都会用到的上下文，例如所用技术栈、项目概述、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的说明。可以为特定语言（例如 TypeScript 或 Astro）提供指导，也可以为创建 UI 组件或新增一组单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中工作时，说明文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体生成代码时，都会使用存储库级说明文件以及带有 `applyTo` front matter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [也支持采用其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理说明文件的最佳实践

全面讨论如何创建说明文件超出了本工作坊的范围。不过，示例项目中的文件展示了一种有代表性的做法。概括来说：

- 将 `copilot-instructions.md` 中的内容聚焦于项目级指导，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（如单元测试、Astro 组件和数据层）或特定任务提供具体说明。
- 使用自然语言并清楚表达指导，同时提供代码应该如何编写以及不应该如何编写的示例。

创建说明文件没有唯一正确的方法，使用 AI 也一样。通过不断尝试，你会找到最适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的说明文件。浏览本项目中的文件时，你可能会发现它们覆盖了许多任务类型，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。各个界面提供此功能的方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。所在工作环境的课程会在相关位置介绍这些方式。
>
> 想寻找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中汇集了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义说明文件

花些时间阅读此存储库附带的说明文件：一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意其中的项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要注意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指导。这些指导适用于你与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览。可以看到其中包含针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了相对于存储库根目录的 glob，用于确定这些说明适用于哪些文件。此处，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会匹配。
5. 查看其中针对本项目单元测试的专门说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中链接到了其他说明文件（如 `unit-tests.instructions.md`）以及项目中的现有文件。这样可以将较大的说明集拆分成更小、可复用的文件，并在生成代码时为 Copilot 指出可供参考的示例。（其中的路径相对于说明文件本身，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但尚未要求在代码中添加文档。接下来的步骤会添加 TSDoc 文档注释和文件注释头规则。

## 为更改创建分支

按照最佳实践，为更改创建一个分支。

1. 返回上一练习中打开的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明*之前*使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，先返回 Codespace、打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。不同练习使用的模式和模型可能不同，相关步骤会在需要时说明。

要观察自定义说明的影响，先在当前说明文件生效的情况下发送一个提示。之后更新说明文件，并再次发送相同提示以查看差异。

1. 关闭之前练习中打开的编辑器选项卡，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应该放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 可以应用文件更改。

   ![显示 Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改 helper 文件及其测试。
6. 注意建议的 helper 是一个带类型的函数，以 `db` 客户端作为第一个参数，并返回带类型的 publishers 数组——这是 `.github/instructions/drizzle.instructions.md` 中的数据层约定（该文件适用于 `src/lib/*.ts`）。
7. 注意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有非确定性，即使没有明确要求，它也可能添加文档注释。如果发生这种情况也没关系；更新说明后*一致性*提升仍然是本练习的重点。

## 添加新的存储库级标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为它补充存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，位置应在第 27 行附近。注意其中已经记录了项目编码标准，但还没有代码内文档规则；这就是生成的 helper 缺少文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别创建说明文件（`.github/copilot-instructions.md`）提供全局指导，也可以创建 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。刚添加的文档注释规则属于项目级标准，适合放在存储库级文件中。

## 重新运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清空对话。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 专注于正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意建议的文件现在会以类似下面的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意建议的函数现在会包含类似下面的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已经引导 Copilot 遵循新的项目标准，并将其应用到后续练习会继续扩展的真实代码中。

## 提交说明并推送分支

说明文件与存储库中的其他资源一样，都通过版本控制管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认说明更新和 helper 更改已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新，以及 Copilot 为 helper 基础更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，应创建拉取请求并关联待办事项中的文档标准议题。也可以手动操作，但 Copilot 可以使用已连接到项目的 GitHub 工具代为完成。让 Copilot 查找议题、创建用于关闭该议题的 PR，然后合并它。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上执行操作。项目模板已将 GitHub 工具连接到工作区，无需额外设置。Copilot 首次使用这些工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。稍后的练习会介绍此连接的工作原理（模型上下文协议）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在智能体选择器中选择 **Agent**。
3. 要求 Copilot 查找与更新说明文件相关的议题，并从当前分支创建 PR，同时说明其中包含说明更新和新的 publishers helper 基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 如果 Copilot 提示允许它代表你执行 GitHub 操作，请检查命令，并在适当时选择 **Allow**。
6. 创建 PR 后，使用以下提示要求 Copilot 合并 PR 并将本地分支切换回 main：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 提示允许它代表你执行 GitHub 操作并运行 shell 命令，请检查命令，并在适当时选择 **Allow**。

现在已在 GitHub Copilot 的帮助下创建并合并了拉取请求！

## 总结和后续步骤

你已经了解 Copilot 如何从项目说明文件获取上下文，并在 VS Code 中使用 Copilot Chat 完成以下工作：

- 发送代码生成提示，观察 Copilot 根据*现有*说明生成的内容；
- 向 `.github/copilot-instructions.md` 添加一项新的全库标准；
- 重新运行相同提示，观察建议代码如何采用新标准；
- 将说明更新和 helper 基础提交到 `main`，以便下一练习继续使用。

接下来，Copilot 会在[智能体模式][next-lesson]中使用这些说明，在整个代码库中添加新功能。

## 资源

- [GitHub Copilot 自定义的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [为 Copilot 编写更好的自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义说明][personal-instructions]
- [Awesome Copilot——说明文件及其他资源的集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
