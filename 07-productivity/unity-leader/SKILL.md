---
name: unity-leader
title: Unity 开发任务组织
description: 当明确要求组织 Unity 开发任务、安排跨模块实施与交付，或选择以 Leader 管理独立 Unity 任务时使用；形成适度的范围、实施安排和验收记录；已有 Trellis 等任务管理机制时沿用现行机制；具体编码与资产制作使用 unity-game-developer；触发词：Unity Leader、Unity 开发计划、Unity 任务组织、Unity 交付安排。
domain: 协作/pm
triggers: [Unity Leader, Unity 开发计划, Unity 任务组织, Unity 交付安排]
tags: [unity, game-dev, planning, delivery]
level: 进阶
status: stable
agents: [claude-code, codex, cursor, gemini-cli]
requires: [unity-game-developer]
related: []
combines_with: [unity-game-developer]
license: CC-BY-4.0
---
# Unity 开发任务组织

## 何时使用

组织 Unity 开发任务的范围、实施顺序、审查安排与交付。具体 C#、Scene、Prefab、UI、插件和验证方法以 `unity-game-developer` 为唯一专业规则来源，按任务读取该技能及相关参考，避免复制技术规则。

本技能为可选组织入口；常规 Unity 实现请求使用 Developer 即可。项目特有玩法、目录、人员安排和内部系统名称保存在项目文档中。

## 与现有任务机制配合

| 使用环境 | 职责安排 |
|---|---|
| 项目采用 Trellis 等任务机制 | 现有机制管理需求、计划、阶段、状态与归档；Developer 提供 Unity 技术规则 |
| 独立项目选择 Leader | Leader 组织范围、实施、审查与交付；Developer 负责专业实现标准 |
| 仅使用 Developer | 按任务所需完成实现与技术检查，沿用项目既有记录方式 |

读取项目指令和当前任务记录，确认本次采用的组织机制。即使显式调用本技能，现有 Trellis 仍负责该任务的计划和状态；Leader 仅补充缺失的组织信息。同一任务维护一套计划、状态、日志与归档。复用当前任务的 PRD、Check 和记录位置，禁止另建平行任务体系或擅自改动现有机制。

发现 `.trellis` 等目录时，读取适用约定；目录存在本身不构成初始化、运行脚本或改变任务状态的授权。按当前环境的技能清单定位 `unity-game-developer`，兼容仓库分类目录与本地平铺安装。依赖技能缺失时说明缺失项，可继续需求与安排工作；涉及专业实现前补齐专业规则，安装行为遵循当前授权。

## 组织任务

**范围与完成条件**：读取已有需求和实现，明确交付对象、范围、验收条件和待定事项。小型、范围明确的修复采用简短安排即可；公开接口、序列化数据或多个系统受影响时，形成与改动相称的实施方案。已有授权继续有效，只有影响相关实现的未定事项需要澄清。

**实施安排**：根据依赖确定先后顺序，说明涉及的模块接口、资产负责人、数据迁移与验证安排。技术选型依据 Developer 的对应参考及现有项目证据；缺少所需 Unity 插件时执行 Developer 的依赖门槛，并标记受阻部分，继续可独立完成的工作。

**审查与验证安排**：确定需要检查的范围、负责角色和完成依据，技术条目使用 Developer 的参考。现有审查或 Trellis Check 可以同时覆盖正确性与必要性；仍适用的证据可复用，修改影响证据有效性时补做相关检查。按实际需要执行，审查顺序无需形成固定的多阶段流程。

**完成与交付**：在现有任务记录或交付说明中列明完成内容、资产配置、验证证据、未完成事项及依赖。区分实现完成与验证完成，按当前机制维护状态和归档。简单任务可用交付说明承载记录，无须预建文档目录、任务台账或日志文件。

## 使用示例

`使用 Trellis 组织背包界面任务，并使用 $unity-game-developer 完成场景、Prefab、脚本与验证。`

`使用 $unity-leader 组织独立 Unity 项目的背包界面任务，结合 $unity-game-developer 确定实现标准和验收证据。`

## 执行边界

任务组织本身不增加子代理、委派、自动提交、推送、合并或发布的授权。按当前会话和项目指令执行相应操作。完成本次任务所需的既有授权可以持续使用。

## 互见

`requires` 与 `combines_with`：`unity-game-developer`，提供 Unity 专业实现、资产制作、审查与验证标准。
