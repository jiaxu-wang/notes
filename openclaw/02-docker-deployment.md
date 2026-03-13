# OpenClaw Docker 部署指南

## 适用场景
- 需要环境隔离的场景
- 方便迁移的场景
- 在服务器上长期运行的场景

## docker-compose 快速启动
OpenClaw 仓库内置了 docker-compose.yml，一条命令即可启动：

```bash
# 克隆仓库
git clone https://github.com/openclaw/openclaw.git
cd openclaw

# 启动
docker-compose up -d
```

## 镜像变体
| 变体 | 说明 | 适用场景 |
|------|------|----------|
| 标准镜像 | 完整功能，包含所有扩展依赖 | 一般使用，功能全 |
| slim 变体 | 多阶段构建，体积更小 | 资源受限环境，CI/CD |
| sandbox | 沙箱环境（Dockerfile.sandbox） | 安全隔离，代码执行 |
| sandbox-browser | 含浏览器的沙箱 | 需要浏览器自动化 |

**使用 slim 变体**：在 docker-compose.yml 中设置环境变量 `OPENCLAW_VARIANT=slim`。v2026.3.7 起支持扩展依赖预烘焙，容器镜像可预装扩展依赖，减少启动时的安装等待。

## 挂载目录
Docker 部署需要挂载两个关键目录，确保数据持久化：

```yaml
volumes:
  - ~/.openclaw:/root/.openclaw  # 配置和状态数据
  - ~/openclaw/workspace:/workspace  # 工作空间（YAML配置文件）
```

**重要**：不挂载这两个目录，容器重启后所有配置和对话记录都会丢失。`~/.openclaw` 存放运行状态，`workspace` 存放 YAML 配置文件。

## 端口映射
OpenClaw Gateway 默认监听 18789 端口（WebSocket），Web UI 默认使用 3000 端口。在 docker-compose.yml 中配置端口映射：

```yaml
ports:
  - "18789:18789"  # Gateway WebSocket
  - "3000:3000"  # Web UI
```