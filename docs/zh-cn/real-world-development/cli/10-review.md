---
title: "课程 10 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已经在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预先创建的筛选议题；
- 添加星级评分，在转发的浏览器中审查结果，并手动合并第一个拉取请求（PR）；
- 从筛选议题开始，在计划模式中确定方案，在自动驾驶模式中实现，然后在交互模式中审查；
- 使用自定义说明引导智能体，然后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查；
- 添加 Playwright 模型上下文协议（MCP）服务器，并在真实浏览器中体验筛选功能；
- 创建并选择质量保证（QA）自定义智能体，评估需求、测试覆盖率、技能结果和浏览器证据；
- 审查完整的筛选功能更改，并授权 Agent Merge 合并筛选 PR；
- 探索用于管理上下文、模型、共享和可选云委托的斜杠命令。

## 已交付内容

本工作坊包含两个 PR 阶段，每个阶段都从已更新的 `main` 创建独立分支：

1. **星级评分：**在游戏卡片上显示现有 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：**实现筛选、更新说明并将其应用到功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并添加相关测试。

从规划筛选功能到创建 PR 的整个过程都使用了同一对话和分支。为了简化工作坊流程，这些更改都放在同一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：运行自动化测试、在浏览器中自行检查，并通过 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并以新的格式报告结果。QA 智能体会在创建 PR 前，结合这些结果审查需求和测试覆盖率。

添加的测试应弥补实际缺口；如果 QA 运行后确认不需要添加测试，也可能是正确结果。授权合并前应审查代码和证据；更改后也要更新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作方式。在本工作坊中，你更新了说明、自定义技能、创建 QA 配置文件并配置 MCP 服务器。这些自定义项可以在不同对话中复用，也可以根据团队需要调整。说明定义标准，技能描述可重复执行的任务，自定义智能体定义专门角色，MCP 服务器连接外部工具。审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用**计划模式**在开始构建前思考方案；使用**交互模式**跟进范围明确的更改；使用**自动驾驶模式**处理范围清晰的任务。常规编辑可以选择更快的模型，复杂工作则可以选择能力更强的模型。

上下文仍与基础设施同样重要。清楚描述要构建的内容、原因和方式，会显著影响输出。

## 继续探索

你已经完成核心工作流。以下是一些值得了解的其他 CLI 功能：

- 使用 `/review` 让代码审查智能体分析更改；
- 使用 `/rubber-duck` 讨论问题并获得不同视角；
- 使用 `/fleet` 并行编排独立子任务；
- 使用 `/worktree` 隔离单独任务；
- 使用 `/delegate` 将任务发送给 Copilot 云智能体。

## 后续步骤

提升任何工具使用能力的最佳方式是持续实践！将它用于生产代码、个人项目，或多年来一直想构建却没有动手的小应用。与团队分享经验，也从他们的经验中学习。还可以继续探索相关文档。

如果希望通过终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列课程][foundry]。若要比较其他工作环境，可以探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot 云智能体工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
