# OpenCode 安装

核对来源：[OpenCode Skills 官方文档](https://opencode.ai/v1/docs/zh-cn/skills/)，核对日期 2026-11-23。

- **项目级目标**：`.opencode/skills/skill-arbiter/SKILL.md`
- **全局目标**：`~/.config/opencode/skills/skill-arbiter/SKILL.md`
- 兼容来源：`.agents/skills/`、`.claude/skills/` 及对应全局目录。

在包含 `skill-arbiter/` 的父目录执行一行复制命令：

```bash
mkdir -p ~/.config/opencode/skills && cp -R ./skill-arbiter ~/.config/opencode/skills/skill-arbiter
```

项目级安装改用：

```bash
mkdir -p .opencode/skills && cp -R ./skill-arbiter .opencode/skills/skill-arbiter
```

## Frontmatter 注意事项

- OpenCode 要求 `name` 与目录名一致，并采用小写字母/数字/单连字符；`description` 为 1–1024 字符。
- 官方识别的核心/可选字段为 `name`、`description`、`license`、`compatibility`、`metadata`；本项目另含 `category` 与 `allowed-tools`，OpenCode 按未知字段忽略，不影响加载。
- 权限配置若为 `skill` 设置 `deny`，技能不会显示；需由用户检查 `opencode.json`。

## 验证命令

```bash
test -f ~/.config/opencode/skills/skill-arbiter/SKILL.md && printf '%s\n' 'skill-arbiter file found'
```

启动 OpenCode，检查可用技能描述中的 `skill-arbiter`，或明确要求“使用 skill-arbiter 分析以下两份 SKILL.md”。Agent 通过原生 `skill` 工具按需加载。

## 预期输出

文件命令输出 `skill-arbiter file found`；宿主可用技能列表包含该名称和描述。成功调用后应出现完整八层产物，而非仅名称回显。

## 版本差异与路径确认

上述目录与权限行为来自当前官方文档。项目发现会向 Git 工作树根遍历，嵌套/非 Git 工作区和旧版本可能不同。未出现时依次检查大写文件名、frontmatter、名称唯一性、权限与当前版本文档。

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
  open_questions: ["实际 OpenCode 加载与探针需在本机验证"]
  appeal_path: "安装文档无强不一致判定；路径差异以当前官方文档为准"
```

<!-- FILE END: install/opencode.md -->
<!-- PROGRESS: 14/21 -->
