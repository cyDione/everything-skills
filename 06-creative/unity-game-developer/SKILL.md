---
name: unity-game-developer
title: Unity 游戏开发
description: 当用 Unity 6 LTS 开发/优化跨平台游戏，需要选渲染管线、写高性能 C#、Profiler 调优、Addressables 资源管理或多平台打包时使用；做出可落地的架构与优化方案；不适用于纯美术/关卡设计、非 Unity 引擎（UE/Godot/Cocos）、或后端服务开发；触发词：Unity、URP、HDRP、Shader Graph、DOTS、Addressables、卡顿优化、跨平台打包。
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
## 何时使用

- 用 **Unity 6 LTS** 做或优化游戏，需要工程级决策：渲染管线选型、性能调优、可扩展架构、跨平台打包时使用。
- 典型任务：选 URP/HDRP/Built-in；用 Profiler 定位 CPU/GPU/内存瓶颈；写零/低 GC 的 C#；Job System + Burst / DOTS-ECS 提升吞吐；Addressables 做动态资源加载；Netcode for GameObjects 做联机；iOS/Android/主机/PC/WebGL/XR 适配打包。
- 触发词：Unity、URP、HDRP、Shader Graph、DOTS、ECS、Addressables、卡顿优化、跨平台打包、Netcode。

不该用的边界：
- 非 Unity 引擎（Unreal、Godot、Cocos、自研引擎）→ 本技能不通用。
- 纯美术/关卡/数值设计、无代码与性能诉求 → 交给设计类技能。
- 通用 C#/.NET 后端、纯算法题 → 用研发卷对应技能；本技能聚焦 Unity 运行时与编辑器特性。
- 不替代真机实测：任何结论都需在目标硬件上验证，不臆断帧率。

## 插件依赖门槛

- 当任务明确要求使用某个 Unity 插件，或所选方案依赖第三方 Unity 插件时，先检查工作区是否提供 Unity 项目及该插件文件。可检查 `Packages/manifest.json`、`Packages/packages-lock.json`、`Assets` 下的插件目录，以及 `.unitypackage` 等用户提供的插件包。
- 如果没有可检查的本地 Unity 项目，或未提供且无法确认所需插件文件已经安装，暂停依赖该插件的实现，说明所需插件名称，并请提供 Unity 项目或插件文件后再继续。
- 插件文件缺失时，禁止自行编写插件的替代功能、仿造其 API，或改用 Unity 原生代码绕过插件依赖。可以继续进行不依赖该插件的资料核查、需求整理和接入步骤说明。
- 收到项目或插件文件后，先检查插件版本、公开 API、依赖项和 Unity 版本兼容性，再开展集成；不得根据插件名称或宣传页推定具体 API。

## 场景、脚本与 uGUI 制作规范

Unity 场景与 Prefab 是可视化编辑资产。CLI、MCP 或脚本可以辅助建立和修改这些资产，但默认交付应保留可在 Unity Editor 的 Hierarchy、Scene、Inspector 与 Prefab Mode 中检查和修改的对象结构。

### 场景层级

- 修改场景前先检查现有 Hierarchy、场景资产与项目命名约定；新对象加入已有职责分组，避免散落在场景根节点。
- 每个场景可用命名清楚的空 GameObject 作为场景对象容器，例如 `--Scene--`。该名称是团队约定，不是 Unity 的内置语义；`.unity` 文件才是场景资产本身。
- 场景根下的主要功能分组建议控制在两至三类。常见示例为 `Environment`、`Gameplay`、`Systems`；按项目内容调整，Canvas 可置于 `Systems/UI`。复杂场景只在职责清楚时增加分组。
- 空 GameObject 只用于收纳相关对象、表达共同变换或承载场景级组件。保持层级浅且可读，避免多余的空父对象和过深嵌套。
- 运行时新增实例应放入其所属系统或容器之下；按需建立明确的动态对象分组，避免实例散落在根层级。

### 脚本挂载与引用

