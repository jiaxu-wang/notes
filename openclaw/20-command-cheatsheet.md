# OpenClaw 命令速查表

## 安装与更新

| 命令 | 说明 |
|------|------|
| `npm install -g openclaw@latest` | 全局安装OpenClaw |
| `openclaw onboard --install-daemon` | 初始配置 + 安装守护进程 |
| `openclaw update --channel stable` | 更新到最新稳定版 |
| `openclaw update --channel beta` | 更新到Beta版（尝鲜） |
| `openclaw doctor` | 诊断检查，排查常见问题 |
| `openclaw --version` | 查看当前版本 |

## 日常使用

| 命令 | 说明 |
|------|------|
| `openclaw gateway --port 18789 --verbose` | 启动Gateway（详细日志模式） |
| `openclaw gateway restart` | 重启Gateway（改配置后必须执行） |
| `openclaw agent --message "xxx"` | 直接发送消息给Agent |
| `openclaw devices pair` | 设备配对（新设备首次连接） |
| `openclaw models list` | 列出已配置的模型 |
| `openclaw models status --probe` | 测试模型连通性 |
| `openclaw config set agents.defaults.model.primary provider/model` | 设置主力模型 |

## 插件管理

| 命令 | 说明 |
|------|------|
| `openclaw plugins install <name>` | 安装插件/Skill |
| `openclaw plugins enable <name>` | 启用插件 |
| `openclaw plugins list` | 列出已安装插件 |
| `openclaw plugins install @openclaw-china/channels` | 安装中国IM插件 |
| `openclaw china setup` | 配置中国IM平台（需先安装插件） |

## 模型认证

| 命令 | 说明 |
|------|------|
| `openclaw onboard --auth-choice zai-api-key` | 配置智谱GLM |
| `openclaw onboard --auth-choice apikey --token-provider openrouter --token "$KEY"` | 配置OpenRouter |
| `openclaw models auth login --provider qwen-portal --set-default` | 通义千问OAuth登录 |

## 聊天命令（在对话中使用）

| 命令 | 说明 |
|------|------|
| `/status` | 会话概览（当前模型、Token用量） |
| `/new` | 清空会话历史，开始新对话 |
| `/think <level>` | 调整推理深度（off/minimal/low/medium/high/highhigh） |
| `/usage off|tokens|full` | 控制回复页脚的用量显示 |
| `/activation mention|always` | 群消息处理模式 |

## Docker部署

| 命令 | 说明 |
|------|------|
| `docker-compose up -d` | 后台启动OpenClaw容器 |
| `docker-compose logs -f` | 查看实时日志 |
| `docker-compose pull && docker-compose up -d` | 更新到最新镜像 |