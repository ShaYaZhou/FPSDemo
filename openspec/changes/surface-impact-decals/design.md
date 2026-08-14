# 设计说明

## 现有链路

`BP_Bullet.HandleHit` 将 `HitResult` 传给 `BP_BaseWeapon.PlayEffects`。后者从 `PhysMat.SurfaceType` 选择音效和 Niagara；Default 与 SurfaceType3-31 汇入同一个 `Spawn Decal at Location`，其材质固定取 `ImpactDecal`。SurfaceType1/2 走血液反馈并绕过贴花节点。

## 第一阶段：最小可用改动

在现有 `Spawn Decal at Location` 前增加按 SurfaceType 选择 MaterialInterface 的逻辑：

- `SurfaceType30`：`MI_Decal_PhyMaterial07Wood`
- 其他表面：原 `ImpactDecal`
- SurfaceType1/2：保持原分支，不进入 Spawn Decal

该阶段不复制 Spawn Decal 节点，以便保持位置、旋转、尺寸、寿命和后续 Niagara 流程一致。

## 第二阶段：全量映射

目标数据结构为 `EPhysicalSurface -> MaterialInterface`，未命中有效条目时回退 `ImpactDecal`。若当前 MCP 对蓝图 Map 类型的创建或编辑不稳定，则先使用 `Select on EPhysicalSurface` 完成等价映射，待安全工具链可用后迁移为 Map/DataAsset。

所有 MI 放置于：

`/Game/FPS_Controller/Effects/Materials/BulletHole/`

命名规则：移除物理表面名中 `PhyMaterial_` 的下划线，前置 `MI_Decal_`。例如：

- `PhyMaterial_07Wood` -> `MI_Decal_PhyMaterial07Wood`
- `PhyMaterial_02Steel_Hard` -> `MI_Decal_PhyMaterial02Steel_Hard`

## 材质策略

所有 MI 继承统一的 Deferred Decal 母材质。若母材质已暴露纹理、颜色、粗糙度等参数，MI 可按表面覆盖；若没有对应美术资源，则先保留母材质默认值，只完成稳定的资产身份和运行时路由，不伪造表面外观。

## 兼容性

- `/Engine/EngineMaterials/PhyMaterial_07Wood` 为 SurfaceType30。
- `/Game/FPS_Controller/PhysicalMaterials/PM_Wood` 已统一为 SurfaceType30；原先引用它的资产会进入 Wood 反馈映射，SurfaceType4 继续保留给 Brick。
- 现有枪械子类的 `ImpactDecal` 保留为回退值。
