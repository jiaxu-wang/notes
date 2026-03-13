# OpenClaw 本地安装指南

## 适用人群
- 开发者
- 希望完全掌控数据的用户

## 技术栈
- TypeScript 项目
- 运行在 Node.js 上

## 系统要求
| 要求 | 详情 |
|------|------|
| Node.js | >= 22（强制要求） |
| 包管理器 | npm / pnpm / bun 均可 |
| macOS | 需要 Xcode Command Line Tools |
| Linux | 标准构建工具（gcc, make） |
| Windows | 强烈推荐 WSL2 |

## 安装方式

### 方式一：npm 全局安装（推荐）
```bash
# 安装 OpenClaw
npm install -g openclaw@latest

# 初始化并安装守护进程
openclaw onboard --install-daemon
```
- `onboard` 命令会引导完成初始配置（选择模型、配置 API Key、设置消息频道等）
- `--install-daemon` 参数会安装守护进程，让 OpenClaw 在后台持续运行

### 方式二：一键脚本安装
```bash
curl -sSL https://get.openclaw.ai | bash
```
- 脚本会自动检测系统环境
- 安装 Node.js（如缺失）
- 完成 OpenClaw 安装