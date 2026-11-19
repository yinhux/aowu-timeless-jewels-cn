# 嗷呜永恒珠宝计算器（国服中文化 + PoB 导出 + 市集联动）

本工具基于 Vilsol 大佬的开源项目 [Vilsol/timeless-jewels](https://github.com/Vilsol/timeless-jewels) 优化（原项目 GPL-3.0），做了国服中文化、离线桌面化、POB 文本导出和国服市集联动。

## 功能

- 中文界面、中文天赋树与词缀（编年史国服口径 poedb.tw/cn）
- 独立 Windows 桌面程序（WebView2 外壳，不依赖浏览器），天赋树与计算数据本地离线
- 点珠宝插槽 → 选珠宝/征服者 → 输入种子，或按词缀反向搜索种子
- 命中默认只勾选显著天赋，可切换全选/小天赋
- POB 导出：复制珠宝物品文本，粘贴进 PoB 物品栏
- 国服市集：登录、逆向搜索（按种子筛选）、内置市集浏览
- 市集浏览中每条在售珠宝可「嗷呜计算」，自动解析种子并计算当前插槽词缀
- 批次发送（1-180 条）：左侧批次列表可按目标词缀筛选，只高亮命中位置
- 每个批次条目显示价格（按估值从低到高），「去购买」跳回内置市集对应条目
- 工具栏「设置 PoB 路径」：可指定自己电脑上的 PoeCharm3.exe
- 数据来自 PoB 内置“Find Timeless Jewel”同源；中文译名以 poedb.tw/cn 为准

## 目录

- `calculator/`、`wasm/`、`data/`、`translations/`：Go 计算核心、WASM 入口、内嵌数据与中文翻译
- `frontend/`：SvelteKit 前端（中文界面、天赋树、市集联动）
- `launcher/`：C# WinForms + WebView2 桌面外壳（`DesktopApp.cs`），以及单文件启动器 `SingleExe.cs`
- `scripts/`：构建脚本
- `dev-tools/`：开发与验收脚本（E2E、对比验证等，路径按开发机写死，仅供参考）
- `docs/`：使用说明与发布说明

## 构建（Windows）

前置：Node 18+、Go 1.26+、.NET Framework 4.x 的 csc、Microsoft Edge WebView2 Runtime；前端依赖需自行安装。

```powershell
cd frontend
npm install --legacy-peer-deps --ignore-scripts
cd ..
pwsh scripts/build-cn.ps1        # 生成中文词典 + wasm + 前端 build
pwsh scripts/build-desktop.ps1   # 编译桌面 exe（需要 WebView2 SDK，脚本会下载）
```

单文件分发版：把桌面程序目录（exe、3 个 WebView2 DLL、`site/`、`说明.txt`）打成 `payload.zip`，再用 `csc` 把 `launcher/SingleExe.cs` 与 `payload.zip` 资源编译成一个 exe；首次运行解压到 `%LOCALAPPDATA%\AowuTimelessJewel\app` 并启动。

## 数据与许可

- 游戏数据、天赋树、词缀来自 PoB（Path of Building）数据与编年史国服（poedb.tw/cn）。
- 本项目遵循 **GPL-3.0**，见 `LICENSE`；修改与再分发需保留上游版权声明与相同许可证。
- 上游项目：https://github.com/Vilsol/timeless-jewels
- 作者：B站寅虎嗷呜呜优化（https://space.bilibili.com/438278539）
