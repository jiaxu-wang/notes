# OpenClaw 远程访问指南

OpenClaw Gateway 默认监听本地 `ws://127.0.0.1:18789`。当你需要从外部网络访问时，有以下几种方案。

## Tailscale Serve / Funnel（推荐方案）

Tailscale 是 OpenClaw 官方推荐的远程访问方案，提供两种模式：

| 模式 | 访问范围 | 使用场景 |
|------|---------|----------|
| Serve | Tailscale 网络内的设备 | 自己的手机/平板访问家里的 OpenClaw |
| Funnel | 公网任何人 | 给 webhook 回调提供公网 URL（如飞书、Slack HTTP 模式） |

```bash
# Serve: 仅 Tailscale 网络可访问
tailscale serve --bg http+insecure://127.0.0.1:18789

# Funnel: 公网可访问（用于 webhook 回调）
tailscale funnel --bg https+insecure://127.0.0.1:18789
```

**核心建议**：
大部分 channel（Telegram long-polling、Discord、Slack Socket Mode、钉钉 Stream 模式）都是 bot 主动连接服务器，不需要公网 IP。只有需要 webhook 回调的场景（BlueBubbles、Slack HTTP 模式）才需要 Funnel 暴露公网地址。

## SSH 端口转发（最通用的方案）

如果 OpenClaw 运行在远程服务器上，用 SSH 隧道将 Gateway 端口转发到本地：

```bash
# 将远程服务器的 18789 端口转发到本地
ssh -L 18789:127.0.0.1:18789 user@your-server

# 后台运行
ssh -fNL 18789:127.0.0.1:18789 user@your-server
```

转发后，本地客户端连接 `ws://127.0.0.1:18789` 即可访问远程 Gateway。

## Dashboard Web UI

OpenClaw 内置 Web UI，启动 Gateway 后可在浏览器中访问管理界面。Web UI 支持查看会话状态、模型配置、channel 连接状况、Token 用量统计等。v2026.3.7 新增了西班牙语支持。

```bash
# Gateway 启动后默认可访问
# 浏览器打开 http://127.0.0.1:18789

openclaw gateway --port 18789 --verbose
```

**安全提醒**：v2026.3.7 起 Gateway 认证要求显式设置 `gateway.auth.mode`（token 或 password）。不要在公网暴露未认证的 Gateway。