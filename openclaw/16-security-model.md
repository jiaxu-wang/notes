# OpenClaw 安全模型

OpenClaw的安全模型建立在「默认不信任」的基础上，但创始人自己坦言：「prompt injection没解决，有绝对风险。」

## 默认不信任

OpenClaw对所有入站消息的默认态度是：不可信。具体体现在以下几个机制：

### DM配对保护

当一个未知的用户通过任何消息渠道（WhatsApp、Telegram等）给你的OpenClaw发消息时，系统不会处理消息。取而代之的是返回一个配对码（pairing code），只有在你手动批准后，该用户的消息才会被处理。这防止了陌生人滥用你的Agent（以及你的API额度）。

### 群组沙箱模式

在群组环境中，OpenClaw默认运行在沙箱模式：
- 每个群组的会话互相隔离
- MEMORY.md（长期记忆）只在私聊的main session中加载，群组看不到
- 可以配置 `requireMention`，只有@提及才响应

### 工具访问控制

| 配置项 | 作用 |
|--------|------|
| allowList | 白名单模式。只允许列出的工具被调用，其他一律禁止。 |
| denyList | 黑名单模式。禁止列出的工具，其他允许。 |
| browser 开关 | 可完全禁用浏览器自动化能力 |
| canvas 开关 | 可禁用Canvas可视化 |
| nodes 开关 | 可禁用对本地设备节点的控制（如摄像头、录屏） |

## v2026.3.7新增：Gateway认证要求

最新版本引入了一个Breaking Change：Gateway认证现在要求显式设置 `gateway.auth.mode`。你必须明确选择 `token` 或 `password` 认证方式，不再有「无认证」的默认选项。

```json
# 在 openclaw.json 中配置
{
  "gateway": {
    "auth": {
      "mode": "token",  // 或 "password"
      "token": "your-secret-token"
    }
  }
}
```

### 注意

如果你从旧版本升级到v2026.3.7且没有配置认证，Gateway将拒绝启动。这是一个有意为之的设计，强制所有用户设置认证。

## Peter的坦诚

OpenClaw创始人Peter Steinberger在多个场合对安全问题保持了罕见的坦诚。他的原话：

> "This is all vibe code. Prompt injection hasn't been solved. There are absolute risks."
> （这全是vibe code。Prompt injection没有被解决。存在绝对风险。）

这种坦诚值得尊重，但也意味着：如果你要在生产环境中使用OpenClaw，安全防护必须由你自己负责。OpenClaw提供了基础的安全机制，但远谈不上「企业级安全」。