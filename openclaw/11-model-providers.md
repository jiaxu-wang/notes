# OpenClaw 模型提供商总览

OpenClaw 支持十余家模型提供商，从国际顶尖到国产平价再到完全免费的本地模型，覆盖所有预算和场景。

OpenClaw 最大的优势之一是模型自由：你不被绑定在某一家厂商上。通过 `~/.openclaw/openclaw.json` 配置文件，可以灵活切换主力模型、设置 Fallback 备选链、甚至让不同任务走不同模型。

## 支持的模型提供商一览

| 提供商 | 代表模型 | 输入价格 / 1M tokens | 输出价格 / 1M tokens | 接入方式 | 推荐场景 |
|--------|----------|---------------------|---------------------|----------|----------|
| Anthropic | Claude Sonnet 4.6 | $3.00 | $15.00 | 内置 Provider | Agent 任务效果最佳 |
| OpenAI | GPT-5.4 | $2.50 | $15.00 | 内置 Provider | 通用能力强 |
| Google | Gemini 3 Pro | $2.00 | $12.00 | 内置 Provider | 多模态、超长上下文 |
| DeepSeek | DeepSeek-V3.2.2 / V4 | $0.14 | $0.28 | 自定义 Provider | 极致低价、代码任务 |
| 智谱 GLM | GLM-5 | $0.80 | $2.56 | 内置 (zai) | 国产最强代码能力 |
| 通义千问 | Qwen 3.5 Max | $1.20 | $6.00 | 插件 (OAuth) | 中文 NLP、代码生成 |
| 豆包 | Seed 2.0 Pro | $0.47 | $2.37 | 自定义 Provider | 批量处理、低成本 |
| 百度文心 | 文心 5.0 | ~$0.58 | ~$1.16 | 自定义 (需适配) | 百度云生态用户 |
| Kimi | Kimi K2.5 | $0.60 | $3.00 | 自定义 Provider | 中文 Agent、长上下文 |
| MiniMax | MiniMax M2.5 | $0.50 | $2.00 | 自定义 Provider | SWE-bench 高分、性价比 |
| Ollama | Qwen3.5-Coder:32B | 免费 | 免费 | 自动发现 | 隐私敏感、零成本 |
| LM Studio | Devstral-24B | 免费 | 免费 | 自定义 Provider | 本地 GUI、模型测试 |

## 配置核心概念

理解三个关键概念，就能掌握 OpenClaw 的模型配置：

- **内置 Provider**: Anthropic、OpenAI、Google、智谱（zai）等无需额外配置，设置 API Key 即可使用
- **自定义 Provider**: DeepSeek、豆包、Kimi 等需要在 `models.providers` 中手动添加
- **Fallback 机制**: 主模型不可用时自动切换到备选，这是最核心的省钱策略

```json
{
  "env": { "API_KEY_NAME": "sk-xxx" },
  "agents": {
    "defaults": {
      "model": {
        "primary": "provider/model-name",  // 主力模型
        "fallbacks": ["provider/model-b"]  // 备选（主模型限流时自动切换）
      }
    }
  },
  "models": {
    "mode": "merge",  // 保留内置 provider，叠加自定义
    "providers": { /* 自定义 provider 配置 */ }
  }
}
```

**核心建议**：
设置 `models.mode: "merge"` 非常重要。它能保留所有内置 Provider 的同时叠加你的自定义配置。如果不设置，自定义配置会覆盖内置 Provider。