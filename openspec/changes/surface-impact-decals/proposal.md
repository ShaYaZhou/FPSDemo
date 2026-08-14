# 变更提案：按物理表面选择受击贴花

## 背景

当前枪械命中由 `BP_BaseWeapon.PlayEffects` 根据 `EPhysicalSurface` 选择声音和 Niagara，但所有非 Body/Head 表面共享单一 `ImpactDecal`。项目已经新增 `MI_Decal_PhyMaterial07Wood`，但尚未被任何命中逻辑引用。

## 目标

- 先让 `SurfaceType30 / PhyMaterial_07Wood` 命中时选择 `MI_Decal_PhyMaterial07Wood`。
- 未配置专用贴花时继续回退到现有 `ImpactDecal`，不改变当前 Body/Head 不生成贴花的行为。
- 为 `DefaultEngine.ini` 中 SurfaceType1-31 补齐按物理表面命名的 `MI_Decal_PhyMaterialXX` 资产。
- 将选择逻辑收敛为可维护的 Surface Feedback 映射，后续可统一管理 Niagara、声音和贴花。

## 非目标

- 本次不擅自为各表面设计新的贴花纹理或美术风格。
- 本次不改变伤害、命中判定、网络 RPC 或 Body/Head 血液反馈逻辑。
- 不直接在文件系统移动、重命名或删除 Unreal Content 资产。

## 验证

- 通过 Unreal Editor/MCP 检查每个 MI 的父材质、资产类型和命名。
- 编译 `BP_BaseWeapon`，确认无蓝图错误。
- PIE 中分别命中 `PhyMaterial_07Wood` 与无专用配置的表面：前者选择木材 MI，后者使用原 `ImpactDecal`。
- 验证 Body/Head 仍不生成通用弹孔贴花。
