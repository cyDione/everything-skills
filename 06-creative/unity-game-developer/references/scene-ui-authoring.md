# 场景与 UI 制作规范

Unity 场景与 Prefab 是可视化编辑资产。CLI、MCP 或脚本可以辅助建立和修改这些资产，但交付应保留可在 Unity Editor 的 Hierarchy、Scene、Inspector 与 Prefab Mode 中检查和修改的对象结构。

## 项目文件夹与资源组织

- Unity 项目根目录中的 `Assets` 保存场景、脚本、Prefab、贴图、模型等项目资产；`Packages` 管理包依赖，`ProjectSettings` 保存项目级设置。这三类内容及其 `.meta` 文件应纳入版本控制。
- `Library` 是本机导入缓存，`Temp` 是临时数据，`Logs` 保存编辑器日志；这些目录通常排除在版本控制之外。`UserSettings` 保存个人偏好与编辑器布局，团队项目通常也排除。不得手工修改 `Library` 内容。
- Unity 没有强制统一的 `Assets` 子目录模板。采用描述性、统一的目录名称；项目自有资产与第三方插件保持分区，第三方资源按其说明保留原有目录结构。
- 中小型项目可从以下层级开始，再按实际内容增删。`_Project`、`ThirdParty` 和其余普通目录名均为团队约定，不具备 Unity 特殊语义。

```text
Assets/
  _Project/
    Scenes/
      Bootstrap/
      Gameplay/
    Features/
      Combat/
        Scripts/
        Prefabs/
        Data/
      UI/
        Scripts/
        Prefabs/
        Art/
    Shared/
      Scripts/
      Art/
    Audio/
    Settings/
  ThirdParty/
```

- 场景文件归入 `Scenes` 等项目约定目录；场景 Hierarchy 中的 `--Scene--` 对象只是场景内组织方式，和磁盘目录无关。
- 资源规模较小时，可按功能归类；某个功能的资源增多后，再在该功能内按 `Scripts`、`Prefabs`、`Data`、`Art` 等类型拆分。命名优先采用清晰且一致的英文单词，不使用空格；测试和实验场景应与正式内容分开。
- 每个 Unity 资源和目录的 `.meta` 文件保存 GUID 及导入设置，必须与对应资产一同纳入版本控制并保持配对。优先通过 Unity Project 窗口移动或重命名资产；在文件管理器中操作时，同时移动或重命名 `.meta` 文件，避免资源引用和脚本组件断链。

### Unity 保留目录

以下目录名会触发 Unity 特定行为；创建前确认用途，避免把普通业务资源误放其中。

| 目录名 | Unity 行为与位置规则 |
|---|---|
| `Editor` | 存放编辑器专用代码，不进入运行时 Player；其 `MonoBehaviour` 不能挂到 GameObject。可分布在 `Assets` 子目录中。 |
| `Editor Default Resources` | 编辑器代码通过 `EditorGUIUtility.Load` 按需读取资源；整个项目只允许一个，位于 `Assets` 根目录。 |
| `Gizmos` | 为 `Gizmos.DrawIcon` 提供 Scene 视图图标；整个项目只允许一个，位于 `Assets` 根目录。 |
| `Plugins` | 存放第三方插件；平台相关的子目录和导入设置应遵照插件说明。 |
| `Resources` | 供 `Resources.Load` 按路径运行时加载；可出现在 `Assets` 子目录中，但其中资源会纳入 Player 构建。只放确需此加载方式的资源，避免构建体积和内存无序增加。 |
| `StreamingAssets` | 文件以原始形式复制到目标平台；整个项目只允许一个，位于 `Assets` 根目录。路径访问方式因平台而异。 |

- Unity 导入时会忽略普通隐藏目录、以点号开头的文件或目录、以 `~` 结尾的文件或目录、名为 `cvs` 的内容及 `.tmp` 文件；`StreamingAssets` 内的点号开头内容属于例外。普通项目目录使用 `_` 等显式前缀表达用途。

## 场景层级

