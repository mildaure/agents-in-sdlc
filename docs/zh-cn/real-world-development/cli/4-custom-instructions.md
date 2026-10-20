---
title: "课程 4 - 使用自定义说明指导 Copilot"
description: "浏览存储库说明、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，就要确保 Copilot 能够访问相关指导。[说明文件][instruction-files]不只是描述你想要什么代码，也会说明代码应该如何组织。现在筛选功能已经完成，接下来将浏览 Copilot 使用的说明、添加文档标准并将其应用于代码。

本课程将：

- 了解存储库说明和按路径限定范围的说明文件如何传递给智能体；
- 更新说明文件，确保遵循编码标准；
- 了解说明文件对代码的影响。

## 场景

和优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 注释应说明意图和不明显的决策，而不是复述代码；
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果存在可注入的 `db` 参数，也应记录；
- 可复用的 Astro 组件应记录其 `Props` 接口，并在相关代码更改时更新注释；
- 保留现有格式和 lint 指导。

使用说明文件可以确保 Copilot 获得所需信息，并按照这些实践要求完成任务。

## 说明文件

自定义说明可以为 Copilot 提供上下文和偏好，帮助它更好地理解编码风格和需求。这项功能可以引导 Copilot 给出更相关的建议和代码片段。你可以指定偏好的编码约定、库，甚至希望添加的注释类型。可以为整个存储库创建说明，也可以为特定文件类型创建说明。

说明文件有两种：

- `.github/copilot-instructions.md`：针对存储库的**每个**请求发送给 Copilot 的单个说明文件。此文件应包含与大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的说明文件。

> [!NOTE]
> 其他说明格式及其支持情况因环境而异。使用特定格式前，请参阅[自定义说明支持参考][custom-instructions-support]。

## 浏览本项目中的自定义说明文件

起始项目中已经包含一组说明文件。先浏览现有文件，再进行更改，以了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器中（而非终端）打开 `.github/copilot-instructions.md`。
3. 浏览文件，注意项目简介和编码指导。这些说明适用于此存储库中的每次 Copilot 交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意这些说明涉及 Astro 文件、Drizzle 数据层、测试等。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置一个 glob 以确定这些说明适用于哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中链接到的其他说明文件和现有项目文件。这样可以将较大的说明拆分为更小、可复用的文件，并为 Copilot 提供代码示例。

## 更新说明文件以符合团队指导

现有文件是很好的开端，但仍有不足。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，位置应在第 35 行附近。
2. 在该部分最后添加以下条目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在新对话开始时加载存储库说明。保存更新后的说明后，退出并恢复筛选功能对话，这样 Copilot 就能在保留功能上下文的同时加载新指导。

1. 如果筛选功能对话仍在运行，输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选功能对话，并确认当前已检出 `game-filters-cli` 分支。
3. 要求 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和后续步骤

你已经了解 Copilot CLI 如何从说明文件中获取上下文，并将新标准应用于功能。本课程中，你：

- 了解存储库说明和按路径限定范围的说明文件如何传递给智能体；
- 更新说明文件，确保遵循编码标准；
- 了解说明文件对代码的影响。

接下来[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保以一致方式运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot——说明文件及其他资源的集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
