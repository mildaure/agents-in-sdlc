---
title: "课程 3 - 智能体模式：计划与自动驾驶"
description: "使用计划模式商定方案，使用自动驾驶模式根据议题构建筛选功能，并在交互模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们从为项目添加一项小功能开始。但较大的更改需要更完善的流程。幸运的是，GitHub Copilot CLI 的设计目标是融入组织现有的工作流，帮助团队以正确的方式构建正确的功能。本课程是多个课程中的第一课，你将遵循典型的智能体驱动开发流程：根据议题生成新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 后续功能工作流将继续使用同一个对话和分支。通常，不同类型的文件会使用不同的分支或 PR，但此处会简化流程，以便专注于核心概念。

本课程将：

- 根据 GitHub 议题启动新的 Copilot 对话；
- 在计划模式中定义需求；
- 使用自动驾驶模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

继续完成此功能时，你还将更新存储库说明、自定义现有 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩大，访客需要按类别和发行方筛选游戏。待办事项中的议题描述了此功能，但组合多个类别等细节需要在编码前达成一致。你将使用计划模式处理这些决定，然后授权 Copilot 使用自动驾驶模式完成范围明确的实现。

## 背景

将 AI 编码智能体引入开发流程不会改变基本原则。事实上，这些原则变得更加重要！大多数开发人员遵循的流程大致如下：

1. 从描述所需更改的现有议题开始。
2. 制定要构建内容的计划。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码经过审查且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述方式类似。

遵循这一标准方法，可以确保 AI 生成的代码满足既定需求，并经过与手写代码相同的审查流程。

## 对话模式

**对话模式**决定智能体可以自主工作到什么程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 在不同模式之间切换：

- **Interactive**：你与智能体协同工作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先制定计划，且不能编辑项目文件。
- **Autopilot**：智能体自主工作，编写代码、运行测试并迭代，直到完成任务。

先使用计划模式审查方案，再使用自动驾驶模式实现方案。

## 从议题开始

开始筛选功能开发前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动再继续。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用箭头键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，然后返回 **Session** 选项卡。

注意提示现在以 `#7` 或类似数字开头。`#` 用于将 GitHub 上的议题或拉取请求（PR）添加到上下文中。

## 规划筛选功能

规划让你可以在交由 Copilot 执行前，先定义实现功能或处理任务的方法。处理复杂任务时，花些时间进行规划总是很有帮助的。现在切换到计划模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到计划模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划：先探索项目，再确定最佳方法。

3. 过程中 Copilot 可能会询问筛选功能应该如何工作。根据偏好回答即可，这里没有错误的答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看并审查计划。
6. 如果计划中的任何部分与已确定的决定不符，请要求 Copilot 修改。

## 批准自动驾驶模式

计划已写好并审查完成，现在可以开始实现。使用自动驾驶模式，让 Copilot 开始工作。

自动驾驶模式可以让 Copilot 持续迭代问题，直到它认为工作已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前安装版本中名称相近的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 查看 Copilot 如何按照既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，应先解决设置问题，之后再认为检查已完成。

## 审查并验证实现

代码生成后，应像审查其他代码一样在合并前进行审查。现在审查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入交互模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 根据议题以及规划时做出的决定，审查实现结果。
4. 完成代码审查后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 修复任何失败项。

## 体验新功能

代码看起来不错，但能否正常运行？像上次一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行方筛选，以及计划中商定的组合筛选。
4. 确认重置和无结果时的行为符合议题和已作出的决定。
5. 返回 Codespace，并要求 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已经使用不同的对话模式构建并审查了一项功能。本课程中，你：

- 根据 GitHub 议题启动新的 Copilot 对话；
- 在计划模式中定义需求；
- 使用自动驾驶模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

接下来通过[自定义说明][next-lesson]进一步了解如何生成代码并确保其遵循文档化的实践。

## 资源

- [GitHub Copilot CLI 中的自动驾驶模式][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
