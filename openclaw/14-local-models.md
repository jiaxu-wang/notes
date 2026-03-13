# OpenClaw 本地模型与推荐方案

完全免费，完全离线，完全隐私。代价是需要硬件投入，能力上限受限。

## Ollama

最流行的本地模型运行方案，完全免费，OpenClaw能自动发现已安装的模型。

```bash
# 1. 安装Ollama后拉取模型
ollama pull qwen2.5:32b
ollama pull deepseek-r1:14b

# 2. 设置环境变量（任意值即可）
OLLAMA_API_KEY=ollama-local

# 3. OpenClaw自动发现支持工具调用的本地模型
```

### 注意

不要使用 v1 OpenAI兼容URL，会导致工具调用异常。让OpenClaw使用原生Ollama API URL进行自动发现。冷启动有延迟，建议保持模型加载状态。

## LM Studio

有GUI界面的本地模型方案，使用Llama.cpp后端，原始性能更好。工具调用在流式模式下比Ollama更稳定。OpenClaw创始人Peter Steinberger个人使用LM Studio作为本地后端。

```json
{
  "models": {
    "mode": "merge",
    "providers": {
      "lmstudio": {
        "baseUrl": "http://127.0.0.1:1234/v1",
        "apiKey": "lm-studio",
        "api": "openai-responses",
        "models": [
          { "id": "model-name", "contextWindow": 32768, "maxTokens": 8192 }
        ]
      }
    }
  }
}
```

## 推荐本地模型

| 模型 | 参数量 | 推荐场景 | 最低内存 |
|------|--------|----------|----------|
| Qwen3.5-Coder:32B | 32B | 代码生成、Agent任务 | 32GB RAM |
| Devstral-24B | 24B | Agent/工具调用 | 32GB RAM |
| Qwen 2.5:32B | 32B | 通用任务 | 32GB RAM |
| DeepSeek-R1:14B | 14B | 推理任务 | 16GB RAM |
| Llama 3.3 | 8B-70B | 通用任务 | 16-64GB RAM |

### 硬件要求速查

运行7-18B参数模型最低需要16GB RAM。运行32B参数模型推荐32GB RAM。如果有NVIDIA/Apple Silicon GPU会显著加速推理。

## 五套推荐方案

### 方案一：极致省钱（月均<$5）
- 主力：DeepSeek-V3.2（$0.14/$0.28）
- 备选：Qwen 3.5 Plus（$0.40/$1.20）
- 推理：Cron：GLM-4.5-Flash（免费）
- 推理任务：DeepSeek-RL（$0.55/$2.19）
- 适合：个人开发者、学习探索。风险：DeepSeek高峰期延迟，需Fallback兜底。

### 方案二：国产性价比（月均$5-15）
- 主力：GLM-5（$0.80/$2.56）
- 备选：DeepSeek-V3.2（$0.14/$0.28）
- 推理增强：Kimi K2.5（$0.60/$3.00）
- 简单任务：GLM-4.5-Flash（免费）
- 适合：国内用户，追求中文体验和稳定性。GLM-5代码能力强，延迟低。

### 方案三：国际平衡（月均$10-30）
- 主力：Claude Sonnet 4.6（$3.00/$15.00）
- 轻量：Claude Haiku 4.5 或 Gemini Flash
- 复杂任务：Claude Opus 4.6（按需升级）
- 心跳/Cron：Gemini Flash（免费额度）
- 适合：追求Agent效果最优、预算充足。Claude在Agent/工具调用场景效果最好。

### 方案四：混合最优（月均$5-20，推荐）
- 复杂任务：Claude Sonnet 4.6
- 日常对话：DeepSeek-V3.2
- 心跳/定时：Gemini Flash 或 Haiku
- Fallback链：Sonnet → Haiku → DeepSeek-V3.2
- 大多数用户的最佳选择。兼顾效果和成本，Fallback机制自动处理限流。

```json
// 方案四的fallback配置示例
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "anthropic/claude-sonnet-4-6",
        "fallbacks": [
          "anthropic/claude-haiku-4-5",
          "deepseek/deepseek-chat"
        ]
      }
    }
  }
}
```

### 方案五：完全免费
- 选项A：本地 Ollama + Qwen3.5-Coder:32B 或 Devstral-24B（需32GB RAM）
- 选项B：免费API组合 ─ GLM-4.5-Flash + ERNIE Speed + Gemini Flash
- 适合：隐私敏感、纯实验用途。本地方案需要较好的硬件。