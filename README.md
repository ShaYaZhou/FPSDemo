# FPSDemo

这是一个基于公版 Unreal Engine 5.7 的 FPS 多人控制器演示工程。工程主体玩法由蓝图资产实现，C++ 部分提供最小运行时模块，方便后续在同一个工程里继续扩展原生代码。

## 工程环境

- 引擎版本：Unreal Engine 5.7
- 工程类型：C++ 工程
- 主要平台：Windows / Win64
- 推荐工具链：Visual Studio 2022、Windows SDK 10.0.22621 或更新版本
- 资源管理：`*.uasset`、`*.umap` 等 Unreal 二进制资产通过 Git LFS 管理

## 内容概览

- `Source/FPSDemo/`：最小 C++ 运行时模块，当前只注册主游戏模块。
- `Config/`：项目设置、输入映射、渲染、网络和默认地图配置。
- `Content/FPS_Controller/`：核心 FPS 控制器资源，包含角色、武器、附件、动画、UI、输入、地图和玩法蓝图。
- `Content/Art/`、`Content/Explosions_Niagara/`、`Content/ExplosionsMegaPack/`：共享美术、特效和 Niagara 爆炸资源。
- `openspec/`：OpenSpec 项目上下文和后续规格变更目录。

默认启动地图为 `/Game/FPS_Controller/Maps/Warehouse.Warehouse`。默认 GameInstance 为 `BP_GameInstance`，默认 GameMode 为 `BP_GM`。项目配置中保留了 Steam 联机相关设置，当前主要逻辑仍以蓝图资产为主。

## 构建和打开

首次克隆后先拉取 LFS 资源：

```powershell
git lfs install
git lfs pull
```

生成项目文件：

```powershell
& 'E:\Epic Games\UE_5.7\Engine\Build\BatchFiles\Build.bat' -projectfiles -project='.\FPSDemo.uproject' -game -rocket -progress
```

编译编辑器目标：

```powershell
& 'E:\Epic Games\UE_5.7\Engine\Build\BatchFiles\Build.bat' FPSDemoEditor Win64 Development -Project='.\FPSDemo.uproject' -WaitMutex -NoHotReloadFromIDE
```

编译成功后，用 Unreal Editor 5.7 打开 `FPSDemo.uproject`。

## 修改注意事项

- 不要在资源管理器中直接移动或重命名 `Content/` 下的资产；需要调整路径时应在 Unreal Editor 内容浏览器内完成。
- 不提交 `Saved/`、`Intermediate/`、`DerivedDataCache/`、`Binaries/`、`.vs/` 和 `.sln` 等本地生成内容。
- 新增 `.uasset`、`.umap` 或贴图源文件时确认它们仍由 Git LFS 跟踪。
- 涉及玩法、资产结构或工程约定的较大改动，先在 `openspec/` 下建立变更说明和任务拆分。
