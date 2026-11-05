---
name: unity-game-developer
title: Unity 游戏开发
description: 在 Unity 项目中设计、实现、调试或审查游戏系统、场景与界面。用于 Unity C# 开发、可视化 Scene 与 uGUI 制作、插件框架选型、性能、资源、联网和平台构建。
domain: 创意/av
triggers: [Unity, URP, HDRP, Shader Graph, DOTS, ECS, Addressables, Unity 卡顿优化, 跨平台打包, Netcode]
tags: [unity, game-dev, csharp, performance, av, creative]
level: 进阶
status: stable
agents: [claude-code, codex, cursor, gemini-cli]
tools: [unity-6-lts, csharp, urp, hdrp, burst, addressables]
requires: []
related: [unreal-engine-cpp, glsl-shader-programming, bevy-ecs-rust]
combines_with: [glsl-shader-programming, unity-leader]
license: MIT
source: sickn33/agentic-awesome-skills
source_license: MIT
---
## 适用范围

面向 Unity 项目的运行时与编辑器开发，涵盖 C# 游戏系统、场景和界面制作、框架选型、渲染、性能、资源管理、联网与平台构建。非 Unity 引擎、纯美术关卡设计，以及通用 C# 后端任务由对应技能处理。性能与平台结论须通过目标硬件测试确认。

## 与任务管理技能配合

本技能提供 Unity 专业标准，可以独立使用。任务计划、阶段、状态和归档沿用项目现行机制；采用 Trellis 时，由 Trellis 组织任务，本技能补充技术实现、审查与验证条目。现有 Check 可以覆盖本技能的审查要求，证据仍适用于当前修改时复用。

需要组织独立 Unity 任务时，可配合 `unity-leader`。该技能负责范围、实施安排、审查安排与交付；本技能保留技术规则的唯一版本。按当前环境技能清单定位技能文件，仓库分类目录与本地安装目录可以不同。仅安装本技能时，按任务所需完成技术工作，无须另建计划、状态或日志体系。

## 插件依赖门槛

任务明确要求 Unity 插件，或方案依赖第三方插件时，先检查本地是否提供 Unity 项目及插件文件。检查 `Packages/manifest.json`、`Packages/packages-lock.json`、`Assets` 插件目录和用户提供的 `.unitypackage` 等文件。

如果没有可检查的本地 Unity 项目，或无法确认所需插件文件已提供并安装，暂停依赖插件的实现，说明插件名称并请求提供 Unity 项目或插件文件。禁止在缺少插件时自行编写替代功能、仿造其 API，或改用 Unity 原生代码绕过依赖。可以继续不依赖该插件的资料核查、需求整理和接入步骤说明。获得文件后，先核对插件版本、公开 API、依赖项和 Unity 版本兼容性。

## 按任务读取参考

先检查 Unity 工程根目录中的 `Assets/`、`Packages/`、`ProjectSettings/`，读取项目指令、当前实现和 Git 状态。核实目标平台、Unity 与包版本、已有资产、玩法契约及受影响的接口和序列化数据；以实际工程为技术依据。未确定的契约只阻塞依赖该决定的实现。

按任务读取相应参考；跨领域任务读取所有相关参考。入口只保存共同要求与路由，详细规则按需载入。

| 任务 | 阅读 |
|---|---|
| 文件夹组织、场景层级、组件宿主、uGUI、Prefab 和可视化装配 | [场景与 UI 制作规范](references/scene-ui-authoring.md) 与 [资产交接规范](references/asset-handoff.md) |
| C# 结构、状态所有权、生命周期、复用及正确性与必要性审查 | [架构与代码审查](references/architecture-and-review.md) |
| Unity 或包 API 核验、测试选择、验证证据及交付状态 | [API 与验证证据](references/api-and-validation.md) |
| 游戏类型对应的工具包、GameFramework、ET、QFramework、DOTS、联机方案 | [游戏框架选型](references/game-framework-selection.md) |
| 渲染管线、Profiler 调优、Addressables、资源、测试、平台打包 | [性能与平台技术](references/performance-and-platforms.md) |

## 通用开发约束

职责按项目需求分别归属于普通 C#、MonoBehaviour、ScriptableObject、Prefab、Scene 或 Editor 工具。保持内容定义、运行时状态、规则执行和表现的边界；选择已有可复用能力，不默认增加 ECS、依赖注入、事件总线、全局单例、Addressables 或联网框架。配置字段的 Inspector 说明包含准确语义、单位、范围和默认值；未知的玩法数值标记待定，不臆造。

尊重既有 Scene、Prefab、Inspector、序列化资产及其所有者。场景行为组件有明确的 GameObject 宿主，结构稳定的 UI 保存在可视化 Scene 或 Prefab 中；运行时逻辑使用明确引用。仅当运行时数据决定对象数量或算法布局时动态实例化对象，并将稳定结构留在 Prefab。操作资源时保留 `.meta` 与 GUID 引用，不重序列化无关资产。

第三方框架推荐属于资料选型，实际接入须满足上方插件依赖门槛。

实现应满足已确认的契约，并能说明新增状态、抽象与依赖的用途。审查覆盖正确性与必要性两个维度，可合并到现有审查中。Unity 资产修改后，复核 Scene 与 Prefab 层级、组件宿主、Inspector 引用和目标平台表现。按影响范围选择技术验证，证据区分静态检查、Unity 编译、自动测试、编辑器验证、Player 构建和设备验证；相关但未执行的项目标记为“未执行”。

## 互见

- related：`code-reviewer` 可用于审查产出的 C# 脚本。
- combines_with：`unity-leader` 用于独立任务的组织与交付；采用 Trellis 时由现有任务机制承担该职责。其他组合以 frontmatter 为准。

---
基于 Unity 游戏开发参考，结合 Unity 主程通用规范整理。入口按任务路由至场景制作、架构审查、API 验证、框架选型与平台技术参考。
