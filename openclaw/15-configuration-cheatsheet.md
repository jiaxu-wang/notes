# OpenClaw 配置要点速查

| 操作 | 命令/配置 |
|------|-----------|
| 引导式配置 | `openclaw onboard` |
| 查看已配置模型 | `openclaw models list` |
| 测试连通性 | `openclaw models status --probe` |
| 设置主力模型 | `openclaw config set agents.defaults.model.primary provider/model` |
| 添加Fallback | 编辑 openclaw.json 的 fallbacks 数组 |
| 重启网关 | `openclaw gateway restart`（改配置后必须执行） |
| 环境变量引用 | 配置中用 `${VAR_NAME}` 引用 env 中的变量 |