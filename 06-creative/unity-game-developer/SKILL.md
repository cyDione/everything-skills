---
name: unity-game-developer
title: Unity 游戏开发
description: 在 Unity 6 LTS 中开发或优化跨平台游戏，处理架构选型、渲染、性能、资源管理、联机和打包时使用；触发词：Unity、URP、HDRP、DOTS、Addressables、Unity 卡顿优化、跨平台打包、Netcode。
domain: 创意/av
triggers: [Unity, URP, HDRP, Shader Graph, DOTS, ECS, Addressables, Unity 卡顿优化, 跨平台打包, Netcode]
tags: [unity, game-dev, csharp, performance, av, creative]
level: 进阶
status: stable
agents: [claude-code, codex, cursor, gemini-cli]
tools: [unity-6-lts, csharp, urp, hdrp, burst, addressables]
requires: []
related: [unreal-engine-cpp, glsl-shader-programming, bevy-ecs-rust]
combines_with: [glsl-shader-programming]
license: MIT
source: sickn33/agentic-awesome-skills
source_license: MIT
---
## 适用范围

面向 Unity 6 LTS 项目的运行时与编辑器开发，涵盖场景和界面制作、游戏框架选型、渲染管线、C# 性能、资源管理、联机与跨平台构建。非 Unity 引擎、纯美术关卡设计，以及通用 C# 后端任务由对应技能处理。性能与平台结论须通过目标硬件测试确认。

## 插件依赖门槛

任务明确要求 Unity 插件，或方案依赖第三方插件时，先检查本地是否提供 Unity 项目及插件文件。检查 `Packages/manifest.json`、`Packages/packages-lock.json`、`Assets` 插件目录和用户提供的 `.unitypackage` 等文件。

如果没有可检查的本地 Unity 项目，或无法确认所需插件文件已提供并安装，暂停依赖插件的实现，说明插件名称并请求提供 Unity 项目或插件文件。禁止在缺少插件时自行编写替代功能、仿造其 API，或改用 Unity 原生代码绕过依赖。可以继续不依赖该插件的资料核查、需求整理和接入步骤说明。获得文件后，先核对插件版本、公开 API、依赖项和 Unity 版本兼容性。

## 按任务读取参考

先按任务读取对应参考文档；跨领域任务读取所有相关文档。框架和插件推荐属于资料选型，不代表插件已安装；开始依赖其实现前仍须执行上方门槛。

| 任务 | 阅读 |
|---|---|
| 场景层级、GameObject 组织、MonoBehaviour 挂载、uGUI、Prefab、界面布局 | [场景与 UI 制作规范](references/scene-ui-authoring.md) |
| 游戏类型对应的工具包、GameFramework、ET、QFramework、DOTS、联机方案 | [游戏框架选型](references/game-framework-selection.md) |
| 渲染管线、Profiler 调优、Addressables、资源、测试、平台打包 | [性能与平台流程](references/performance-and-platforms.md) |

完成 Unity 资产修改后，按所读文档中的检查项复核场景、Prefab、Inspector 引用和目标平台表现。输出方案时说明适用平台、主要取舍及尚待真机验证的项目。

## 互见

- related：`code-reviewer` 可用于审查产出的 C# 脚本。
- combines_with：以本文件 frontmatter 的 `combines_with` 列表为准。

---
采编自 sickn33/antigravity-awesome-skills（MIT 许可）；适配重写并按主题拆分为入口与参考文档，聚焦 Unity 6 LTS 实践。
