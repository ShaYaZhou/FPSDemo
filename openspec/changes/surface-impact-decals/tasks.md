# 任务清单

- [x] 盘点 SurfaceType1-31 与目标 MI 名称，检查命名冲突。
- [x] 核对当前贴花母材质参数与 `MI_Decal_PhyMaterial07Wood` 的用户改动。
- [x] 在 Unreal Editor 中补齐全部 `MI_Decal_PhyMaterialXX`，统一父材质并保存。
- [x] 在 `BP_BaseWeapon.PlayEffects` 落地 SurfaceType30 最小选择和 `ImpactDecal` 回退。
- [x] 扩展为 SurfaceType1-31 的完整专用贴花映射；Body/Head 保持不生成通用贴花。
- [x] 编译、保存并检查资产引用、蓝图错误和材质类型。
- [x] PIE 启动/退出冒烟测试，确认 Warehouse1 正常进入 PIE 且无近期蓝图运行时错误。
- [x] 将旧 `/Game/FPS_Controller/PhysicalMaterials/PM_Wood` 从 SurfaceType4 迁移为标准 Wood 的 SurfaceType30，并核对原引用者。
- [ ] 在可控测试场景中逐项实射验证 Wood、Common/Default、至少一种其他表面和 Body/Head。
- [x] 记录仍缺少专用纹理覆盖的 MI，交由后续美术补充。

## 实现记录

- `M_Decal_BulletHole` 保留用户已有的 `BaseTexture`、`AlphaTexture` 与 `Color` 参数化改动。
- 31 个专用 MI 均位于 `Effects/Materials/BulletHole`，并由 `BP_BaseWeapon` 的 `Select on EPhysicalSurface` 直接引用。
- `SurfaceType_Default` 继续读取每把枪原有的 `ImpactDecal`，避免破坏枪械子类的回退配置。
- SurfaceType1/2 的执行流仍只进入血液反馈，不进入通用 `Spawn Decal at Location`。
- 新建 MI 暂时继承母材质默认外观；除已有 Wood MI 外，尚无经过确认的表面专用纹理/颜色覆盖，不在本次实现中臆造美术差异。
- `/Game/FPS_Controller/PhysicalMaterials/PM_Wood` 已迁移为 SurfaceType30；`SM_BigBox`、`SM_Palet` 与 `MultiplayerTest_level` 保持原引用，无需重新绑定。