- 承担场景行为的 `MonoBehaviour` 应挂载在职责对应的场景 GameObject 上。场景级管理逻辑可挂载在命名明确的空对象上；Prefab 行为脚本应作为组件保存在对应 Prefab 中。
- 新增脚本时同时建立明确的组件宿主并配置必要组件与 Inspector 引用。脚本文件存在于场景目录，不等于其行为已连接到场景对象。
- 可由 Inspector 配置的对象、组件与 Prefab 引用优先使用序列化字段并在 Editor 中连接。避免为固定场景引用额外使用字符串查找或运行时扫描。
- 纯数据可使用 `ScriptableObject`；静态工具类、普通 C# 服务类和 Editor 扩展按其用途组织，无需强行挂到 GameObject。需要依赖生命周期、启用状态或场景引用的逻辑才使用合适的 `MonoBehaviour` 宿主。
- 禁止把脚本挂在没有明确职责的场景根对象上。确需集中管理多个子系统时，使用命名清楚的管理对象，并通过显式引用说明其责任范围。

### uGUI 与界面 Prefab

- 对于常驻界面和结构稳定的面板，优先在 Unity Editor 中搭建 Canvas、RectTransform、UI 子对象及组件，再保存为场景对象或 Prefab。界面结构、组件与引用应能在 Hierarchy、Inspector 和 Prefab Mode 中检查及修改。
- 仅要求界面布局或视觉元素时，交付 Prefab 或场景层级；只有任务明确要求交互逻辑或代码时才增加脚本。创建按钮等交互控件时，先正确配置对应 uGUI 组件，控件本身不意味着需要额外的自定义脚本。
- 界面行为脚本挂在负责该行为的 UI GameObject 上，或挂在该面板的根对象上。优先用序列化字段连接按钮、文本、图像、面板与数据视图；避免在代码中按对象名称查找界面元素。
- 布局优先使用锚点、Pivot、Layout Group、Layout Element、Content Size Fitter 等 Editor 可配置组件。除非布局确实由运行时数据或算法决定，避免在代码中逐个创建控件并赋予坐标、尺寸、锚点和样式。
- 对背包条目、列表项、通知、对话选项等数量由运行状态决定的重复元素，先制作完整 UI Prefab，包含视觉层级、所需 uGUI 组件及该元素自己的行为脚本，再由运行时代码实例化并更新数据。
- 动态实例化只负责数量、生命周期、数据和必要事件更新；稳定视觉层级、组件设置与默认布局保留在 Prefab 和 Inspector。作为 Layout Group 子项时，让布局组件负责排布；仅在设计确实要求自由定位时设置 `RectTransform`。
- 运行时创建整页 UI、逐元素构造视觉树或用代码替代可视化布局，必须有明确需求，例如完全由程序生成的调试界面或算法生成的图表，并说明为何 Prefab、场景对象及布局组件无法满足。

### 制作后检查

- 在 Editor 中复核场景层级、组件宿主、Inspector 引用、Prefab 引用和 UI 布局；验证界面元素尺寸、锚点、遮挡顺序与目标分辨率下的表现。
- 对有交互的 uGUI，检查场景中恰有一个有效 `EventSystem`、Canvas 上存在 `GraphicRaycaster`，且按钮回调按需求连接。
- 使用 CLI、MCP 或自动化脚本生成对象后，仍需检查并保存场景或 Prefab 资产；不要只创建运行时脚本而未产出可编辑的界面对象结构。

## 按游戏类型选择框架

下表是起始候选，不代表唯一或永久排名。框架的维护状态、Unity 版本支持、依赖和价格会变化；项目定型前核实产品页面与文档，并在目标 Unity 版本建立空白测试项目验证导入和构建。选中第三方插件后，执行插件依赖门槛。

