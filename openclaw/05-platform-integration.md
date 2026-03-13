# OpenClaw 平台接入指南

## 国际平台接入

### Telegram（推荐入门 · 5分钟零门槛）
Telegram 是 OpenClaw 官方推荐的入门渠道。使用 long-polling 模式，bot 主动轮询 Telegram 服务器拉取消息，不需要公网 IP、反向代理或端口转发。本地开发、NAT 后面、防火墙内都能正常工作。

1. **找到 @BotFather**
   在 Telegram 搜索 `@BotFather`，这是 Telegram 官方的 Bot 管理工具。向它发送 `/newbot` 命令。

2. **创建 Bot**
   按提示设置 bot 的显示名称和 username（必须以 `bot` 结尾，如 `my_openclaw_bot`）。创建成功后，BotFather 会返回一个 Bot Token。

3. **配置到 OpenClaw**
   将 Token 写入 `openclaw.yml`：
   ```yaml
   channels:
     telegram:
       enabled: true
       botToken: "YOUR_BOT_TOKEN"
       dmPolicy: pairing  # 需配对码才能使用
   ```

4. **启动并配对**
   重启 Gateway。在 Telegram 中给你的 bot 发送任意消息，Gateway 会返回配对码，输入后即可开始对话。

**核心建议**：
Telegram 的 Bot API 9.5（2026年3月）新增了 `sendMessageDraft` 功能。国内用户需要代理访问 Telegram，但 bot 运行本身不受影响——只要运行 Gateway 的机器能访问 api.telegram.org 即可。

## 国内平台接入

### QQ（国内首选 · 扫码即用）
QQ 是国内用户接入 OpenClaw 最简单的方式。腾讯官方开放了 QQ Bot 能力给 OpenClaw，扫码 1 分钟即可完成绑定。支持 Markdown、图片、语音、文件等多媒体消息，手机 QQ 和桌面 QQ 均可使用。

1. **注册 QQ Bot 开发者**
   用手机 QQ 扫码完成开发者注册。未实名认证的账号需要先完成实名。单个账号最多创建 5 个 Bot。

2. **创建 QQ Bot**
   在 QQ 开放平台一键创建 Bot，获取 App ID 和 Token。

3. **配置 OpenClaw**
   在 OpenClaw 运行环境中完成配置绑定，即可在 QQ 上与 bot 对话。

**核心建议**：
QQ Bot 适合两种场景：个人助手（私聊模式）和 QQ 社群管理（群聊自动回复、批量处理、定时通知）。

### 飞书（国内企业首选 · OpenClaw 2026.2 起内置）
飞书自 OpenClaw 2026.2 起获得原生内置支持。使用 WebSocket 事件订阅，支持私聊、群聊、照片/文件/视频等多媒体消息。

1. **创建飞书应用**
   在飞书开放平台（open.feishu.cn）创建企业自建应用，获取 App ID 和 App Secret。

2. **运行向导配置**
   运行 `openclaw onboard`，选择 Feishu channel，粘贴 App ID 和 App Secret。

3. **重启 Gateway**
   重启 Gateway 后即可在飞书中与 bot 对话。

**社区替代方案**：
如果不想用内置插件，AlexAnys/feishu-openclaw 提供独立 bridge，不需要公网服务器、域名或 ngrok，5 分钟即可部署。AlexAnys/openclaw-feishu 仓库有保姆级配置指南，含 API 耗尽排查和 Lark Webhook 内网穿透方案。

### 钉钉（社区插件 · Stream 模式免公网）
钉钉通过社区插件接入 OpenClaw。消息接收使用 Stream 模式（WebSocket 长连接），不需要公网地址。支持私聊、群聊、文件附件、语音消息、钉钉文档 API、多 Agent 路由等功能。

1. **创建钉钉应用**
   在钉钉开放平台创建应用，添加机器人能力。

2. **设置 Stream 模式**
   将消息接收模式设置为 Stream 模式。这样 bot 通过 WebSocket 长连接接收消息，不需要配置公网回调地址。

3. **安装插件并配置**
   安装社区插件 `@slavv/dingtalk`，或使用 DingTalk-Real-AI 官方出品的 `dingtalk-openclaw-connector`（支持 AI Card 流式响应）。配置 `openclaw.yml` 后启动 Gateway。

**核心建议**：
钉钉尚未获得 OpenClaw 官方内置支持（2026年3月有 Feature Request 提出），但社区方案已经非常成熟。DingTalk-Real-AI 连接器由钉钉团队维护，可靠性有保障。

### 企业微信（两种模式 · 已被多家云平台验证）
企业微信有两种接入模式：Agent 模式（XML 回调经典模式）和 Bot 模式（JSON 回调，原生 stream 支持）。已被腾讯云、火山引擎、天翼云等公有云平台采纳验证。

1. **创建企业微信应用**
   在企业微信管理后台创建自建应用（Agent 模式）或配置智能机器人（Bot 模式）。

2. **安装社区插件**
   可选择：`dingxiang-me/openClaw-Wechat`（支持个人微信互通、流式输出、群聊@、白名单控制、全中文配置）或 `sunmoy/openclaw-plugin-wecom`（支持动态 Agent 管理、指令白名单）。

3. **配置并启动**
   按插件文档配置 `openclaw.yml`，启动 Gateway。要求 OpenClaw ≥ 2026.2.9，部分功能需 ≥ 2026.3.2。