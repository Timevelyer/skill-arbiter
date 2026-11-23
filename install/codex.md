# Codex 安装

核对来源：[OpenAI Codex Skills 官方文档](https://developers.openai.com/codex/skills)，核对日期 2026-11-23。

- **项目级目标**：`<repo>/.agents/skills/skill-arbiter/SKILL.md`
- **用户级目标**：`~/.agents/skills/skill-arbiter/SKILL.md`
- Codex 会从当前目录向仓库根扫描 `.agents/skills`；同名技能不会自动合并。

在包含 `skill-arbiter/` 的父目录执行一行复制命令：

```bash
mkdir -p ~/.agents/skills && cp -R ./skill-arbiter ~/.agents/skills/skill-arbiter
```

项目级安装改用：

```bash
mkdir -p .agents/skills && cp -R ./skill-arbiter .agents/skills/skill-arbiter
```

## Frontmatter 注意事项

- `name` 与 `description` 必需，描述应前置触发任务和排除场景。
- frontmatter 中不存在 `agent_created`（D23 已移除）；Codex 只依赖 `name` 与 `description`，其余字段按未知字段忽略。
- 未添加 `agents/openai.yaml`，因此不声明 Codex 专有 UI、工具依赖或隐式调用策略。

## 验证命令

```bash
test -f ~/.agents/skills/skill-arbiter/SKILL.md && printf '%s\n' 'skill-arbiter file found'
```

启动 Codex 后运行 `/skills` 检查条目，或在提示中用 `$skill-arbiter` 显式调用。Codex 文档说明通常能自动检测变更；未出现时重启 Codex。

## 预期输出

文件命令输出 `skill-arbiter file found`；技能选择器中出现 `skill-arbiter`。有效分析输出包含解析摘要、pair 与组合检查、处置和 `arbiter-summary`，而不是只复述安装说明。

## 版本差异与路径确认

`.agents/skills` 路径来自当前官方文档；管理员路径、插件目录和旧版本可能不同。若 `/skills` 无条目，核对启动目录、仓库根、同名遮蔽和当前版本文档，不要改用未经核实的固定目录。

## 项目文件摘要

```yaml
arbiter-summary:
  version: "9.0.0"
  mode: "full"
  skills_in: 0
  groups_out: 0
  verdict: "human-review"
  rules: {total: 0, kept: 0, deduped: 0, conditional: 0, deferred: 0, excluded: 0}
  conflicts: {exclusive: 0, shared: 0, entangled: 0, contradictory: 0, dependency: 0, intent: 0}
  severity: {S0: 0, S1: 0, S2: 0, S3: 0}
  degraded_level: "none"
  actionability: "none"
  confidence: {certain: 0, high: 0, medium: 0, low: 0}
  open_questions: ["实际 Codex 加载与探针需在本机验证"]
  appeal_path: "安装文档无强不一致判定；路径差异以当前官方文档为准"
```

<!-- FILE END: install/codex.md -->
<!-- PROGRESS: 13/21 -->
