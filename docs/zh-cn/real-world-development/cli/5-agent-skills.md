---
title: "课程 5 - 自定义并使用 quality-checks 技能"
description: "浏览现有的 quality-checks 技能，自定义其报告格式，并使用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是编写代码。我们已经手动验证了代码是否正常运行，并使用说明文件确保它遵循标准。但测试、lint 和持续集成（CI）的其他环节又该如何处理？

这类任务最适合使用**智能体技能**！技能可以帮助 Copilot 正确执行这些操作。

本课程将：

- 浏览现有的 `quality-checks` 技能；
- 自定义其结果格式；
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易于阅读。

## 说明、脚本和资源

智能体技能将可复用的任务说明、可执行脚本和支持资源封装在一起，由智能体按需加载。技能的核心是一个以技能名称命名的文件夹，其中包含名为 `SKILL.md` 的 Markdown 文件。该 Markdown 文件包含用于说明技能用途的 front matter（名称和描述）、功能概览，以及关于何时应调用技能的指导。文件夹还可以包含子文件夹，提供脚本和其他资源供技能调用时使用。

> [!NOTE]
> 技能不一定需要其他文件和文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用了项目已有的命令。

技能可以位于项目的 `.github/skills` 文件夹中，作为存储库资源供团队共享和复用；也可以存放在用户技能目录 `~/.copilot/skills` 中。

## 浏览技能

现在来查看 Tailspin Toys 团队创建的技能 `quality-checks`，了解它如何运行单元测试、lint 和类型检查。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读文件顶部的 `name` 和 `description`。描述帮助 Copilot 判断何时调用技能。
3. 阅读说明，了解它如何引导 Copilot 执行测试和 lint 流程。
4. 注意技能目前还没有 **Results output formatting** 部分。

## 更改前运行技能

技能可以通过 Copilot CLI 直接调用，也可以使用自然语言请求。现在请 Copilot 运行这三项检查。

1. 返回交互模式中的筛选功能对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告。

## 自定义报告

我们希望得到更完善的报告，其中说明运行了什么、是否成功以及工具报告的实际结果。现在更新技能，让它生成这样的报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

更改已完成，现在查看实际效果！Copilot CLI 可以在不重启对话的情况下重新加载编辑过的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 再次使用与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告，并与第一份报告比较。

## 总结和后续步骤

你已经自定义并使用了现有的智能体技能。本课程中，你：

- 浏览了用于单元测试、lint 和类型检查的 `quality-checks` 技能；
- 自定义了结果格式；
- 重新加载并运行了技能。

此更改将与筛选功能一起包含在功能 PR 中。接下来使用 [Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不是额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和打包的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
