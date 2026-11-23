# DeepSeek Harness（DSH）安装

核对来源：[官方 `dsh-skill-filesystem` README](https://github.com/deepseek-ai/deepseek-harness/blob/master/packages/skill/skill-filesystem/README.md)，核对日期 2026-11-23。DSH 仍可能快速演进，应以安装版本的包文档为准。

- **项目级目标**：`<projectRoot>/.dsh/skills/skill-arbiter/SKILL.md`
- **项目兼容目标**：`<projectRoot>/.agents/skills/skill-arbiter/SKILL.md`
- **用户级目标**：`~/.dsh/skills/skill-arbiter/SKILL.md`（若设置 `$DSH_HOME`，则为 `$DSH_HOME/skills/`）
- 共享 Agent 兼容目录：`~/.agents/skills/`（若设置 `$DSH_AGENTS_HOME`，以其为准）。

在包含 `skill-arbiter/` 的父目录执行一行复制命令：

```bash
mkdir -p ~/.dsh/skills && cp -R ./skill-arbiter ~/.dsh/skills/skill-arbiter
```

项目级安装改用：

```bash
mkdir -p .dsh/skills && cp -R ./skill-arbiter .dsh/skills/skill-arbiter
```

## Frontmatter 注意事项

- 目录包必须位于扫描根的直接下一级：`<root>/<name>/SKILL.md`；不要把本技能再嵌套一层。
- `name` 必须为 kebab-case，`description` 必需。
- DSH 还支持 `whenToUse`、`metadata`、`disable-model-invocation`、`user-invocable`；本项目不依赖这些扩展。
- `agent_created` 不属于 DSH 必需字段，不用于控制调用。

## 验证命令

```bash
test -f ~/.dsh/skills/skill-arbiter/SKILL.md && printf '%s\n' 'skill-arbiter file found'
```

确保已挂载 `@deepseek-ai/dsh-skill`、`@deepseek-ai/dsh-skill-filesystem`，并按官方 [`tool-skill` README](https://github.com/deepseek-ai/deepseek-harness/blob/master/packages/skill/tool-skill/README.md) 挂载 `@deepseek-ai/dsh-tool-skill` 以提供 catalog、loader 和 `/name` 调用。新会话目录中应出现 `skill-arbiter`；也可输入 `/skill-arbiter` 直接加载。文件系统 provider 会监视目录和 frontmatter 变化。

## 预期输出

文件命令输出 `skill-arbiter file found`；DSH catalog 中出现该技能，`/skill-arbiter` 或原生 skill loader 能返回正文。无效 frontmatter 会被跳过并记录 warning，因此“不存在”和“无效”需结合日志区分。

## 版本差异与路径确认

项目根是最近的 `.git` 祖先，无 `.git` 时为当前目录。优先级为 project-dsh、project-agents、自定义、user-dsh、user-agents；同名遮蔽可能使副本看似未加载。自定义根、环境变量和 Developer Preview 版本差异须通过当前配置及包 README 核对。

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
  open_questions: ["实际 DSH catalog 与探针需在本机验证"]
  appeal_path: "安装文档无强不一致判定；路径差异以当前包文档和配置为准"
```

<!-- FILE END: install/deepseek-harness.md -->
<!-- PROGRESS: 16/21 -->
