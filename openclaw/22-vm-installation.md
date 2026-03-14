# OpenClaw 虚拟机安装指南

## 一、系统准备

先安装好ubuntu24.04

### 1、更新系统并安装基础工具

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl git build-essential
```

### 2、安装并启动 SSH 服务 (可选)

```bash
sudo apt install -y openssh-server
sudo systemctl status ssh
sudo systemctl start ssh
```

## 二、安装 Node.js 22

OpenClaw 要求 Node.js 版本 ≥ 22。推荐使用 NodeSource 脚本安装。

### 1、下载并运行安装脚本

```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash - && sudo apt install -y nodejs
```

### 2、验证版本

```bash
node -v
npm -v
```

## 三、安装 OpenClaw

### 1、使用官方一键脚本安装，并配置环境变量。

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

### 2、验证安装

```bash
openclaw -v
```

**重要提示：使环境变量生效**

安装完成后，你需要运行`source ~/.bashrc`或者重新打开命令行窗口，才能运行 openclaw 命令

## 四、初始化配置指引

执行交互式初始化

```bash
openclaw onboard
```

## 五、设置 systemd 后台服务

```bash
sudo openclaw gateway install-service
sudo systemctl enable openclaw-gateway --now
```