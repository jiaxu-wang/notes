# OpenClaw 首次配置指南

无论哪种部署方式，安装完成后都需要进行首次配置。这里覆盖最关键的几个配置项。

## Gateway 认证设置

**注意**：
v2026.3.7 Breaking Change: Gateway 认证现在要求显式设置 `gateway.auth.mode`。不设置将导致 Gateway 无法启动。这是为了修复此前暴露在互联网上的 30,000+ 未认证实例的安全隐患。

在 `~/openclaw/workspace` 目录下的配置文件中设置认证模式：

```yaml
# 选择一种认证模式
gateway:
  auth:
    mode: token  # 方式一：Token 认证（推荐用于 API 集成）
    # 或
    mode: password  # 方式二：密码认证（推荐用于 Web UI 访问）
```

## 模型选择与 API Key 配置

OpenClaw 支持多模型切换，你需要至少配置一个模型的 API Key。常见的选择：

| 模型来源 | 获取方式 | 说明 |
|---------|---------|------|
| 阿里云百炼 | 百炼平台申请 | 国内首选，qwen3.5-plus 模型 |
| 腾讯云 Coding Plan | 腾讯云购买 | 多模型套餐，首月 7.9 元 |
| 火山方舟 | 方舟平台申请 | 豆包系列模型 |
| Anthropic API | console.anthropic.com | Claude 系列模型，按量付费 |
| OpenAI API | platform.openai.com | GPT 系列模型，按量付费 |
| Ollama（本地） | 本地安装 Ollama | 免费，需要足够的本地算力 |

**核心建议**：
如果使用的是国内云厂商的一键部署方案，模型和 API Key 通常在购买时已自动配置。只有本地安装和 Docker 部署才需要手动配置。

## 版本更新

OpenClaw 几乎每天都有新版本发布。使用以下命令更新：

```bash
# 更新到最新稳定版（推荐）
openclaw update --channel stable

# 更新到 Beta 版（尝鲜）
openclaw update --channel beta

# 更新到开发版（最新功能，可能不稳定）
openclaw update --channel dev
```

三个更新渠道的区别：

| 渠道 | 更新频率 | 稳定性 | 适合人群 |
|------|---------|--------|----------|
| stable | 每周数次 | 高 | 大多数用户 |
| beta | 几乎每天 | 中 | 想尝鲜新功能的用户 |
| dev | 持续 | 低 | 开发者、贡献者 |

## 诊断检查

安装完成后，运行诊断命令检查环境是否正常：

```bash
openclaw doctor
```

这个命令会检查：
- Node.js 版本是否满足要求 (>= 22)
- 必要的系统依赖是否已安装
- Gateway 连接是否正常
- 已配置的模型 API Key 是否有效
- 守护进程状态
- 网络连通性