---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能够访问这些上下文。[说明文件][instruction-files]可以提供这些指导，帮助 Copilot 不仅理解要做什么，也理解应该如何完成。

在本练习中，你将：

- 了解如何通过存储库自定义说明和按路径限定范围的说明文件，为 Copilot 提供项目上下文、编码准则和文档标准；
- 向 `.github/copilot-instructions.md` 添加一项新的全库标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，这里不会使用提示对比更改前后的结果。Copilot 云智能体会异步处理 GitHub 议题，因此不容易实时并排展示其影响。稍后审查云智能体创建的拉取请求时，可以看到说明文件发挥的作用。

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

## 添加新的存储库级标准

接下来只需进行一次编辑：添加全库规则，要求使用 TSDoc 文档注释和文件级注释头。之后，云智能体处理分配给它的议题时就会使用这项规则。

编辑前，先创建一个分支（云智能体会读取议题目标分支中的说明，不过提交编辑仍应保持整洁）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在，为它补充存储库编码标准，以改善代码建议。

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

## 提交、推送并合并说明更新

云智能体会读取议题目标分支中的说明。下一练习将向 Copilot 分配议题，它会从 `main` 创建分支，因此必须先将说明更改合并到 `main`，云智能体才能使用这些说明。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。这样，云智能体在下一练习中处理任务时就会读取这些说明。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main` 分支，可以跳过创建分支，并直接提交到 `main`。创建分支是为了让工作坊流程更接近真实项目的处理方式。

## 总结和后续步骤

你已经了解 Copilot 如何从本项目的说明文件获取上下文，并向 `.github/copilot-instructions.md` 添加了一项新的全库标准。接下来审查云智能体创建的拉取请求时，会看到这项标准的应用效果。

下一步，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [为 Copilot 编写更好的自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot——说明文件及其他资源的集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
