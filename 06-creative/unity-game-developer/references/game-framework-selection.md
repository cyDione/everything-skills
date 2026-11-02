# 游戏类型与框架选型

本文件提供初步候选。插件的维护状态、Unity 版本支持、依赖和价格会变化；项目定型前核实产品页面与文档，并在目标 Unity 版本建立小型验证项目。选中第三方插件后，执行 Unity 技能入口中的插件依赖门槛。

## 按游戏类型选择工具包

| 游戏类型 | 优先评估 | 适用范围与边界 |
|---|---|---|
| 2D 横版平台跳跃、跑轰 | [Corgi Engine](https://assetstore.unity.com/packages/templates/systems/corgi-engine-2d-2-5d-platformer-26617) | 提供 2D/2.5D 角色控制、跳跃、攀爬、武器、伤害与示例关卡。适合横版动作和类银河恶魔城；玩法仍需按项目定制。商店页面列有 Unity 6 支持。 |
| 2D/3D 俯视角动作 | [TopDown Engine](https://assetstore.unity.com/packages/templates/systems/topdown-engine-89636) | 提供俯视角角色控制、武器、伤害、瞄准、冲刺和反馈系统。适合双摇杆射击、俯视动作和轻量探索游戏；商店页面列有 Unity 6 支持。 |
| 第一人称、第三人称动作或射击 | [Opsive Ultimate Character Controller](https://opsive.com/assets/ultimate-character-controller/) | 角色控制与能力系统，覆盖移动、动画、物品和战斗能力，可扩展到射击、近战和冒险项目。它不是完整 FPS 框架；选用前核实目标 Unity 版本、输入系统和所需集成。 |
| 2D/3D RPG | [ORK Framework](https://orkframework.com/) | 覆盖回合制、ATB、即时战斗和阶段战斗，并提供角色状态、物品、装备、任务、对话等 RPG 系统。ORK 4 要求 Unity 6.3 或更新版本；旧项目需按版本选择 ORK 3 并验证迁移成本。 |
| 点击式冒险、剧情解谜 | [Adventure Creator](https://adventurecreator.org/) | 支持 2D、2.5D 和 3D，提供视觉脚本、交互、谜题、物品栏、对话和场景管理。适合以探索和剧情推进为主的冒险游戏；可用 C# 扩展专用机制。 |
| 视觉小说、文字冒险 | [Naninovel](https://naninovel.com/guide/index) | 提供文本脚本语言、故事编辑器、对话和叙事功能，适合分支剧情、视觉小说和过场叙事。自定义玩法或第三方集成通常还需 C#。 |
| 即时战略 | [Unity RTS Engine](https://docs.gamedevspice.com/rtsengine/) | 包含单位、建筑、资源和指令等 RTS 系统，可通过模块扩展。部分文档较早，正式采用前检查当前 Unity 版本、模块兼容性及联机实现。 |
| 塔防 | [Tower Defense Starter Kit](https://marketplace.unity.com/packages/tools/game-toolkits/tower-defense-starter-kit-128727) | 提供路径编辑、敌人波次、防御塔放置和基础界面，适合快速建立塔防玩法。商品页列有 Unity 6.0 至 6.6 兼容信息；依赖 TDTK-4 特定功能时另行验证版本。 |
| 赛车 | [Racing Game Starter Kit](https://assetstore.unity.com/packages/templates/systems/racing-game-starter-kit-22615)，搭配车辆物理系统 | RGSK 提供比赛流程、圈数和名次、AI、回放、锦标赛与界面；车辆物理可比较 [NWH Vehicle Physics 2](https://assetstore.unity.com/packages/tools/physics/nwh-vehicle-physics-2) 和 [Realistic Car Controller Pro](https://assetstore.unity.com/packages/tools/physics/realistic-car-controller-pro-178967)。各自职责不同，需核对接口与许可。 |
| 生存、采集、制作、建造 | [Survival Engine](https://assetstore.unity.com/packages/templates/systems/survival-engine-crafting-building-farming-178160) | 提供背包、制作、建造、采集、战斗与存档等基础系统，适合生存冒险原型和模拟玩法。开放世界项目重点验证存档规模、寻路、网络同步和内容流送能力。 |
| 农场与生活模拟 | [Farming Engine](https://assetstore.unity.com/packages/templates/systems/farming-engine-191881) | 包含种植、牲畜、背包、制作、建造和存档系统，适合农场经营原型。复杂经济、日历和长周期模拟通常需要扩展数据模型。 |
| 三消益智 | [MK Match-3 Kit](https://assetstore.unity.com/packages/templates/systems/mk-match-3-kit-candy-sweet-game-template-275837) | 完整三消模板，商品介绍列有多个关卡、地图、编辑器、道具及移动端变现功能。适合快速验证经典交换消除玩法；检查目标平台 SDK 和变现组件。 |
| 传统格子 Roguelike | [Ultimate Roguelike Framework](https://assetstore.unity.com/packages/tools/game-toolkits/ultimate-roguelike-framework-traditional-roguelike-starter-kit-363050) | 面向 Unity 6 的网格制 Roguelike 基础，适合回合制移动、地牢探索和单局流程。动作型 Roguelite 通常更适合从角色控制或俯视角框架开始。 |
| 卡牌构筑、卡牌 Roguelike | [Game Card System](https://assetstore.unity.com/packages/templates/systems/game-card-system-gcs-396692) | 提供卡牌战斗运行时、统一数据管理和 FlowGraph 可视化编辑，适合卡牌构筑和战斗玩法。复杂规则、服务器验证和牌组平衡仍由项目实现。 |
| 音乐节奏游戏 | [Rhythm Game Starter](https://www.rhythm.game/) | 提供 MIDI 导入、谱面制作和序列编辑工具，并按音频时钟同步节奏。适合轨道式和下落式音游；针对目标设备测量输入延迟并提供玩家校准。 |

## 通用游戏架构框架

GameFramework、ET 与 QFramework 提供项目架构或游戏模块；DOTS 是 Unity 的数据导向技术栈，Entities ECS 是其中一部分，职责层次不同。

| 框架 | 定位与优势 | 适用游戏与限制 |
|---|---|---|
| [Unity Game Framework](https://github.com/EllanJiang/UnityGameFramework) | 通用游戏框架，内置流程、资源、场景、界面、网络、事件、对象池、本地化、声音和数据表等模块；MIT 许可。 | 适合需要较完整客户端模块体系、异步资源管理和流程组织的中大型 GameObject 项目。上游提交记录较早，需核对目标 Unity 版本、维护状态及社区扩展。 |
| [ET](https://github.com/egametang/ET) | Unity 客户端与 C# 服务端框架，面向 Actor、Fiber、分布式服务器、多进程和热更新，也包含测试及机器人支持。 | 适合 MMO、持久在线游戏和需要共享客户端服务端逻辑的团队。学习、部署与架构约束较多；ET10 许可证要求商业产品发布前取得授权，标价人民币 4999 元，并限制自行修改后再开源。采用前逐项核对许可证和依赖。 |
| [QFramework](https://github.com/liangxiegame/QFramework) | 轻量分层架构，以 ViewController、System、Model、Utility、Command 和 Event 组织表现、业务逻辑与数据；提供可选 UIKit、ResKit、AudioKit 等工具包；MIT 许可。 | 适合独立游戏及小中型单机项目，也适合希望保留 MonoBehaviour、场景和 Prefab 可视化编辑方式的团队。区分核心架构与工具包，并按需引入。 |
| [Unity DOTS / Entities](https://unity.com/dots) | 数据导向技术栈，包括 Entities ECS、Burst 编译器和 C# Job System；可与 GameObject 并用。Unity 官方示例覆盖实体、物理、图形及 Netcode for Entities。 | 适合 RTS、大量单位、城市模拟、群体 AI、复杂物理或高吞吐仿真。它不提供完整玩法、内容生产或通用服务端架构；确认 Unity 与 Entities 包版本，并以性能分析验证负载适配。 |

轻量 2D、解谜、剧情、卡牌和单机 Roguelite 可先采用 Unity 原生 GameObject 结构，或选 QFramework 核心分层；需要现成流程、资源及界面模块时评估 GameFramework。大型在线 RPG、MMO 和长期运营游戏可评估 ET，并将授权条件列入立项决策。RTS、大规模群体、城市建造和仿真项目可把 DOTS 用于实体密集的模拟部分，继续用 GameObject、场景和 Prefab 管理关卡、角色表现与 UI。

## 联机框架

联机框架负责网络同步、连接和权威模型，不取代玩法框架。先确定玩家规模、服务器权威、延迟敏感度、主机模式或专用服务器，以及托管预算。

| 方案 | 适用场景 | 选择依据 |
|---|---|---|
| [Netcode for GameObjects](https://docs.unity.com/en-us/multiplayer/netcode/netcode) | 小规模合作游戏，或给现有 GameObject/MonoBehaviour 项目增加联网 | Unity 官方方案，可与 Unity Multiplayer Services、Relay 等配合，适合建立基础联机。 |
| [Photon Fusion 2](https://doc.photonengine.com/fusion/v2/fusion-intro) | 对状态同步和实时响应要求较高的动作或竞技玩法 | 支持 Shared、Host 和 Server 等拓扑；确认 Photon 服务、托管方式和用量费用符合预算。 |
| [FishNet](https://fish-networking.gitbook.io/docs/overview/readme/features) | 需要客户端预测及较多网络功能的 GameObject 项目 | 提供预测、回滚相关功能和多种传输层；检查当前 Unity 兼容性，并规划服务器与中继服务。 |
| [Netcode for Entities](https://docs.unity.com/en-us/multiplayer/netcode/netcode) | 已采用 ECS/DOTS、需要高吞吐数据导向架构的多人项目 | 与 Entities 技术栈配套；适合愿意承担 ECS 学习与工具链成本的团队。 |

大型 MMO、持久化世界或跨区服务通常需要独立的服务器模拟、数据库、匹配和运维架构。安装联网 SDK 无法提供完整 MMO 服务端。

## 选型验证

结合玩法规模、团队经验、内容编辑方式、目标平台、Unity 与包版本、维护状态、许可、网络拓扑和迁移成本。优先用小型验证场景测试核心循环、场景编辑、Prefab/UI、构建及目标设备性能，再确定使用范围。候选链接用于核验来源，不表示插件已安装或已验证；插件缺失时遵守 Unity 技能入口的插件依赖门槛。
