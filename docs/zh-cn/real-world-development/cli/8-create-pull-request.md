---
title: "课程 8 - 创建并合并功能 PR"
description: "一起审查筛选功能、说明、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证（QA）配置文件和测试都保存在同一个分支中。现在一起审查它们并创建拉取请求。上一次由你手动合并了星级评分拉取请求（PR），这次将由 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、说明更新、技能更新和 QA 智能体拆分为几个不同的 PR。为了简化工作坊流程，本次在同一个对话和分支中保留完整的筛选和质量工作流，并将所有更改放入同一个 PR。

本课程将：

- 了解 Agent Merge 以及它如何自动管理合并生命周期；
- 检查完整功能更改和验证证据；
- 创建筛选功能 PR；
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能工作流中，你使用 Copilot 规划、实现和验证了一个功能。Tailspin Toys 现在希望自动执行剩余的 PR 工作，同时让开发人员保留合并授权。

## 认识 Agent Merge

**Agent Merge** 会自动执行合并拉取请求所需的后续工作。启用后，Copilot 会处理阻止 PR 合并的问题，例如修复持续集成（CI）检查失败、回复审查评论，以及在需要时进行 rebase；然后在存储库允许时启用 GitHub 自动合并。

到目前为止，你一直是自行选择 **Merge pull request**。Agent Merge 可以接手这项工作。启用 Agent Merge 前，请先审查工作并确认已经准备就绪。

## 使用 Agent Merge 管理 PR

所有代码已完成。现在一起审查、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监视 PR 中的持续集成流程。所有检查通过后，它会执行合并。
7. 确认 Copilot 显示类似 “PR #14 was squash-merged successfully.” 的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限要求。如果合并受阻，请阅读报告的原因，并在存储库允许时手动完成已审查的合并。

## 总结和后续步骤

你已自动完成开发流程中的多个环节，包括生成代码、测试和验证代码，以及管理拉取请求。本课程中，你：

- 了解 Agent Merge 以及它如何自动管理合并生命周期；
- 检查完整功能更改和验证证据；
- 创建了筛选功能 PR；
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

接下来探索更多实用的 [Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型、共享以及可选的云委托。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
