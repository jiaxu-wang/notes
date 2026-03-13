# OpenClaw 国产模型配置

国产模型是OpenClaw用户省钱的核心武器。DeepSeek-V3.2.2的输入价格仅为Claude Sonnet的1/20。

## DeepSeek

性价比之王。DeepSeek-V3.2.2是当前稳定版（2025年12月发布），输入价格仅$0.14/M tokens。2026年3月初DeepSeek V4已发布（万亿参数级，支持100万Token上下文），旗舰版正在逐步放量。

| 模型 | 输入 / 1M | 输出 / 1M | 定位 |
|------|-----------|-----------|------|
| DeepSeek-V4（最新） | 待定 | 待定 | 万亿参数旗舰，100万上下文 |
| DeepSeek-V3.2.2（deepseek-chat） | $0.14 | $0.28 | 当前稳定版，极致性价比 |
| DeepSeek-RL（deepseek-reasoner） | $0.55-0.70 | $2.19-2.50 | 深度推理 |

### 配置方式（自定义Provider）

```json
{
  "env": { "DEEPSEEK_API_KEY": "sk-xxx" },
  "models": {
    "mode": "merge",
    "providers": {
      "deepseek": {
        "baseUrl": "https://api.deepseek.com/v1",
        "apiKey": "${DEEPSEEK_API_KEY}",
        "api": "openai-completions",
        "models": [
          { "id": "deepseek-chat", "contextWindow": 128000, "maxTokens": 8192 },
          { "id": "deepseek-reasoner", "contextWindow": 128000, "maxTokens": 8192 }
        ]
      }
    }
  }
}
```

### 注意

DeepSeek高峰期偶有延迟甚至不可用，不建议作为唯一Provider。务必搭配Fallback模型兜底。

## 智谱GLM

国产模型中代码能力最强的选择。GLM-5在SWE-bench上拿到了开源模型最高分，价格仅$0.80/M输入。更妙的是，OpenClaw内置了zai Provider，配置极为简单。

| 模型 | 输入 / 1M | 输出 / 1M | 定位 |
|------|-----------|-----------|------|
| GLM-5 | $0.80 | $2.56 | 最新旗舰，代码能力强 |
| GLM-4.5 | $0.60 | $2.20 | 上一代主力 |
| GLM-4.7-Flash | 免费 | 免费 | 轻量免费 |
| GLM-4.5-Flash | 免费 | 免费 | 轻量免费 |

### 配置方式（内置支持）

```bash
# CLI快速配置
openclaw onboard --auth-choice zai-api-key

# 或手动配置 openclaw.json
{
  "env": { "ZAI_API_KEY": "sk-xxx" },
  "agents": {
    "defaults": {
      "model": { "primary": "zai/glm-5" }
    }
  }
}
```

### 注意

`z.ai/*` 和 `z-ai/*` 前缀会自动转换为 `zai/*`。

### 核心建议

GLM Flash系列完全免费，非常适合用于心跳任务和简单对话。把免费模型用在低价值任务上，把预算留给真正需要强模型的场景。

## 通义千问 Qwen

Qwen 3.5是阿里2026年2月发布的最新版本（397B总参数/17B激活，MoE架构，已开源）。代码专用的Qwen 3.5-Coder性价比极高。

| 模型 | 输入 / 1M | 输出 / 1M | 定位 |
|------|-----------|-----------|------|
| Qwen 3.5 Max | $1.20 | $6.00 | 旗舰模型（397B-A17B） |
| Qwen 3.5 Plus | $0.40 | $1.20 | 主力平衡 |
| Qwen 3.5 Coder | $0.22 | $1.00 | 代码专用，性价比极高 |
| Qwen 3.5 8B | $0.05 | $0.40 | 轻量低成本 |

### 配置方式（插件 + OAuth）

```bash
# 通过插件接入，OAuth设备码认证（无需API Key）
openclaw plugins enable qwen-portal-auth
openclaw gateway restart
openclaw models auth login --provider qwen-portal --set-default
```

### 模型ID

`qwen-portal/coder-model`、`qwen-portal/vision-model`。每日2,000次免费请求。

## 豆包 Doubao

| 模型 | 输入 / 1M | 输出 / 1M | 定位 |
|------|-----------|-----------|------|
| Seed 2.0 Pro | $0.47 | $2.37 | 旗舰推理，对标GPT-5.2 |
| Doubao 1.5 Pro-32k | $0.11 | — | 通用对话，极致低价 |
| Doubao 1.5 Lite-32k | $0.042 | — | 最便宜的选择之一 |

### 配置方式（自定义Provider）

```json
{
  "env": { "DOUBAO_API_KEY": "xxx" },
  "models": {
    "mode": "merge",
    "providers": {
      "doubao": {
        "baseUrl": "https://ark-cn-beijing.volces.com/api/v3",
        "apiKey": "${DOUBAO_API_KEY}",
        "api": "openai-completions",
        "models": [
          { "id": "doubao-seed-2-0-pro", "contextWindow": 128000, "maxTokens": 4096 }
        ]
      }
    }
  }
}
```

## Kimi（月之暗面）

| 模型 | 输入 / 1M | 输出 / 1M | 定位 |
|------|-----------|-----------|------|
| Kimi K2.5 | $0.60 | $3.00 | 最新旗舰 |
| Kimi K2 0905 | $0.39 | $1.90 | 性价比版 |

### 配置方式

```json
{
  "env": { "MOONSHOT_API_KEY": "sk-xxx" },
  "models": {
    "mode": "merge",
    "providers": {
      "moonshot": {
        "baseUrl": "https://api.moonshot.cn/v1",
        "apiKey": "${MOONSHOT_API_KEY}",
        "api": "openai-completions",
        "models": [
          { "id": "kimi-k2.5", "contextWindow": 256000, "maxTokens": 8192 }
        ]
      }
    }
  }
}
```

也可通过OpenRouter接入：`openrouter/moonshotai/kimi-k2.5`

## 百度文心

文心5.0于2026年1月22日发布（2.4万亿参数，原生全模态，激活参数<3%）。

| 模型 | 输入价格 | 输出价格 | 定位 |
|------|----------|----------|------|
| 文心 5.0 | ~$0.58/M | ~$1.16/M | 最新旗舰（2.4万亿参数） |
| ERNIE Speed | 免费 | 免费 | 轻量 |
| ERNIE Lite | 免费 | 免费 | 最轻量 |

### 注意

百度API格式与OpenAI不完全兼容，需要通过one-api等中转工具适配。在OpenClaw社区中存在感最低，配置复杂度最高。如果没有特别的百度云生态绑定，建议优先选择其他国产模型。