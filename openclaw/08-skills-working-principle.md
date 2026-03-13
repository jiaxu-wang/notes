# OpenClaw Skills 工作原理

Skills 是 OpenClaw 的能力扩展单元。理解它的加载机制，才能真正用好这个系统。

## 三层优先级

OpenClaw 的 Skills 有三个来源，按优先级从高到低排列：

| 优先级 | 位置 | 说明 |
|--------|------|------|
| 最高 | `$workspace/skills/` | 项目级 Skills，只对当前工作区生效。适合对特定项目定制的能力。 |
| 中 | `~/.openclaw/skills/` | 用户级 Skills，全局生效。通过 ClawHub 安装或手动放置的 Skills 都在这里。 |
| 最低 | bundled skills | 内置的 95+ Skills，随 OpenClaw 版本发布。不需要安装，开箱即用。 |

**核心建议**：
如果同名 Skill 存在于多个层级，高优先级会覆盖低优先级。这意味着你可以在 workspace 级别「重写」一个内置 Skill 的行为，而不影响其他项目。

## Skill 加载过程

当 OpenClaw 启动或收到消息时，Skills 的加载遵循以下流程：

1. **读取 Skill 元数据**
   扫描三层目录，读取每个 Skill 的 `SKILL.md` 文件，解析名称、描述、触发条件、所需环境变量等信息。

2. **应用环境变量**
   如果 Skill 声明了需要的 API Key 或环境变量（如 `GITHUB_TOKEN`），系统会从 `openclaw.json` 的 `env` 字段中注入。缺少必要变量的 Skill 会被静默跳过。

3. **构建 System Prompt**
   将所有可用 Skills 的描述注入到 system prompt 中，告知模型当前可以调用哪些能力。这是模型「知道自己能做什么」的关键步骤。

4. **运行后恢复**
   Skill 执行完毕后，恢复原始环境变量和上下文状态，避免 Skill 之间相互干扰。

## ClawHub 注册表

ClawHub（clawhub.com）是 OpenClaw 的官方 Skill 注册表，类似于 npm 之于 Node.js。它提供：
- 公共 Skills 的发布和版本管理
- 基于向量搜索的 Skill 发现
- 下载量统计和社区评分
- VirusTotal 合作的安全扫描（但覆盖率有限）