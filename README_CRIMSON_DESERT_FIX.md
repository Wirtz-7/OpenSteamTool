# Crimson Desert compatibility fix

本分支基于 matheeshapathirana/OpenSteamTool 的 v1.4.8-netfix2（29c4fbb04d63c34d14ebf5b1edd30aab120af5ea），保留上游 GPL-3.0 许可证。

## 修复内容

- 继承社区 PR #212 的 Steam CNetPacket 布局兼容修复。
- 新增 KnownBuildHash 检测，修复当前 CrimsonDesert.exe 无法被旧入口特征和节名/字符串规则识别的问题。
- 只对文件名 CrimsonDesert.exe、主程序标记及完整 SHA-256 同时匹配的模块生效。其他文件继续使用原有规则。
- 保存补丁作者提供的 Steam Build 1792618285 适配 TOML。

## 已验证版本

- Steam Client Build：1792618285。
- 游戏 EXE 文件版本：1.0.0.3462。
- 游戏 EXE SHA-256：2c98b00e8922f14ac1937b5ac996956c7b9ca2d0fbcb0049dc0436321a09dd77。
- 工具内部版本：1.4.8-netfix2-cd3462-20261101。
- 验证：原检测漏识别、新检测命中、同名不同文件拒绝，三项源码测试通过；实际启动已验证。

本分支的哈希识别仅适用于上述游戏文件。游戏或 Steam 再次更新后应重新检查对应适配。

## 下载与使用

在 Releases 下载 OpenSteamTool-v1.4.8-cd3462-fix1-win64.zip。完全退出 Steam，先备份现有工具 DLL 及配置，再将压缩包内三个 DLL 和 opensteamtool 文件夹复制到 Steam 根目录。保留已有个人配置。

本仓库与发布包均不包含个人 Lua、游戏票据、存档、备份或私人日志。

## 本地编译

测试环境为 MSVC x64、Visual Studio 2026 Build Tools、CMake。安装了对应工具后：

```powershell
cmake -S src -B build -G "Visual Studio 18 2026" -A x64 -DOPENSTEAMTOOL_VERSION=1.4.8-netfix2-cd3462-20261101 "-DCMAKE_CXX_FLAGS_RELEASE=/O2 /Ob2 /DNDEBUG /utf-8"
cmake --build build --config Release --target OpenSteamTool --parallel
```

Release 静态链接运行库，只需 Windows 自带动态组件。构建依赖的确切提交见 fix-build-info.json；源码构建会下载依赖。

## 来源

- 原项目：https://github.com/OpenSteam001/OpenSteamTool
- 基础分支：https://github.com/matheeshapathirana/OpenSteamTool/tree/v1.4.8-netfix2
- Steam 网络包修复：https://github.com/OpenSteam001/OpenSteamTool/pull/212
- 新版 Steam TOML：https://github.com/matheeshapathirana/OpenSteamTool/releases/download/v1.4.8-netfix2/steam-1792618285-tomls.zip
