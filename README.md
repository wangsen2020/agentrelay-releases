# AgentRelay

A local AI service for browser extensions, websites and desktop workflows. Connect compatible Codex, Grok Bot and Antigravity clients through one HTTP interface, choose an existing conversation, and keep using its context across tasks.

[GoodExts](https://goodexts.com/) · [Product details](https://goodexts.com/agentrelay) · [XFollower integration tutorial](https://goodexts.com/blog/xfollower-agentrelay) · [中文说明](README.zh-CN.md)

![AgentRelay connection settings](https://goodexts.com/images/agentrelay/en/home.png)

This repository contains product documentation and binary releases. Application source is maintained privately. Third-party components retain their respective licenses.

## 2.3.6 fix

Improves Codex health checks and conversation discovery by waiting for helper processes to exit and retrying a stopped read-only connection once. Message sending is never automatically repeated.

## 2.3.5 fix

Fixes Codex result recovery when conversation history contains Unicode line or paragraph separators. JSON-RPC uses LF framing with UTF-8 chunk handling and CRLF compatibility; existing job IDs and exact message receipts are reused without another generation. Invalid protocol data now reports an error instead of waiting for a timeout.

## Download 2.3.6

| Platform | Installer | Portable archive |
| --- | --- | --- |
| Windows 10/11 x64 | [Windows setup](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.3.6/AgentRelay-2.3.6-win-x64.exe) | [Windows ZIP](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.3.6/AgentRelay-2.3.6-win-x64.zip) |
| macOS Apple Silicon | [macOS DMG](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.3.6/AgentRelay-2.3.6-mac-arm64.dmg) | [macOS ZIP](https://github.com/wangsen2020/agentrelay-releases/releases/download/v2.3.6/AgentRelay-2.3.6-mac-arm64.zip) |

[All releases and checksums](https://github.com/wangsen2020/agentrelay-releases/releases)

Keep the entire Windows portable folder or macOS app bundle. Windows includes the Python runtime needed by the Antigravity adapter; macOS currently uses an existing Python 3 installation. Install and sign in to the AI clients separately.

The application is not OS code-signed or Apple-notarized. Windows or macOS may show an unidentified-publisher prompt. Mac ARM64 acceptance runs in GitHub CI with controlled AI fixtures; it does not replace testing with your Mac and accounts.

## Start using it

1. Install or extract AgentRelay, then run it. Startup checks and data migration happen before the main window opens.
2. Start the AI client you want to use, or use the startup screen to launch it. You can skip the checks or wait 10 seconds to continue.
3. On the home page, copy the local service address and access token into your compatible tool. The default address is `http://127.0.0.1:8787`.
4. Choose an engine and conversation in the Test page to verify the connection.

The desktop opens without an account or token form. Closing its main window hides the UI while the service and tasks keep running. Click the tray icon to restore the same window. Use an explicit **Quit** action to finish active tasks and stop the application.

## One conversation, multiple tasks

Open **Sessions** in the sidebar to search titles, original IDs and workspaces. Filter by AI client and any date range, select several conversations, then use the original client's supported actions.

| Client | Conversation management |
| --- | --- |
| Codex | Archive, restore and delete |
| Antigravity IDE | Delete the conversation and its summary |
| Grok Bot | Browse existing Bots and copy available IDs |

The confirmation lists deletion targets. Codex deletion can also remove child conversations. Archived conversations have a separate view. Refreshing or restarting AgentRelay preserves the source client's state.

![AgentRelay conversation management with demonstration data](https://goodexts.com/images/agentrelay/en/sessions.png)

The session window opens wider than the main window, within the available screen space. Theme and language follow the main window while filters and selections are retained.

| Action | Grok Bot | Codex | Antigravity |
| --- | --- | --- | --- |
| Check health | `GET /grokbot/health` | `GET /codex/health` | `GET /antigravity/health` |
| List conversations | `GET /grokbot/sessions` | `GET /codex/sessions` | `GET /antigravity/sessions` |
| Create a conversation | Existing bots only | `POST /codex/sessions` | `POST /antigravity/sessions` |
| Send a task | `POST /grokbot/jobs` | `POST /codex/jobs` | `POST /antigravity/jobs` |
| Read a task | `GET /grokbot/jobs/{jobId}` | `GET /codex/jobs/{jobId}` | `GET /antigravity/jobs/{jobId}` |

Business requests require `Authorization: Bearer <token>`. JSON bodies use `Content-Type: application/json`.

```json
{"jobId":"your-unique-task-id","sessionId":"selected-conversation-id","text":"Your request","timeoutMs":120000}
```

Poll the original job ID. When its status is `completed`, read `result.text`. Reusing an ID with the same payload returns the original task; a different payload returns 409. Conversations execute serially. An `unknown` delivery state is not permission to resend with a new ID.

Existing Grok Bot routes, including `/ask`, remain compatible. Codex and Antigravity reuse the selected conversation across messages. ACP routes are optional experimental integrations configured separately from the default native-client routes.

For a practical browser-extension workflow, see the [XFollower and AgentRelay tutorial](https://goodexts.com/blog/xfollower-agentrelay). Discover the broader tool collection on the [GoodExts home page](https://goodexts.com/).

## Updates and data

Windows data lives in `%APPDATA%\AgentRelay`; macOS uses `~/Library/Application Support/AgentRelay`. Installation and upgrades preserve configuration, tokens, tasks and session mappings. Legacy data paths are migrated before opening the main window.

Version 1.x uses a different single-file updater. Install or extract 2.x manually once; subsequent 2.x updates use this release repository. The old 1.x release assets remain unchanged.

Check for updates in **Config → Software updates**. AgentRelay verifies the signed manifest and platform archive checksum, waits for active tasks, backs up the app and business data, and then replaces the program. A failed startup restores the previous app, data and upgrade history. New writes pause during handoff; task queries remain available.

If an older 2.0.4/2.0.5 Windows build reports that the update helper stopped, install 2.0.6 over the existing directory once. Keep the same data directory; configuration and task records are retained. Version 2.0.6 fixes that helper startup problem for subsequent updates.

## What changed in 2.3.5

- Native conversation management with source, title, workspace and date-range filters; batch archive, restore and delete depend on the source client's capabilities.
- Deleted Antigravity summaries and stale local Codex aliases are cleared. Archived conversations stay out of active conversation pickers after refresh and restart.
- SQLite task storage keeps requests, receipts, results and checkpoints together. Older task JSON is imported before the main window opens.
- Recover completed results using the original job and upstream message IDs. Ambiguous delivery remains visible for review.
- GoodExts recommendations in the Plugins page, improved client startup detection and synchronized child-window appearance.

Earlier desktop improvements remain available.

- Desktop updates use the system proxy/PAC and allow up to ten minutes for large downloads, while checking every redirect, manifest signature, archive size and SHA-256.
- The Windows update helper runs independently of the closing desktop process, with startup acknowledgement and rollback protection.
- Electron desktop for Windows x64 and macOS ARM64, with bundled Chromium.
- Reliable close-to-tray behavior, preserved drafts, and background task continuity.
- Startup client checks, migration loading screen, instant theme/language controls, and consistent window layouts.
- Signed platform updates with task draining, data backup and rollback; UTF-8 Windows update paths.
- Default 1 MiB JSON bodies and 120000-character messages, preserving custom limits.
- Bounded same-ID recovery for Grok Bot failures confirmed to occur before sending.

Optional Antigravity completion notifications to a Codex desktop chat currently require Windows. On macOS, use ordinary task polling. Internal client adapters can require updates when their upstream applications change.
