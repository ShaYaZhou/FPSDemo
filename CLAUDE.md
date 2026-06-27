# FPSDemo 项目说明

## 默认工作方式

- 面向用户的说明、计划、总结和文档默认使用中文。
- 修改现有内容时保持外科手术式改动，只改和当前任务直接相关的文件。
- 不要提交 Unreal 本地缓存、构建产物或 IDE 状态文件。
- 涉及较大功能、资产组织或工程规则变更时，先使用 OpenSpec 建立变更说明，再实施。

## 工程概览

这是一个公版 Unreal Engine 5.7 C++ 工程，仓库名为 `FPSDemo`。当前 C++ 层只提供最小 `FPSDemo` 模块，玩法主体在蓝图和内容资产中实现。

关键路径：

- `FPSDemo.uproject`：UE 5.7 工程入口，声明 `FPSDemo` Runtime 模块。
- `Source/FPSDemo/`：主游戏模块源码。
- `Config/DefaultEngine.ini`：默认地图、GameMode、GameInstance、渲染、碰撞、Steam 网络等配置。
- `Config/DefaultInput.ini`：旧输入映射和 Enhanced Input 组件配置。
- `Content/FPS_Controller/`：主要玩法资产，包括角色、武器、附件、动画、UI、地图、输入和蓝图。
- `Content/Art/`、`Content/Explosions_Niagara/`、`Content/ExplosionsMegaPack/`：共享美术和特效资产。
- `openspec/`：规格驱动变更目录。

## 实现理解

- 工程是蓝图主导的 FPS 多人控制器示例。
- 默认地图是 `Warehouse`，另有 `MainMenu` 和 `MultiplayerTest_level`。
- 默认 GameInstance 为 `/Game/FPS_Controller/Blueprints/Game/BP_GameInstance.BP_GameInstance_C`。
- 默认 GameMode 为 `/Game/FPS_Controller/Blueprints/Game/BP_GM.BP_GM_C`。
- 输入包括移动、视角、开火、瞄准、换弹、奔跑、下蹲、卧倒、切换武器、配件和菜单等动作。
- 武器和附件资源覆盖 AK、手枪、刀、手雷、阔剑地雷、瞄具、弹匣、枪口、枪托、握把和手电等内容。
- 项目启用了 `AnimationWarping` 和 `MotionWarping` 插件，配置中保留 Steam OnlineSubsystem 设置。

## 构建验证

生成项目文件：

```powershell
& 'E:\Epic Games\UE_5.7\Engine\Build\BatchFiles\Build.bat' -projectfiles -project='.\FPSDemo.uproject' -game -rocket -progress
```

编译编辑器目标：

```powershell
& 'E:\Epic Games\UE_5.7\Engine\Build\BatchFiles\Build.bat' FPSDemoEditor Win64 Development -Project='.\FPSDemo.uproject' -WaitMutex -NoHotReloadFromIDE
```

## 资产和 Git 规则

- `*.uasset`、`*.umap` 和常见 Unreal 二进制资产必须通过 Git LFS 管理。
- 不要在文件系统中直接移动、重命名或删除 `Content/` 资产，避免破坏软引用和路径依赖。
- 新增 C++ 代码时优先保持模块轻量，不为一次性需求引入过度抽象。
- 修改蓝图依赖的配置项时，要检查默认地图、GameMode、GameInstance、输入映射和碰撞通道是否仍一致。