| 游戏类型 | 优先评估 | 适用范围与边界 |
|---|---|---|
| 2D 横版平台跳跃、跑轰 | [Corgi Engine](https://assetstore.unity.com/packages/templates/systems/corgi-engine-2d-2-5d-platformer-26617) | 提供 2D/2.5D 角色控制、跳跃、攀爬、武器、伤害与示例关卡。适合横版动作和类银河恶魔城；以它的角色控制和系统为基础，玩法仍需按项目定制。商店页面列有 Unity 6 支持。 |
| 2D/3D 俯视角动作 | [TopDown Engine](https://assetstore.unity.com/packages/templates/systems/topdown-engine-89636) | 提供俯视角角色控制、武器、伤害、瞄准、冲刺和游戏反馈系统。适合双摇杆射击、俯视动作和轻量探索游戏；商店页面列有 Unity 6 支持。 |
| 第一人称、第三人称动作或射击 | [Opsive Ultimate Character Controller](https://opsive.com/assets/ultimate-character-controller/) | 属于角色控制与能力系统，覆盖移动、动画、物品和战斗能力，可扩展到射击、近战和冒险项目。它不是完整 FPS 游戏框架；选用前核实目标 Unity 版本、输入系统和所需集成。 |
| 2D/3D RPG | [ORK Framework](https://orkframework.com/) | 覆盖回合制、ATB、即时战斗和阶段战斗，并提供角色状态、物品、装备、任务、对话等 RPG 系统。ORK 4 要求 Unity 6.3 或更新版本；旧项目需根据版本选择 ORK 3，并验证迁移成本。 |
| 点击式冒险、剧情解谜 | [Adventure Creator](https://adventurecreator.org/) | 支持 2D、2.5D 和 3D，提供视觉脚本、交互、谜题、物品栏、对话和场景管理。适合以探索和剧情推进为主的冒险游戏；可用 C# 扩展专用机制。 |
| 视觉小说、文字冒险 | [Naninovel](https://naninovel.com/guide/index) | 提供文本脚本语言、故事编辑器、对话和叙事功能，适合分支剧情、视觉小说和过场叙事。自定义玩法或第三方集成通常还需要 C#。 |
| 即时战略 | [Unity RTS Engine](https://docs.gamedevspice.com/rtsengine/) | 包含单位、建筑、资源和指令等 RTS 系统，可通过模块扩展。部分文档较早，正式采用前需检查当前 Unity 版本、模块兼容性及联机实现。 |
| 塔防 | [Tower Defense Starter Kit](https://marketplace.unity.com/packages/tools/game-toolkits/tower-defense-starter-kit-128727) | 提供路径编辑、敌人波次、防御塔放置和基础界面，适合快速建立塔防玩法。商品页列有 Unity 6.0 至 6.6 兼容信息；若项目依赖 TDTK-4 的特定功能，另行验证其版本兼容性。 |
| 赛车 | [Racing Game Starter Kit](https://assetstore.unity.com/packages/templates/systems/racing-game-starter-kit-22615)，搭配车辆物理系统 | RGSK 提供比赛流程、圈数和名次、AI、回放、锦标赛与界面；车辆物理可按玩法比较 [NWH Vehicle Physics 2](https://assetstore.unity.com/packages/tools/physics/nwh-vehicle-physics-2) 和 [Realistic Car Controller Pro](https://assetstore.unity.com/packages/tools/physics/realistic-car-controller-pro-178967)。RGSK 与车辆物理解决不同问题，需核对各自的接口和许可。 |
| 生存、采集、制作、建造 | [Survival Engine](https://assetstore.unity.com/packages/templates/systems/survival-engine-crafting-building-farming-178160) | 提供背包、制作、建造、采集、战斗与存档等基础系统，适合生存冒险原型和模拟玩法。开放世界项目需重点验证存档规模、寻路、网络同步和内容流送能力。 |
| 农场与生活模拟 | [Farming Engine](https://assetstore.unity.com/packages/templates/systems/farming-engine-191881) | 包含种植、牲畜、背包、制作、建造和存档系统，适合农场经营原型。复杂经济、日历和长周期模拟通常需要扩展现有数据模型。 |
| 三消益智 | [MK Match-3 Kit](https://assetstore.unity.com/packages/templates/systems/mk-match-3-kit-candy-sweet-game-template-275837) | 完整三消模板，商品介绍列有多个关卡、地图、编辑器、道具及移动端变现功能。适合快速验证经典交换消除玩法；需要检查目标平台 SDK 和变现组件。 |
| 传统格子 Roguelike | [Ultimate Roguelike Framework](https://assetstore.unity.com/packages/tools/game-toolkits/ultimate-roguelike-framework-traditional-roguelike-starter-kit-363050) | 面向 Unity 6 的网格制 Roguelike 基础，适合回合制移动、地牢探索和单局流程。动作型 Roguelite 通常更适合从角色控制或俯视角框架开始。 |
| 卡牌构筑、卡牌 Roguelike | [Game Card System](https://assetstore.unity.com/packages/templates/systems/game-card-system-gcs-396692) | 提供卡牌战斗运行时、统一数据管理和 FlowGraph 可视化编辑，适合卡牌构筑和战斗玩法。复杂规则、服务器验证和牌组平衡仍由项目实现。 |
| 音乐节奏游戏 | [Rhythm Game Starter](https://www.rhythm.game/) | 提供 MIDI 导入、谱面制作和序列编辑工具，并按音频时钟同步节奏。适合轨道式和下落式音游；需针对目标设备测量输入延迟并提供玩家校准。 |

### 联机框架

联机框架负责网络同步、连接和权威模型，本身不替代玩法框架。先确定玩家规模、服务器权威、延迟敏感度、主机模式或专用服务器，以及托管预算，再选择网络层。

| 网络方案 | 适用场景 | 选择依据 |
|---|---|---|
| [Netcode for GameObjects](https://docs.unity.com/en-us/multiplayer/netcode/netcode) | 小规模合作游戏，或为现有 GameObject/MonoBehaviour 项目增加联网 | Unity 官方方案，与 Unity Multiplayer Services、Relay 等配合；适合先建立基础联机。 |
| [Photon Fusion 2](https://doc.photonengine.com/fusion/v2/fusion-intro) | 对状态同步和实时响应要求较高的动作或竞技玩法 | 支持 Shared、Host 和 Server 等拓扑；确认 Photon 服务、托管方式和用量费用符合项目预算。 |
| [FishNet](https://fish-networking.gitbook.io/docs/overview/readme/features) | 需要客户端预测和较多网络功能的 GameObject 项目 | 提供预测、回滚相关功能和多种传输层；检查当前 Unity 兼容性，并规划服务器与中继服务。 |
| [Netcode for Entities](https://docs.unity.com/en-us/multiplayer/netcode/netcode) | 已采用 ECS/DOTS、需要高吞吐数据导向架构的多人项目 | 与 Entities 技术栈配套，适合愿意承担 ECS 学习与工具链成本的团队；与传统 GameObject 工程的代码结构差异较大。 |

大型 MMO、持久化世界或跨区服务通常需要独立的服务器模拟、数据库、匹配和运维架构。单独安装联网 SDK 无法提供完整 MMO 服务端。

## 开发步骤 / 指令

```
1. 厘清目标与约束（先问后做）
   - 目标平台（移动/主机/PC/WebGL/XR）、目标帧率与机型档位、画面档次、是否联机。
   - 这些直接决定管线、资源预算与架构，缺失则先澄清。

2. 选渲染管线
   - URP：移动/中端/跨平台默认首选，可扩展、性能可控。
   - HDRP：仅 PC/主机高画质（实时光追、体积光），移动端禁用。
   - Built-in：仅遗留项目维护；新项目不选，必要时规划迁移。
   - 自定义效果走 Shader Graph（可视化原型）或 HLSL（精细控制）；重计算用 Compute Shader / VFX Graph。

3. 架构落地（按规模选型，勿过度设计）
   - 数据驱动用 ScriptableObject；状态用状态机；解耦用事件/观察者。
   - 高频实例化（子弹、特效、敌人）用对象池，杜绝运行时频繁 new/Destroy。
   - 海量同质实体（大规模战斗/集群）才上 DOTS-ECS + Job System + Burst；小项目别滥用。
   - 根据「按游戏类型选择框架」选择候选工具包；先确认版本、管线、依赖、许可和扩展方式，再决定是否采用。
   - 任务要求插件时，先执行「插件依赖门槛」；插件或 Unity 项目未提供时，暂停依赖插件的实现并请求相应文件。

4. 性能优化（Profiler 驱动，先测后改）
   - 用 Unity Profiler 定位是 CPU / GPU / 内存 哪一类瓶颈，再针对性优化。
   - 渲染：Frame Debugger 查 DrawCall/SetPass；开启遮挡剔除、LOD、合批、纹理压缩与流式加载。
   - 内存：Memory Profiler 查堆与原生内存；消除每帧 GC 分配（缓存数组、避免 LINQ/装箱/字符串拼接、复用集合）。
   - 物理：减小 FixedTimestep 负担、用层级碰撞矩阵裁剪、简化碰撞体。

5. 资源管理
   - 用 Addressables 做动态加载与远程内容，替代散乱 Resources/手搓 AssetBundle。
   - 纹理选对压缩格式（移动端 ASTC）、音频按场景选压缩与加载方式、网格做 LOD。
   - 大资源用 Git LFS；规划资源目录与依赖，防循环引用。

6. 联机（如需要）
   - 按玩家规模、权威模型和延迟要求比较 Netcode for GameObjects、Photon Fusion、FishNet 或 Netcode for Entities。
   - 做客户端-服务器同步与延迟补偿；按需接 Relay/Lobby/专用服务器。

7. 测试与打包
   - 用 Unity Test Framework 写 EditMode/PlayMode 测试；做内存泄漏与性能回归基线。
   - 各平台单独配置 Player Settings 与签名；Unity Cloud Build / CI 自动出包。
   - 上线前在目标真机 Profiler 实测，按平台认证要求过审。
```

规则：
- 性能从工程伊始就纳入设计，而非后期补救；但优化必须 Profiler 先行，不靠猜。
- 给生产级 C#：含错误处理与日志，遵循 Unity 命名/编码规范。
- 每个方案显式标注目标平台限制与取舍。
- 推荐商业扩展时，注明其属于第三方工具，并以官方商品页和文档确认版本、依赖、许可与价格；不把厂商宣传语当作独立评测结论。

## 示例

最小提示词（让 Agent 产出方案）：
```
目标：[平台/帧率/画面档/是否联机]。
基于 Unity 6 LTS 给出：1) 渲染管线选型及理由；2) 关键架构（含对象池/数据驱动/是否上 DOTS）；
3) 三条最可能的性能瓶颈与 Profiler 验证方法；4) 资源加载方案；5) 打包与真机验证清单。
不臆断帧率，凡结论标注需真机实测。
```

避免每帧 GC 分配（高频踩坑）：
```csharp
// 反例：Update 中每帧分配，触发 GC 卡顿
void Update() {
    var hits = Physics.OverlapSphere(pos, r);          // 每帧 new 数组
    var names = enemies.Where(e => e.alive).ToList();  // LINQ + ToList 分配
}

// 正例：预分配 + 非分配 API，复用缓冲
readonly Collider[] _buf = new Collider[32];
void Update() {
    int n = Physics.OverlapSphereNonAlloc(pos, r, _buf); // 复用 _buf，零分配
    for (int i = 0; i < n; i++) { /* 用 _buf[i] */ }
}
```

对象池替代频繁实例化：
```csharp
// 用 UnityEngine.Pool.ObjectPool<T> 复用，避免 Instantiate/Destroy 引发 GC 与卡顿
var pool = new ObjectPool<Bullet>(
    createFunc: () => Instantiate(prefab),
    actionOnGet: b => b.gameObject.SetActive(true),
    actionOnRelease: b => b.gameObject.SetActive(false));
```

## 注意事项

- 管线选型不可逆性强：URP/HDRP 项目中途互转代价高，开工前定死。
- HDRP 不要用于移动端；WebGL 不支持多线程，Job System/部分特性受限，需降级方案。
- DOTS-ECS 学习与重构成本高，仅在确有大规模实体性能需求时引入，否则徒增复杂度。
- 优化禁忌「凭感觉」：必须 Profiler/Frame Debugger/Memory Profiler 量化前后对比，且在目标机型实测。
- 字符串拼接、`Find`/`GetComponent` 放在热路径、未缓存的 `Camera.main`、装箱、闭包捕获都是常见 GC 来源，逐一排查。
- 跨平台输入统一用新版 Input System，勿混用旧 Input Manager。
- 大资源进 Git 前配好 Git LFS，否则仓库迅速膨胀且难回退。
- 第三方框架可能限制 Unity 版本、渲染管线、平台或与其他插件的组合；先用小型验证场景检查核心玩法，再纳入主项目。
- 使用任何 Unity 插件前，必须能检查本地 Unity 项目和对应插件文件；缺少其中任一项时，提示提供项目或插件文件，并暂停插件相关实现。
- 未取得所需插件文件时，不编写插件功能的替代实现，也不模拟插件 API；仅处理与该插件无关的工作。

## 互见

- requires：无（建议先具备基础 C# 与 Unity 编辑器操作能力）。
- related：`code-reviewer`（审查产出的 C# 脚本正确性与可读性）。
- combines_with：以本文件 frontmatter 的 `combines_with` 列表为准。

---
采编自 sickn33/antigravity-awesome-skills（MIT 许可）；适配重写而非逐字翻译，聚焦 Unity 6 LTS 可执行实践。
