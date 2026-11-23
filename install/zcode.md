# ZCode 安装

核对来源：[ZCode Skills 官方文档](https://zcode.net.cn/en/docs/skill)，核对日期 2026-11-23。

- **用户级目标**：`~/.zcode/skills/skill-arbiter/SKILL.md`
- **项目级目标**：当前引用页面未给固定项目目录；通过“设置 → 技能 → 导入”选择当前项目作为目标，并以页面显示路径为准。
- 也可从 Claude Code、Codex 等已安装来源使用 ZCode 的导入功能。

在包含 `skill-arbiter/` 的父目录执行一行复制命令：

```bash
mkdir -p ~/.zcode/skills && cp -R ./skill-arbiter ~/.zcode/skills/skill-arbiter
```

## Frontmatter 注意事项

- `name` 与 `description` 必须存在；目录名即技能名。
- `description` 超过 1024 字符会导致整个技能被丢弃，而不是截断。
- 当前文档说明正文超过 100KB 会截断；本项目的详细规则保留在按需引用的 `reference/` 中。
- frontmatter 中不含 `agent_created`（D23 已移除）；ZCode 的调用只依赖 `name` 与 `description`。

## 验证命令

```bash
test -f ~/.zcode/skills/skill-arbiter/SKILL.md && printf '%s\n' 'skill-arbiter file found'
```

打开“设置 → 技能”，点击“刷新”，确认 `skill-arbiter` 已启用。聊天中用 `$skill-arbiter` 或技能菜单显式调用。

## 预期输出

文件命令输出 `skill-arbiter file found`；设置页显示技能名称、来源和描述且无 frontmatter 诊断。调用后应生成证据化分析，不得只输出“已加载”。

## 版本差异与路径确认

用户路径来自当前官方文档；项目路径有意不臆造。远程主机默认看不到本地用户技能时，按官方远程开发文档使用“同步技能”。升级后若未出现，使用设置页导入对话框核对实际目录。

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
  open_questions: ["项目路径需由当前 ZCode 设置页确认", "实际探针: not-run"]
  appeal_path: "安装文档无强不一致判定；路径差异以当前设置页和官方文档为准"
```

<!-- FILE END: install/zcode.md -->
<!-- PROGRESS: 15/21 -->
