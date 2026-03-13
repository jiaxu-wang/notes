# OpenClaw 自建 Skill 指南

一个 Skill 的最小单位就是一个目录加一个 `SKILL.md` 文件。

## 目录结构

```
my-skill/
├── SKILL.md  # 必须，Skill 的核心定义文件
├── scripts/  # 可选，辅助脚本
├── helper.py
├── templates/  # 可选，模板文件
├── report.md
└── README.md  # 可选，说明文档
```

唯一必须的文件是 `SKILL.md`，其他都是可选的。最简单的 Skill 只需要一个 `SKILL.md` 就能工作。

## SKILL.md 格式示例

```markdown
# My Custom Skill

## Description
帮助用户进行每日工作总结，生成结构化的日报。

## Trigger
当用户提到「日报」「工作总结」「今日汇报」时激活。

## Instructions
1. 询问用户今天完成了哪些工作
2. 按项目分类整理
3. 标注每项工作的状态（已完成/进行中/阻塞）
4. 生成 markdown 格式的日报
5. 保存到 `~/reports/YYYY-MM-DD.md`

## Environment Variables
- REPORTS_DIR: 日报存储目录（默认 `~/reports`）

## Tools Required
- file.write
- memory.search
```

## 安装方式

| 方式 | 位置 | 生效范围 | 命令 |
|------|------|----------|------|
| 项目级 | `<workspace>/skills/my-skill/` | 仅当前工作区 | 直接将文件夹放到 workspace 的 skills 目录下 |
| 全局 | `~/.openclaw/skills/my-skill/` | 所有会话 | 直接复制，或通过 ClawHub 安装 |

**核心建议**：
项目级 Skill 非常适合团队协作场景：把 Skill 放进 Git 仓库的 `skills/` 目录，团队成员克隆仓库后就自动获得了相同的 Agent 能力。

## 分享到 ClawHub

1. **准备 Skill**
   确保 `SKILL.md` 格式正确，包含清晰的 Description 和 Instructions。

2. **登录 ClawHub**
   ```bash
   openclaw clawhub login
   ```

3. **发布**
   ```bash
   openclaw clawhub publish ./my-skill
   ```

发布后其他用户可以通过 `openclaw skills install your-skill-name` 安装。ClawHub 会自动进行基础安全扫描，但不保证完全可靠。