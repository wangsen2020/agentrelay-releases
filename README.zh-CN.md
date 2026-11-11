# AgentRelay

[English](README.md) · [简体中文](README.zh-CN.md) · [Português (Brasil)](README.pt-BR.md) · [Español](README.es.md)

**2.8.3 · Windows x64 / macOS Apple Silicon** · [下载与发行记录](https://github.com/wangsen2020/agentrelay-releases/releases/latest)

[GoodExts 官网](https://goodexts.com/) · [产品详情](https://goodexts.com/agentrelay) · [XFollower 接入教程](https://goodexts.com/blog/xfollower-agentrelay)

AgentRelay 把本机 Codex、Grok Bot、Antigravity、Grok CLI 与 DeepSeek Harness 连接为统一的本地 AI 服务。插件、网页及其他客户端选择引擎和已有对话，使用同一个会话 ID 持续发送任务，无需每次重新拼接上下文。Codex 和 Antigravity 也支持显式创建新对话。

![AgentRelay 本地 AI 服务和接入设置](https://goodexts.com/images/agentrelay/zh/home.png)

源码在私有仓库维护；公开仓库仅提供产品说明、接入技能和发行文件。三方组件保留各自许可。

## AI 接入 Skill

如果你需要让 AI 编程助手（Codex、Antigravity、Claude Code、Cursor）帮你的浏览器插件、网页或客户端接入 AgentRelay，可直接使用本仓库根目录提供的标准接入 Skill：

- 📖 **Skill 说明文档**：[`agentrelay-integration/SKILL.md`](agentrelay-integration/SKILL.md)
- 📦 **完整接入包下载**：[`agentrelay-integration.zip`](agentrelay-integration.zip)
- 🔌 **包含组件**：
  - `agentrelay-integration/scripts/client.mjs`（原生 ES module 客户端，即拷即用，适配 Chrome MV3 后台）
  - `agentrelay-integration/scripts/probe.mjs`（命令行检测与全流程验证工具）
  - `agentrelay-integration/scripts/self-test.mjs`（本地模拟服务单测，无需真实调用 AI）

将 `agentrelay-integration/` 文件夹放置在你的项目内或 `~/.codex/skills/`，直接向你的 AI 发送接入指令：
> “请读取 `agentrelay-integration/SKILL.md`，按照我的项目分支规则，将当前项目接入 AgentRelay……”

## 2.8.3 更新

- **Grok CLI 与 DeepSeek Harness (DSH) 双新引擎**：完整支持终端持久会话发现、多帧压缩流解码与双向指令投递，原生支持 5 大本地 AI 客户端（Codex、Grok Bot、Antigravity、Grok CLI、DeepSeek Harness）。
- **Grok Bot 核心升级至 0.12.5 (PR166) 与读取恢复**：彻底解决服务端 401 "This version of Grok Bot is no longer supported" 版本过期阻断，并引入会话读取恢复队列，杜绝偶发 502 网关超时抖动。
- **会话原生压缩与所有者任务队列隔离**：针对打标与评论高频业务提供会话级原生压缩支持与执行队列隔离，防止并发任务冲突。
- **Windows 安装程序防占用与完整性保护**：重构安装与覆盖更新进程退出握手，彻底解决多实例进程占用误报与沙盒 Low Integrity 完整性标记冲突。

## 下载 2.8.3

| 平台 | 安装程序 | 便携压缩包 |
| --- | --- | --- |
| Windows 10/11 x64 | [Windows 安装包](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.8.3/AgentRelay-2.8.3-win-x64.exe) | [Windows ZIP](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.8.3/AgentRelay-2.8.3-win-x64.zip) |
| macOS Apple Silicon | [macOS DMG](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.8.3/AgentRelay-2.8.3-mac-arm64.dmg) | [macOS ZIP](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.8.3/AgentRelay-2.8.3-mac-arm64.zip) |

[所有发行版本与 SHA-256 校验](https://github.com/wangsen2020/agentrelay-releases/releases)

## 2.3.5 更新

- 新增独立会话管理窗口，按来源、标题、ID、工作区和任意日期范围筛选。
- Codex 支持批量归档、恢复与删除；Antigravity IDE 删除会话正文和摘要；Grok Bot 可浏览已有 Bot 和复制原始 ID。
- 已归档会话从专用入口查看。刷新和重启后继续遵循原客户端状态，清理已删除的摘要和旧本地别名。
- SQLite 持久保存请求、回执、结果和状态轨迹，启动时先迁移旧任务记录，再打开主窗口。
- 原任务 ID 和原消息回执支持结果恢复；发送状态不明确时保留记录供核对。
- 主窗口单向同步语言与主题，子窗口保留筛选与选择；会话窗口默认更宽，并适配屏幕可用空间。

![AgentRelay 中文会话管理界面，使用演示数据](https://goodexts.com/images/agentrelay/zh/sessions.png)

以下桌面能力继续可用。

- 桌面更新下载沿用系统代理与 PAC，大包下载时限增至 10 分钟；继续逐跳检查域名并校验签名、大小与 SHA-256。
- 修复 Windows 更新助手随旧桌面进程退出而中断；独立后台执行、启动确认与失败回退继续生效。
- Electron + electron-vite 跨平台桌面程序，自带 Chromium，Windows 不再依赖 WebView2。
- 启动引导先等待旧任务结束、备份并迁移数据，再启动服务和检测 AI 客户端；可启动客户端、跳过，或等待 10 秒进入首页。
- 关闭主窗口只隐藏界面，后台服务和任务继续运行；托盘单击、双击恢复原页面和输入。明确“退出”时先等待任务完成。
- 标题栏中英文、深浅色即时切换；统一控件和 10px 间距，修复内容底部裁切与矮窗口侧栏溢出。
- Windows / Mac 分平台签名更新、任务等待、数据备份与失败回退；修复 Windows 更新计划的 UTF-8 路径读取。
- 默认 JSON 包体上限 1 MiB、文本上限 120000 字符；保留已保存的自定义限制。
- Grok Bot 发送前超时可按原任务 ID 有界恢复；发送结果不明只查询原记录，不换 ID 自动重发。

## 安装与首次运行

旧 2.0.4 / 2.0.5 Windows 若提示更新助手已停止，请用 2.0.6 安装包覆盖原目录一次，保留原数据路径；新版修复后续更新的助手启动问题。

| 平台 | 文件 | 用法 |
| --- | --- | --- |
| Windows 10/11 x64 | `AgentRelay-2.3.7-win-x64.exe` | 安装向导，可选目录与快捷方式 |
| Windows 10/11 x64 | `AgentRelay-2.3.7-win-x64.zip` | 解压完整目录，运行 `AgentRelay.exe` |
| macOS Apple Silicon | `AgentRelay-2.3.7-mac-arm64.dmg` | 打开映像，将应用复制到 Applications |
| macOS Apple Silicon | `AgentRelay-2.3.7-mac-arm64.zip` | 解压并运行完整 `AgentRelay.app` |

Windows 默认安装到 `%LOCALAPPDATA%\Programs\AgentRelay`。便携目录中的其他文件也是程序的一部分，不能只复制 EXE。安装版与便携版共享数据目录，不要同时运行；卸载保留业务数据。

双击后直接进入首页，不要求注册或输入 Token。连接失败显示原因与重试入口。右上角关闭仅隐藏窗口；从托盘菜单或“关于”明确退出才停止服务。

应用自带 Node.js。Windows 包含 Antigravity Python；Mac 初版使用已有 Python 3，缺少时明确提示。三种 AI 客户端仍需自行安装、登录并保持可用。系统签名和 Apple notarization 尚未配置，系统可能提示未识别的发行者。

| 引擎 | 准备条件 | 会话 |
| --- | --- | --- |
| Grok Bot | 已安装、登录桌面客户端 | 已有 Bot、刷新、持续调用 |
| Codex | 已安装并登录 Codex CLI | 已有对话、创建、同会话多轮消息 |
| Antigravity | IDE 已启动并登录，语言服务可用 | 发现对话、改名同步、ID 去重、创建及多轮消息 |

Mac ARM64 使用 GitHub CI 验收，尚无用户 Mac 实机、真实 AI 账号验收。

## 数据与旧版本升级

Windows 数据为 `%APPDATA%\AgentRelay`，Mac 为 `~/Library/Application Support/AgentRelay`。配置为其中的 `config.json`，升级历史为旁边的 `AgentRelay.upgrade-history.json`；在 **配置 → 版本升级**查看。

启动迁移先等待旧服务任务排空，再备份、迁移、打开主窗口。兼容 Windows 旧 `%APPDATA%\GrokBridge`，以及 Mac 旧 `.config/agent-relay`、`.config/grok-bridge`。配置、令牌、任务和会话映射保留；无法确认的目录冲突停止并提示。

`AGENT_RELAY_HOME`、`AGENT_RELAY_CONFIG` 可覆盖数据及配置路径；旧 `GBOT_SERVICE_HOME`、`GBOT_SERVICE_CONFIG` 继续兼容。旧 1.x 单文件更新器不能安装 Electron 目录包，首次升级到 2.x 请手动安装或解压本版。历史 1.x 发行源保留兼容，不重打旧标签或替换旧资产。

## 外部接入

默认地址 `http://127.0.0.1:8787`，管理页 `/ui`。首页可复制地址、Token 与请求示例。默认允许所有来源，已有配置保留原值；业务 API 必须带 `Authorization: Bearer <token>`，JSON 请求带 `Content-Type: application/json`。管理接口继续限制本机同源，浏览器独立访问保留认证。

| 操作 | Grok Bot | Codex | Antigravity |
| --- | --- | --- | --- |
| 健康检查 | GET /grokbot/health | GET /codex/health | GET /antigravity/health |
| 列出会话 | GET /grokbot/sessions | GET /codex/sessions | GET /antigravity/sessions |
| 显式创建 | 暂无 API | POST /codex/sessions | POST /antigravity/sessions |
| 发送任务 | POST /grokbot/jobs | POST /codex/jobs | POST /antigravity/jobs |
| 查询任务 | GET /grokbot/jobs/{jobId} | GET /codex/jobs/{jobId} | GET /antigravity/jobs/{jobId} |

创建请求为 `{"name":"我的对话"}`。取列表中的 `id` 用作 `sessionId`：

```json
{"jobId":"调用方生成的唯一编号","sessionId":"选定会话的ID","text":"消息内容","timeoutMs":120000}
```

按原 `jobId` 轮询，`completed` 时读取 `result.text`。同 ID、同内容重复提交返回原任务，不同内容返回 409；同一会话串行执行。`unknown` 不表示未发送，不应换 ID 盲目重发。Grok Bot 已确认发送前失败的恢复沿用原 ID，GET 查询保持只读。

旧 Grok Bot `/bots`、`/ask`、`/send`、`/thread`、`/jobs` 等路由保留；`/ask` 使用 `bot`、`text`、可选 `timeoutMs` 并直接等待回复。测试页支持 Enter 发送、Shift+Enter 换行；会话菜单可搜索、长标题省略，展开不推动输入框。

Antigravity 的可选 `notifyThreadId` 主动回执目前仅支持 Windows Codex 桌面适配。Mac 使用普通任务轮询，主动通知请求在发送前返回不支持。各产品内部通信接口可能随版本调整。ACP 是需显式配置的实验接入，原生客户端路由继续作为默认入口。

## 自动更新

2.x 正式发行源为 [wangsen2020/agentrelay-releases](https://github.com/wangsen2020/agentrelay-releases)。启动约 30 秒后检查，之后每 12 小时检查；也可在 **配置 → 软件更新**手动检查。

选择更新后下载对应平台 ZIP，验证 Ed25519 签名、文件名、大小与 SHA-256，再暂停新业务写入并等待当前任务完成。独立助手备份原程序、数据和升级历史后替换；新程序在健康检查确认前不接收业务写入，失败时恢复旧程序和数据。等待阶段可取消，查询原任务仍可用；网络或校验失败保留当前程序。

设置页展示状态和最近完成/回退结果。更新备份保存在数据目录 `updates`；AI 客户端自身的数据不属于 AgentRelay 备份。不要让新旧版本同时修改同一个业务目录。

更多配置与案例见 [GoodExts 官网](https://goodexts.com/)、[产品详情](https://goodexts.com/agentrelay)及 [XFollower 教程](https://goodexts.com/blog/xfollower-agentrelay)。
