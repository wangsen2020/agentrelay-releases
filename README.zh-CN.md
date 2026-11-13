# AgentRelay

[English](README.md) · [简体中文](README.zh-CN.md) · [Português (Brasil)](README.pt-BR.md) · [Español](README.es.md)

**2.13.3 · Windows 10/11 x64 · 仅 EXE 安装包**

连接本机 Codex、Grok Bot、Antigravity、Grok CLI、DeepSeek Harness 与 WorkBuddy。Codex、Antigravity、Harness、Grok CLI 及 Antigravity CLI 支持活动轮次原生直达引导；WorkBuddy 现已作为第 6 引擎接入，支持桌面端会话复用、独立官方扫码登录、`/ui/workbuddy` 独立对话窗口、三端消息底部快捷复制栏与公共 `/workbuddy/jobs` 接口。

## 下载与升级

[Windows x64 EXE](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.13.3/AgentRelay-2.13.3-win-x64.exe) · [SHA-256 校验](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.13.3/SHA256SUMS.txt) · [发行说明](https://github.com/wangsen2020/agentrelay-releases/releases/tag/v2.13.3)

等待活跃任务结束，明确退出软件后运行 EXE 覆盖安装，保留原 AppData 配置与任务记录。本版手动升级，不将旧 ZIP 自动更新通道作为此次升级路径。

## 选择技能

- [三方直达协作 agentrelay-session-bridge](agentrelay-session-bridge/SKILL.md)：AI 会话派单、活动轮次直达、主动回执与事件驱动交接。

- [插件与客户端接入 agentrelay-integration](agentrelay-integration/SKILL.md)：普通 jobs 接入与原任务恢复。

- [本机重打包 agentrelay-local-rebuild](agentrelay-local-rebuild/SKILL.md)：仅适合已获私有源码访问权限的维护者。

## 安装直达 Skill

将整个 `agentrelay-session-bridge` 文件夹复制到 `~/.codex/skills/` 或对应 AI 工具的技能目录，保留 scripts、references、agents。需要 Node.js 22+、本机 AgentRelay 2.11.0+ 和已登录可用的原生 AI 客户端。

向 AI 发出指令：“使用 $agentrelay-session-bridge，把这个有明确边界的任务交给选定的已有会话，携带当前主控会话地址，要求完成后主动回传 DONE，阻塞时回传 BLOCKED。”

主控每轮提供真实当前 provider/sessionId。活动轮次用严格 Steer；空闲会话用普通 jobs 唤醒。只有契约允许且明确 no_active_turn，才向同一主控排队一次结果通知；未知状态不自动重发，不新建替代会话。

## 已验证能力与边界

Windows 实测三方六向直达、双方随机 nonce 原样匹配及两份 DONE 返回同一 Codex 活动轮次。服务受理、对端收到和业务完成分别核实。

空闲 Codex 后续自动执行、崩溃自恢复、无限无人值守或固定费用节省比例未由本次实测证明。AI 会话不会自动共享上下文，账户、额度与权限仍遵守各产品要求。

应用源码保持私有。公开技能只有通用 API 工具与工作流说明，不含账户凭据或服务源码。

[GoodExts 产品页与接入指南](https://goodexts.com/zh/agentrelay)
