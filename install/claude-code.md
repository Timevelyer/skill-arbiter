# Claude Code 安装

核对来源：[Claude Code Skills 官方文档](https://code.claude.com/docs/en/skills)，核对日期 2026-11-23。

- **项目级目标**：`<repo>/.claude/skills/skill-arbiter/SKILL.md`
- **用户级目标**：`~/.claude/skills/skill-arbiter/SKILL.md`

在包含 `skill-arbiter/` 的父目录执行一行复制命令：

```bash
mkdir -p ~/.claude/skills && cp -R ./skill-arbiter ~/.claude/skills/skill-arbiter
```

项目级安装改用：

```bash
mkdir -p .claude/skills && cp -R ./skill-arbiter .claude/skills/skill-arbiter
```

## Frontmatter 注意事项

- 文件首行必须是 `---`，并保留 `name`、`description`。
- 本项目 frontmatter 只使用 `name`、`description`、`category`、`license`、`allowed-tools`；`agent_created` 已移除（D23），Claude Code 不依赖它触发。
- `allowed-tools: Read, Write, Edit, Bash, Glob, Grep` 是工具范围声明；若你的 Claude Code 版本要求 YAML 列表形式，请改为列表后再安装。
- 不添加 `hooks` 或动态上下文语法，以维持跨 Agent 一致性。

## 验证命令

```bash
test -f ~/.claude/skills/skill-arbiter/SKILL.md && printf '%s\n' 'skill-arbiter file found'
```

随后在 Claude Code 会话用 `/skills` 检查条目；未出现时按当前版本文档使用 `/reload-skills`，再输入 `/skill-arbiter` 并粘贴两份技能文本。`claude doctor` 只能作为通用环境诊断，不能替代技能发现验证。

## 预期输出

文件命令输出 `skill-arbiter file found`；`/skills` 中应出现该条目，且 `/skill-arbiter` 的实际调用输出包含八层分析、证据、处置和 `arbiter-summary`。具体菜单显示随版本可能不同。

## 版本差异与路径确认

个人与项目路径来自当前官方文档。企业托管配置、插件分发和旧版本可能使用不同入口；若未出现，运行 `/doctor` 并在当前版本 Skills 文档中核对搜索路径，不要假定所有版本一致。

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
  open_questions: ["实际 Claude Code 加载与探针需在本机验证"]
  appeal_path: "安装文档无强不一致判定；路径差异以当前官方文档为准"
```

<!-- FILE END: install/claude-code.md -->
<!-- PROGRESS: 12/21 -->
