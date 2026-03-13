# OpenClaw ClawHub 技能市场

ClawHub 是 OpenClaw 的官方 Skill 注册表，类似于 npm 之于 Node.js。

## 市场概况

| 指标 | 数据 |
|------|------|
| 总注册技能 | 13,729 |
| 精选技能（awesome列表筛选） | 5,494 |
| 被过滤技能（垃圾/重复/恶意） | 6,940 |
| 被标记为恶意的 | 800+（约20%在高峰期） |

**注意**：
ClawHub 的质量问题非常严重。社区项目 awesome-openclaw-skills (31.4K Stars) 从 13,729 个技能中精选了 5,494 个，剩下的大部分是垃圾、重复或低质量内容。安装任何第三方 Skill 前，务必查看源码。

## 安装与搜索

```bash
# 安装 Skill
openclaw skills install <skill-name>

# 搜索 Skill
openclaw skills search "browser automation"

# 列出已安装的 Skills
openclaw skills list

# 卸载 Skill
openclaw skills uninstall <skill-name>
```

ClawHub 支持向量搜索，也就是说你可以用自然语言描述需求来搜索 Skill，不必精确匹配名称。

## 技能分类 Top 10

| 排名 | 分类 | 数量 | 说明 |
|------|------|------|------|
| 1 | 编码Agent与IDE | 1,222 | 代码生成、调试、重构等开发辅助 |
| 2 | Web与前端开发 | 938 | HTML/CSS/JS生成、组件开发 |
| 3 | DevOps与云 | 408 | Docker、K8s、CI/CD管理 |
| 4 | 搜索与研究 | 350 | 联网搜索、信息汇总 |
| 5 | 浏览器与自动化 | 335 | 网页操作、表单填写、截图 |
| 6 | 生产力与任务 | 206 | 日程、待办、项目管理 |
| 7 | AI与LLM | 197 | 提示工程、模型切换、多Agent协作 |
| 8 | CLI工具 | 186 | 终端命令增强、系统管理 |
| 9 | Git与GitHub | 170 | 仓库管理、PR审查、Issue处理 |
| 10 | 图片与视频生成 | 169 | AI绘画、视频处理 |

编码相关的技能占了绝大多数（前两名合计2,160个），反映出 OpenClaw 用户中开发者占比极高。但也意味着这两个分类里重复和低质量 Skill 最多。