# 性能与平台流程

## 开发步骤

1. **厘清目标与约束**：确认目标平台（移动、主机、PC、WebGL 或 XR）、目标帧率与机型档位、画面档次及联机需求。这些信息决定管线、资源预算和架构；关键信息缺失时先澄清。
2. **选择渲染管线**：URP 适合移动、中端与跨平台项目；HDRP 面向 PC/主机高画质项目，移动端不适用；Built-in 用于维护遗留项目，新项目优先评估 URP。自定义效果可用 Shader Graph 原型或 HLSL 精细控制；高负载计算可评估 Compute Shader 或 VFX Graph。
3. **按规模组织架构**：数据驱动内容可用 ScriptableObject；状态可用状态机；模块解耦可用事件。高频实例化对象考虑对象池。只有海量同质实体等吞吐需求明确时评估 DOTS、Job System 与 Burst。框架按 `game-framework-selection.md` 选择；依赖插件时先执行技能入口的插件依赖门槛。
4. **用 Profiler 诊断性能**：先定位 CPU、GPU 或内存瓶颈，再针对性修改。Frame Debugger 检查 Draw Call 与 SetPass；Memory Profiler 检查托管堆与原生内存。物理负载可通过 Fixed Timestep、碰撞层矩阵与碰撞体复杂度调整。
5. **规划资源管理**：动态或远程内容可评估 Addressables，避免散乱使用 Resources 或自行维护 AssetBundle。按平台选择纹理压缩格式（移动端可评估 ASTC），按场景选择音频压缩及加载方式，并为网格规划 LOD。大型资源使用 Git LFS；规划资源目录与依赖，避免循环引用。
6. **设计联机**：如项目需要联网，按玩家规模、权威模型和延迟要求比较 Netcode for GameObjects、Photon Fusion、FishNet 或 Netcode for Entities；按需接入 Relay、Lobby 或专用服务器。
7. **测试与构建**：按需使用 Unity Test Framework 建立 EditMode/PlayMode 测试和性能基线；各平台分别配置 Player Settings 与签名，可使用 CI 自动构建。发布前在目标真机使用 Profiler 实测，并按平台要求验证。

## 性能规则

性能从工程设计阶段纳入考虑；具体优化由 Profiler 数据驱动，不凭感觉推断。报告生产级 C# 方案时包含必要错误处理与日志，遵循 Unity 命名和编码规范，并标注目标平台限制与取舍。

避免在高频路径产生临时分配：检查字符串拼接、`Find`、未缓存的 `GetComponent` 与 `Camera.main`、装箱、闭包捕获及 LINQ。跨平台输入优先统一使用新版 Input System；WebGL 不支持多线程，Job System 等特性需要平台降级方案。HDRP 不适用于移动端。URP 与 HDRP 转换成本较高，开始项目前确认管线方向。DOTS 有学习与迁移成本，应由实体规模或性能数据证明其价值。

## 示例：复用物理查询缓冲

```csharp
readonly Collider[] _buffer = new Collider[32];

void Update()
{
    int count = Physics.OverlapSphereNonAlloc(position, radius, _buffer);
    for (int i = 0; i < count; i++)
    {
        Process(_buffer[i]);
    }
}
```

## 示例：对象池

```csharp
using UnityEngine;
using UnityEngine.Pool;

ObjectPool<Bullet> pool = new ObjectPool<Bullet>(
    createFunc: () => Instantiate(bulletPrefab),
    actionOnGet: bullet => bullet.gameObject.SetActive(true),
    actionOnRelease: bullet => bullet.gameObject.SetActive(false));
```

对象池适用于高频复用的实例；创建、取出、归还和销毁时机应由玩法生命周期管理。

## 验证清单

- 在目标平台和目标设备上运行 Profiler，记录优化前后数据。
- 核对 Player Settings、签名、输入、资源格式及平台专属限制。
- 检查大资源是否按项目要求使用 Git LFS，以及资源依赖是否存在循环引用。
- 商业扩展以官方商品页和文档核对版本、依赖、许可与价格；厂商介绍作为产品声明，不作为独立性能评测结论。
