---
name: agentrelay-local-rebuild
description: 在用户要求本机验证时，为 Windows 上的 AgentRelay 构建 Electron EXE、安全替换现有安装、重启服务并验收；保留配置、会话和任务。适用于本地重打包、覆盖安装、重载及排障；正式发行另配合 git-flow-release，不自动授予发布权限。
---

# AgentRelay 本机重打包与重载

从 Antigravity 同名技能同步的 Codex 适配版。仅适合已获 AgentRelay 私有源码访问权限的维护者；本公开仓库不包含应用源码，普通用户不要尝试从这里执行源码构建。<source-workspace> 须替换为实际获授权的源码目录。执行时以当前用户指令、仓库 AGENTS.md、实际构建配置和运行现场为准。

## 平台与授权

- 仅 Windows x64 EXE 安装包：`AgentRelay-<version>-win-x64.exe`。不构建或发行便携 ZIP、macOS、Linux 产物，不跑其他平台的编译或兼容测试。
- 使用普通 Windows 宿主环境，不自行启用 OS 开发沙盒，不降低文件完整性级别。产品自己的 Electron/浏览器安全沙盒不在此禁令范围。
- 本地替换与重启需在用户授权范围内；发版使用 `git-flow-release`。构建、安装、推送和公开发行分别记录，不把本机测试当作发布成功。
- 在本项目先用 `unified-branch-dev` 检查共享编辑锁；只在 `feature/unified` 开发，不因本技能另建 feature/worktree。与其他写入者协调，避免并发修改共享源码或 dist。

## 确认现场

1. 核对源码绝对目录、git common-dir、分支、未提交与未跟踪文件、全部 worktree、package 版本和 electron-builder.yml。历史路径只作候选，不能重置或覆盖别人工作。
2. 核对安装程序完整路径与 PID、启动时间、数据目录、端口、现有任务和压缩操作。Token 从本地配置读取，不打印或写入公开材料。认证只读查询 `/health`，不能仅凭进程存在判断服务可用。
3. 当前常见位置：源码 `<source-workspace>`；程序 `%LOCALAPPDATA%/Programs/AgentRelay/AgentRelay.exe`；数据 `%APPDATA%/AgentRelay`。逐项以当前现场验证。
4. 本机替换前等待活跃任务、队列、恢复与压缩排空，并协调调用方暂停新投递。历史 unknown 保留原编号，不能重发或当作当前运行锁。无法安全排空时保留现场并报告具体阻塞。
5. 执行 EXE 前检查完整路径和 `icacls` 输出，确认没有意外 Low 标记。若需修复，只处理确认受影响的对象，先保留原 ACL/完整性证据；禁止为了开发可写而降为 Low。

## 构建和验证

1. 检查当前 Node 版本符合 package engines；遵守版本维护，不为同一未交付迭代反复升版。不要清除别人正在使用的依赖或产物。
2. 执行 `npm run check`、相关回归、`npm run build:desktop` 和 `npm run test:desktop`。安装、启动或更新相关变化增加对应测试。只选 Windows 宿主测试，不调用包含其他平台的测试集合。
3. 安装包构建前必须通过 `node scripts/agent-lock.js build-check`；工作区、分支与 vendor 校验通过后使用 `npm run package:desktop`。旧 `build:exe` 是 SEA/WebView2 路径，不作为 Electron 交付替代。
4. Windows 配置只允许 NSIS x64。核对最终 `dist/electron/AgentRelay-<version>-win-x64.exe`、大小、SHA-256 与完整性标记；不要把遗留旧 ZIP/Mac 产物加入本轮上传。
5. 使用真实打包 EXE 对应应用或隔离数据实例进行 Electron CDP/UI 验收。`electron-cdp-acceptance` 要求覆盖宽窄视口、主流程、必要失败状态与保留数据；截图须实际查看。

## 安全升级现有安装

1. 用户数据、配置、令牌、SQLite 与升级记录先备份并核验。活跃任务安全排空后，通过支持的 `POST /ui/api/quit` 请求退出，带正确 Bearer 和 Origin。
2. 确认目标主进程、子进程、端口与数据文件释放。退出失败先核对真实完整路径和运行任务，不按映像名批量强杀所有 AgentRelay，不清除未知请求。
3. NSIS 支持 `/S` 静默覆盖。PowerShell 使用完整路径：`Start-Process -FilePath '<installer.exe>' -ArgumentList '/S' -Wait -PassThru -WindowStyle Hidden`。核对退出码与实际安装文件，保留用户数据。
4. 优先通过系统正常应用启动/用户桌面入口启动。需要用户看见的验收窗口可以显示；辅助后台启动默认隐藏窗口。仅在已证明普通启动无法进入交互桌面且用户授权时考虑临时交互计划任务，使用本轮唯一名字，不覆盖已有任务，完成后清理。不得将计划任务用作绕过平台强制沙盒或权限限制的手段。
5. 已在运行时可核对同一完整路径的单实例二次唤醒，不能为了显示窗口启动另一数据写入实例。

## 验收与发行交接

- 确认实际进程路径、目标窗口、认证 `/health` 的版本、现有配置与任务保留；磁盘新包不等于旧进程已升级。
- 根据本轮改动打开真实控制面板及对应独立看板：`/ui`、`/ui/antigravity`、`/ui/grok-cli`。它们的可用性以当前后端声明和现场为准。
- 区分测试夹具、真实原生投递与业务操作。投递受理不等于收到或任务完成；未知操作只查原凭据。
- 正式发行遵守 Git Flow、README 与 doc 同步、全部 feature/worktree 价值盘点、最终合并后重建和远程核验。只上传 Windows EXE 和必要校验说明，不上传未验证平台产物。
- 现有 ZIP 自动更新器不能据 EXE-only Release 宣称自动升级已生效。没有经过实际适配与验证时明确手动 EXE 升级路径。
- 交付报告源码/构建/安装运行/公开发行各自版本、SHA、测试、截图与限制。结束及失败时释放自己的编辑锁。
