---
title: "课程 9 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话以及可选委托给云智能体的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。它们公开了高级功能、幕后信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在探索其他几个实用命令。

本课程将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文；
- 使用 `/model` 探索可用模型；
- 了解 `/share` 如何导出或共享会话；
- 探索用于并行工作、工作树和委托给云智能体的可选命令。

## 场景

核心 CLI 工作流已完成。现在了解一些其他功能：管理上下文、切换模型、共享会话，以及选择将工作委托给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会遇到模型上下文窗口的限制。Copilot CLI 会在需要时自动压缩对话；你也可以使用斜杠命令查看或手动压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 使用量，以及上下文在系统说明、工具、消息和剩余空间之间的分配情况。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> Copilot CLI 会在上下文窗口逐渐填满时自动压缩上下文。如果希望自行决定压缩时机，请使用 `/compact`。如果要切换到无关任务并开始新对话，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有所长，不同开发人员也有不同偏好。Copilot CLI 可列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型和使用情况。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作并分享经验有助于所有人更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看当前安装版本中 `/share` 的选项。
2. 如果希望共享此会话，请选择适合需求的目标位置，例如使用 `/share file` 将其导出为本地 Markdown 文件。
3. 发送或发布前，请审查导出内容。会话导出内容可能包括提示、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队不打算共享的存储库或对话内容。

## 可选：扩展并行工作或委托任务

核心工作坊流程已完成。Copilot CLI 还提供适用于大型任务的命令：

- `/fleet` 可以将独立子任务分配给子智能体并并行运行；
- `/worktree` 可以为单独任务创建隔离的 Git 工作树；
- `/delegate` 可以将任务发送给 Copilot 云智能体，以异步方式处理并可能创建拉取请求。

这些命令是可选的，因为它们可能创建其他工作树或远程任务。尝试之前，请先创建范围明确的新任务，并按照常规工作流审查结果。如果希望进一步了解异步智能体工作方式，可以继续学习[云智能体工作坊][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以进行配置、共享会话并了解幕后状态。本课程中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文；
- 使用 `/model` 探索可用模型；
- 了解 `/share` 如何导出或共享会话；
- 探索用于并行工作、工作树和委托给云智能体的可选命令。

Copilot CLI 还提供了更多斜杠命令，值得继续探索！接下来[回顾所学内容][next-lesson]并了解后续学习步骤。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