- 修改场景前检查现有 Hierarchy、场景资产与项目命名约定；新对象加入已有职责分组，避免散落在场景根节点。
- 每个场景可用命名清楚的空 GameObject 作为场景对象容器，例如 `--Scene--`。该名称是团队约定，不是 Unity 内置语义；`.unity` 文件才是场景资产本身。
- 场景根下的主要功能分组建议控制在两至三类。常见示例为 `Environment`、`Gameplay`、`Systems`；按项目内容调整，Canvas 可置于 `Systems/UI`。复杂场景仅在职责清楚时增加分组。
- 空 GameObject 用于收纳相关对象、表达共同变换或承载场景级组件。保持层级浅且可读，避免多余的空父对象和过深嵌套。
- 运行时新增实例应放入所属系统或容器之下；按需建立明确的动态对象分组，避免实例散落在根层级。

## 脚本挂载与引用

- 承担场景行为的 `MonoBehaviour` 应挂载在职责对应的场景 GameObject 上。场景级管理逻辑可挂载在命名明确的空对象上；Prefab 行为脚本应作为组件保存在对应 Prefab 中。
- 新增脚本时同时建立明确的组件宿主并配置必要组件与 Inspector 引用。脚本文件存在于场景目录，不等于其行为已连接到场景对象。
- 可由 Inspector 配置的对象、组件与 Prefab 引用优先使用序列化字段并在 Editor 中连接。避免为固定场景引用额外使用字符串查找或运行时扫描。
- 纯数据可使用 `ScriptableObject`；静态工具类、普通 C# 服务类和 Editor 扩展按其用途组织，无需强行挂到 GameObject。需要依赖生命周期、启用状态或场景引用的逻辑才使用合适的 `MonoBehaviour` 宿主。
- 禁止把脚本挂在没有明确职责的场景根对象上。确需集中管理多个子系统时，使用命名清楚的管理对象，并通过显式引用说明责任范围。

## uGUI 与界面 Prefab

- 对于常驻界面和结构稳定的面板，优先在 Unity Editor 中搭建 Canvas、RectTransform、UI 子对象及组件，再保存为场景对象或 Prefab。界面结构、组件与引用应能在 Hierarchy、Inspector 和 Prefab Mode 中检查及修改。
- 仅要求界面布局或视觉元素时，交付 Prefab 或场景层级；只有任务明确要求交互逻辑或代码时才增加脚本。创建按钮等交互控件时，先正确配置对应 uGUI 组件，控件本身不意味着需要额外的自定义脚本。
- 界面行为脚本挂在负责该行为的 UI GameObject 上，或挂在该面板的根对象上。优先用序列化字段连接按钮、文本、图像、面板与数据视图；避免在代码中按对象名称查找界面元素。
- 布局优先使用锚点、Pivot、Layout Group、Layout Element、Content Size Fitter 等 Editor 可配置组件。除非布局确实由运行时数据或算法决定，避免在代码中逐个创建控件并赋予坐标、尺寸、锚点和样式。
- 对背包条目、列表项、通知、对话选项等数量由运行状态决定的重复元素，先制作完整 UI Prefab，包含视觉层级、所需 uGUI 组件及该元素自己的行为脚本，再由运行时代码实例化并更新数据。
- 动态实例化只负责数量、生命周期、数据和必要事件更新；稳定视觉层级、组件设置与默认布局保留在 Prefab 和 Inspector。作为 Layout Group 子项时，让布局组件负责排布；仅在设计确实要求自由定位时设置 `RectTransform`。
- 运行时创建整页 UI、逐元素构造视觉树或用代码替代可视化布局，须有明确需求，例如完全由程序生成的调试界面或算法生成的图表，并说明 Prefab、场景对象及布局组件无法满足的原因。

## 制作后检查

- 在 Editor 中复核场景层级、组件宿主、Inspector 引用、Prefab 引用和 UI 布局；验证界面元素尺寸、锚点、遮挡顺序与目标分辨率下的表现。
- 对有交互的 uGUI，检查场景中恰有一个有效 `EventSystem`、Canvas 上存在 `GraphicRaycaster`，且按钮回调按需求连接。
- 使用 CLI、MCP 或自动化脚本生成对象后，仍需检查并保存场景或 Prefab 资产；不要只创建运行时脚本而未产出可编辑的界面对象结构。
