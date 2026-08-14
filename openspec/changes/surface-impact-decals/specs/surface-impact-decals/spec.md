# Surface Impact Decals 规格增量

## 新增需求

### 物理表面专用贴花

系统必须允许每个 `EPhysicalSurface` 配置独立的 `MaterialInterface` 贴花，并在命中时按 `HitResult.PhysMat.SurfaceType` 选择。

#### 场景：命中 07Wood

- 给定命中返回 `SurfaceType30 / PhyMaterial_07Wood`
- 当 `BP_BaseWeapon.PlayEffects` 处理命中
- 则 Spawn Decal 使用 `MI_Decal_PhyMaterial07Wood`

#### 场景：表面没有有效专用贴花

- 给定映射中没有有效 MaterialInterface
- 当命中进入通用贴花分支
- 则继续使用武器现有 `ImpactDecal`

#### 场景：命中 Body 或 Head

- 给定 SurfaceType1 或 SurfaceType2
- 当命中反馈播放
- 则保持血液反馈路径，不生成通用弹孔贴花

### 资产命名完整性

系统必须为 `DefaultEngine.ini` 中 SurfaceType1-31 提供与物理表面名称一致的 `MI_Decal_PhyMaterialXX` 资产；所有资产必须是可供 `Spawn Decal at Location` 使用的 MaterialInterface。
