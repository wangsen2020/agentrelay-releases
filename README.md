# AgentRelay

**2.11.0 · Windows 10/11 x64 · 仅 EXE 安装包**

[下载 Windows 安装包](https://github.com/wangsen2020/agentrelay-releases/releases/tag/v2.11.0) · [GoodExts 产品页](https://goodexts.com/agentrelay) · [发行记录](https://github.com/wangsen2020/agentrelay-releases/releases)

AgentRelay 把本机 Codex、Grok Bot、Antigravity、Grok CLI 和 DeepSeek Harness 接入统一的本地服务。调用方选择引擎及已有会话发送任务；Codex、Antigravity 与 Harness 还可在活动轮次中直接接收其他 AI 的进度、纠正与完成回执。

应用源码在私有仓库维护；[公开发行仓库](https://github.com/wangsen2020/agentrelay-releases)提供产品说明与 Windows 安装包，第三方组件保留各自许可。AgentRelay 不销售模型额度；所接入的 AI 客户端仍需各自安装、登录并遵守其权限与服务条款。

## 2.11.0 更新

- **三方原生直达**：统一 `POST /steer` 为 Codex、Antigravity 和 DeepSeek Harness 路由原生活动轮次消息。无需等正在运行的普通任务结束，再把完成回执排到下一轮。
- **主动回传与事件驱动协作**：主控在任务中提供自己的当前会话地址；子控自主执行，保存结果后主动回传，主控收到事件继续验收、集成或派发后续任务。任务范围和验收职责由调用方 skill 约定，不在服务中实现业务调度策略。
- **空闲与活动分开处理**：普通 `/{provider}/jobs` 用于派单与空闲会话唤醒；严格 Steer 在没有活动轮次时返回 `409 no_active_turn`。未知投递不自动重发，也不自动新建会话。
- **对话看板与历史改进**：Grok CLI 与 Antigravity CLI 看板支持会话历史加载、流式回复与思考显示，完善会话命名、复制、刷新及历史持久化。
- **Windows 安装包收敛**：仅发行 Windows x64 NSIS EXE，不构建或发布其他平台与便携 ZIP。

2026-11-12 在 Windows 宿主、AgentRelay 2.11.0 上完成 Codex、Antigravity、Harness 三方六向直达验证。双方以随机 nonce 交叉确认接收，`A_DONE` 与 `H_DONE` 实际返回同一个 Codex 活动轮次。服务受理、对端实际收到、业务验收完成是不同状态；六向通信通过不代表无人值守、崩溃恢复或无限续跑均已验证。

## 安装与升级

1. 从 [2.11.0 正式发行页](https://github.com/wangsen2020/agentrelay-releases/releases/tag/v2.11.0)下载 `AgentRelay-2.11.0-win-x64.exe`，可按随附 SHA-256 校验文件核对完整性。
2. 升级前等待现有任务结束，从托盘或应用中的“退出软件”正常退出，再运行 EXE 覆盖安装。不要只关闭窗口后强行终止后台任务。
3. 默认安装位置为 `%LOCALAPPDATA%/Programs/AgentRelay`；配置、令牌、任务与映射保存在 `%APPDATA%/AgentRelay`。升级与卸载不会主动清除该业务目录，仍建议保留自己的备份。
4. 启动应用后进入本地面板。右上角关闭仅隐藏窗口；明确退出才停止服务。所选 AI 客户端需要保持可用。

**本版使用手动 EXE 升级。** 旧桌面自动更新器依赖 ZIP 与签名清单，不能把本次 EXE-only 发行视为已经适配该更新通道；本版不发布旧 ZIP 清单。系统可能对未配置代码签名的安装器提示发行者信息，请核对下载来源和 SHA-256。

## 三方直达与主动回执

```text
主控派单 → 子控自主执行 → 持久保存结果 → 主动回传 DONE/BLOCKED
                                                ↓
                           主控验收 → 集成或派发下一项 → 目标完成
```

这种机制把执行和反馈分散到各个 AI 会话。AgentRelay 仍是本地公共通信服务，主控保留目标、依赖与最终验收职责；会话上下文不会自动共享，消息需明确携带必要信息。

- 每次派单使用当前主控的引擎和会话 ID，不能复用示例或历史测试 ID。
- 要求子控完成或阻塞后主动回传结论、产物、实际验证、未决问题及下一步；避免只在子控窗口输出结果。
- 当前主控活动时用严格 Steer。主控已空闲时，只有约定允许回执唤醒，才向同一已有会话排队一次结果通知；受理不等于主控已经开始处理。
- 超时、断线或 owner 不可用时保留原操作与未知状态，先查原凭据，不换 ID 盲发，不让 ACK 相互触发 ACK。
- 服务与客户端需持续可用；空闲 Codex 主控排队回执后的自动执行、崩溃恢复和长期无人值守没有纳入本次六向实测。

## API

默认服务地址为 `http://127.0.0.1:8787`，面板路径 `/ui`。调用业务 API 使用本地面板提供的 `Authorization: Bearer <token>`，JSON 请求使用 `Content-Type: application/json; charset=utf-8`。不要公开令牌。

| 操作 | 接口 |
| --- | --- |
| 检查服务 | `GET /health` |
| 列出会话 | `GET /{provider}/sessions` |
| 提交普通任务 | `POST /{provider}/jobs` |
| 查询原任务 | `GET /{provider}/jobs/{jobId}` |
| 活动轮次直达 | `POST /steer` |
| 引擎专用直达 | `POST /codex/steer`、`POST /antigravity/steer`、`POST /dsh/steer` |
| 显式原生压缩 | `POST /codex/compactions`、`POST /antigravity/compactions` |

普通任务 provider 包括 `codex`、`grokbot`、`antigravity`、`grok-cli`、`dsh`。ACP 接入根据本机配置和运行时能力单独处理，不替代默认的 IDE/桌面原生路由。

普通派单示例：

```json
{"jobId":"调用方预先保存的唯一任务编号","sessionId":"会话列表中的真实 ID","text":"任务与回执契约","timeoutMs":300000}
```

三方直达示例：

```json
{"provider":"antigravity","sessionId":"已有活动会话的真实 ID","text":"来自真实子控：任务已完成，结果与证据……","strict":true}
```

`POST /steer` 可显式指定 `codex`、`antigravity`、`dsh`，或使用 `auto` 检索所属引擎。Codex 回执包含真实 `turnId`，Harness 回执包含 `requestId`；Antigravity 的受理回执不保证提供原生 requestId，调用方可另记本地操作编号，不能把两者混为一谈。

`200 steered` 表示原生投递受理；对端回显唯一标记才证明实际接收。`409 no_active_turn` 表示没有活动轮次；404/503 与超时应保留其真实错误或未知状态。普通 jobs 同编号、同内容返回原任务，冲突返回 409；同会话普通任务串行。Steer 结果未知时不能据普通 jobs 的幂等性推断安全重发。

## 持久结果与职责边界

- Node 24 内置 SQLite 保存任务、回执、结果与状态，WAL、FULL 同步和单连接写入。旧 JSON 迁移保留原文件，归档任务仍可按原 ID 查询。
- Codex 根据原 thread/turn/message 凭据恢复结果；Antigravity 核对同一原请求、原执行的真实报告。恢复不重新发送业务消息。
- SQLite 与外部 AI 发送不能组成同一事务；不完整回执保留 `unknown`，不伪造成功。
- 原生压缩完全由调用方显式请求。AgentRelay 不统计打标/评论次数、不设置业务阈值、不自动定时压缩，也不因历史未知压缩收据永久阻断普通任务。
- 账户、调用频率、AI 订阅和业务权限属于对应客户端或调用方；本地桥接不代表模型离线运行、无限额度或任意 API 使用授权。

