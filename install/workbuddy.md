# WorkBuddy 安装

- **项目级目标**：`<workspace>/.workbuddy/skills/skill-arbiter/SKILL.md`
- **用户级目标**：`~/.workbuddy/skills/skill-arbiter/SKILL.md`
- 用户级目录在 Windows 通常位于用户配置目录下；以 WorkBuddy 当前技能管理入口显示的位置为准。

在包含 `skill-arbiter/` 的父目录执行一行复制命令（Bash/Git Bash）：

```bash
mkdir -p ~/.workbuddy/skills && cp -R ./skill-arbiter ~/.workbuddy/skills/skill-arbiter
```

项目级安装改用：

```bash
mkdir -p .workbuddy/skills && cp -R ./skill-arbiter .workbuddy/skills/skill-arbiter
```

Windows PowerShell 可将源目录复制到用户范围：

```powershell
New-Item -ItemType Directory -Force "$HOME/.workbuddy/skills" | Out-Null; Copy-Item -Recurse -Force "./skill-arbiter" "$HOME/.workbuddy/skills/skill-arbiter"
```

## Frontmatter 注意事项

- 保留 `name`、`description` 和 `agent_created: true`；后者使本地创建的技能可被技能管理功能识别为维护对象。
- 其他 Agent 不必理解 `agent_created`；跨 Agent 行为只能依赖公共字段与正文。
- 不把 WorkBuddy 专用工具名写入核心规则，保证离线文本分析可移植。

## 验证命令

```bash
test -f ~/.workbuddy/skills/skill-arbiter/SKILL.md && printf '%s\n' 'skill-arbiter file found'
```

在 WorkBuddy 中刷新或重开会话，检查技能列表；随后输入 `/skill-arbiter` 或明确请求“使用 skill-arbiter 分析以下技能”。

## 预期输出

文件命令输出 `skill-arbiter file found`；技能列表可发现 `skill-arbiter`，调用后加载八层规则并引用 `reference/`、`templates/`。若仅复制 `SKILL.md` 而未复制资源目录，则无法获得完整模板。

## 版本差异与路径确认

用户级与项目级路径是 WorkBuddy 的常规技能作用域；具体安装、启用和管理界面可能随版本变化。若未发现，检查当前会话的技能管理页、项目工作区根和启用状态，不要删除 `.workbuddy` 项目数据目录。

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
  open_questions: ["实际 WorkBuddy 自动发现与探针需在本机验证"]
  appeal_path: "安装文档无强不一致判定；路径差异以当前技能管理页为准"
```

<!-- FILE END: install/workbuddy.md -->
<!-- PROGRESS: 17/21 -->
