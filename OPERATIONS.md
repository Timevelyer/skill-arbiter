# skill-arbiter 操作指南

> 面向使用者和维护者的单页手册。目标：**先能在 5 分钟内用起来**，再决定要不要走发布流程。
>
> 当前版本 9.0.0 ｜ 最近更新 2026-11-23 ｜ 仓库 `Timevelyer/skill-arbiter`（private）

---

## 0. 一分钟决策

| 你的目标 | 走哪一节 | 状态 |
|---|---|---|
| 想立刻用它裁决两份技能 | [第 1 节](#1-快速使用) | 可用，约 5 分钟 |
| 打 `v9.0.0` 标签发布 | [第 2 节](#2-发布流程四道门禁) | 2.1 / 2.2 已签收，2.3 已跳过，2.4 待验证 |
| 跑起来不对，想定位问题 | [第 3 节](#3-故障排查) | — |

**当前状态**：仓库 38 个文件已推送，CI 三作业全绿；发布门禁 `rubric-gate` 在签收后可通过。

**已知缺口（写在这里，避免被忽略）**：37 个语义用例**未执行**；六个宿主的安装与调用**未验证**。这两项不阻塞发布，但也不能说成"已验证"。

---

## 1. 快速使用

### 1.1 安装

技能就是一个目录，复制到宿主的技能路径即可。**WorkBuddy（当前环境）**：

```bash
cd H:/Workbuddy_Workshop
mkdir -p ~/.workbuddy/skills
cp -R ./skill-arbiter ~/.workbuddy/skills/skill-arbiter
```

其他宿主的目标目录：

| 宿主 | 用户级目录 | 项目级目录 | 详细说明 |
|---|---|---|---|
| Claude Code | `~/.claude/skills/skill-arbiter` | `<repo>/.claude/skills/` | [install/claude-code.md](install/claude-code.md) |
| Codex | `~/.agents/skills/skill-arbiter` | `<repo>/.agents/skills/` | [install/codex.md](install/codex.md) |
| OpenCode | `~/.config/opencode/skills/skill-arbiter` | `.opencode/skills/` | [install/opencode.md](install/opencode.md) |
| ZCode | `~/.zcode/skills/skill-arbiter` | `~/.zcode/skills/` | [install/zcode.md](install/zcode.md) |
| DeepSeek Harness | `~/.dsh/skills/skill-arbiter` | `<project>/.dsh/skills/` | [install/deepseek-harness.md](install/deepseek-harness.md) |
| WorkBuddy | `~/.workbuddy/skills/skill-arbiter` | `<workspace>/.workbuddy/skills/` | [install/workbuddy.md](install/workbuddy.md) |

通用命令模板（把 `~/.xxx/skills` 换成上表对应目录）：

```bash
mkdir -p ~/.xxx/skills && cp -R ./skill-arbiter ~/.xxx/skills/skill-arbiter
```

### 1.2 验证安装

```bash
test -f ~/.workbuddy/skills/skill-arbiter/SKILL.md && echo "OK: 技能文件已就位"
```

文件在只是第一步——**必须新开一个会话**，当前会话的上下文里没有新技能。

### 1.3 输入格式

触发语句里要明确点名技能，并把两份文本用分隔标记隔开：

````text
用 skill-arbiter 分析下面两份技能。

=== 技能 A: alpha ===
（粘贴 alpha 的完整 SKILL.md，含 frontmatter）

=== 技能 B: beta ===
（粘贴 beta 的完整 SKILL.md）
````

要点：

- **粘贴全文**，不要只给结论或摘要。技能会逐条提取原子规则，缺正文就没法给证据。
- 三份以上同样处理，加 `=== 技能 C: gamma ===` 即可。
- 增量模式粘贴基线：`=== BASE ===` + 新技能。

### 1.4 预期输出

| 部分 | 内容 |
|---|---|
| 解析摘要 | 每个技能抽出多少条原子规则、指纹、是否截断 |
| pair 矩阵 | 两两之间的关系（六类之一）与严重度 S0–S3 |
| 证据 | 双方**逐字原文** + 规则 ID，不接受转述 |
| 全局判定 | `merged` / `router` / `rejected` / `human-review` |
| provenance | 每条源规则的唯一去向：保留 / 去重 / 条件覆盖 / 排除 |
| worked example | 覆盖、负向、最小性三检查 |
| `arbiter-summary` | 合法 YAML，供机器读取 |

一句话判断质量：**凡是给不出双方原文的强结论，都可以当作无效**。

### 1.5 安装后仍然报"找不到技能"

1. 确认目录名与 frontmatter `name` 一致（都必须是 `skill-arbiter`）。
2. 确认 `SKILL.md` 在第一层，不是 `skill-arbiter/skill-arbiter/SKILL.md`。
3. 确认开了新会话。
4. 各宿主的菜单显示方式不同，见对应 `install/*.md`；**未在真实环境验证过的宿主，说明里已标注"未验证"**。

---

## 2. 发布流程（四道门禁）

发布门禁只在你推 `v*` 标签时运行。日常 push 只跑结构校验，所以现在仓库是绿的；一旦打标签，门禁会拦住未签收的状态。

### 2.1 签收评分（已完成）

文件：[tests/rubric-score.yml](tests/rubric-score.yml)

| 字段 | 当前值 |
|---|---|
| `review_status` | `"approved"` |
| `reviewer` | `"Timevelyer"` |
| `reviewed_by_role` | `"human"` |
| `signoff_basis` | 说明本次签收依据所有者指示、以自动首评为证据基础 |
| `independently_read` | `false` |

**记录完整 ≠ 独立人工审计。** 若你之后逐条读完五维并对某些分数有异议，应改分并新增一条审核记录（保留旧分与差异原因），而不是覆盖。

当前分数：

| 维度 | 当前分 | 依据在哪 |
|---|---:|---|
| 规则保留完整度 | 4 | `reference/arbiter-rules.md` 映射表 + provenance 表 |
| Worked example 覆盖度 | 4 | `templates/merged-skill.md` 三检查 |
| 接管抗性 | 4 | 本文件 `probes` 段的三段原始输出 |
| DevEx 可读性 | 4 | README 架构图 + 摘要 schema 一致性 |
| 路由与修复路径 | 4 | `templates/router-skill.md` 决策表 |

均值 **4.00**，恰好压在 ≥4.0 的线上。**任何一维降到 3 都会让均值掉到 3.8，门禁不再放行**——这是刻意的，别为了让 CI 变绿而调分。

### 2.2 golden 复核（已完成）

```bash
cd H:/Workbuddy_Workshop/skill-arbiter
diff -u tests/golden/basic-merge.md tests/fixtures/basic-merge.md
```

无输出（当前状态），[tests/golden/change-log.yml](tests/golden/change-log.yml) 已登记 `reviewer: Timevelyer`。若将来出现差异：先判断是规则变更还是产物错误，**不得无解释覆盖基线**。

### 2.3 37 个用例（已跳过）

文件：[tests/case-results.md](tests/case-results.md)｜规格：[tests/cases.md](tests/cases.md)

**当前状态：全部 `not-run`，所有者选择跳过。** `rubric-score.yml` 里 `case_runs.skipped_by_owner: true` 已记录该决定。

这意味着：本仓库**没有** 37 个用例的通过率，任何"测试全通过"的说法都是不成立的。门禁 D 不检查这一节，所以发布不受影响。

若日后要补，按下面顺序起步（先验证流程，再铺开）：

| 建议顺序 | 用例 | 测什么 | 为什么先跑 |
|---:|---|---|---|
| 1 | C36 | Mermaid 与 JSON 边列表不一致 | 纯结构，不依赖语义判断 |
| 2 | C37 | worked example 违反保留规则 | 有明确负向信号 |
| 3 | C28 | 输入缺 `name` / `description` | 输入错误路径 |
| 4 | C29 | 两个同名技能正文不同 | 输入错误路径 |
| 5 | C01 | 首行标题 vs 首行状态 | 最典型的 S1 硬矛盾 |

每跑完一条填一行：

```markdown
| C36 | `pass` | Timevelyer | 2026-11-23 | 产物第 6 节图/边一致性；规则 ID R12 |
```

**不得批量填 `pass`**，也不得用规格里的"预期"代替实际产物。

### 2.4 六宿主验证

在每个宿主的真实环境执行 1.1 的复制命令，然后新开会话触发一次。验证完在 [LAUNCH.md](LAUNCH.md) 勾选。

未验证的**保持空白**，不要为了让清单好看而勾选。目前 OpenCode / Codex / ZCode 按既定约束未在本机安装。

### 2.5 打标签与发布

上面 2.1–2.3 完成后：

```bash
cd H:/Workbuddy_Workshop/skill-arbiter
git add -A && git commit -m "chore: record human sign-off for 9.0.0"
git push
git tag v9.0.0 && git push origin v9.0.0
gh release create v9.0.0 --title "skill-arbiter 9.0.0" --notes-file LAUNCH.md
```

推送 tag 会触发 `rubric-gate`。**通过**才创建 Release；**失败**说明还有一项没签收，按报错补。

> 远端使用 SSH（`git@github.com:Timevelyer/skill-arbiter.git`）。`gh` 的 OAuth token 缺 `workflow` scope，走 HTTPS 推 workflow 文件会被拒；如需改回 HTTPS，先 `gh auth refresh -h github.com -s workflow`。

---

## 3. 故障排查

| 现象 | 原因 | 处理 |
|---|---|---|
| push 报 `refusing to allow an OAuth App to create or update workflow` | token 缺 `workflow` scope | 改用 SSH 远端，或刷新 token 授权 |
| CI 报 `golden ... unexpected difference` | 产物与基线不一致，或 checkout 变成 CRLF | 检查 `.gitattributes` 是否生效；确认差异是预期变更后再登记基线 |
| CI 报 `case-results.md must list C01-C37` | 用例表漏行或顺序错 | 补齐 37 行，编号与 `cases.md` 对齐 |
| 门禁报 `review_status is not approved` | 尚未人工签收 | 见 2.1，这是设计行为 |
| 门禁报 `requires reviewed_by_role: human` | 只改了状态没改角色 | 两行要一起改 |
| 门禁报 `average ... below 4.0` | 某维降分导致均值不足 | 要么改回，要么真的修产物再评 |
| 技能不触发 | 目录层级错 / 未开新会话 | 见 1.5 |

---

## 4. 边界（写在这里，避免误期待）

1. 本技能是**离线文档型协议**，不调用外部 API，不扫描未提供的仓库，不执行源技能。
2. 语义判断依赖模型或人工分析，**不能保证 100% 准确**；贪心团检测是近似方法。
3. L2/L3 降级会损失证据能力，L3 只能给"疑似"。
4. worked example 是必要交付物，但**不能保证零接管**。
5. CI 只校验结构与记录完整性，**不理解语义，也不替你评分**。
6. 跨 Agent 只依赖 `name`、`description` 与正文这一公共核心，不依赖各宿主的私有扩展。

---

## 5. 相关文件

| 想了解 | 看这个 |
|---|---|
| 八层流程与三条铁律 | [SKILL.md](SKILL.md) |
| 六类关系的判定信号与反例 | [reference/arbiter-rules.md](reference/arbiter-rules.md) |
| 四类报告长什么样 | [templates/](templates/) |
| 评分标准与锚定描述 | [tests/evaluation-rubric.md](tests/evaluation-rubric.md) |
| 门禁逐步执行手册（更细） | [tests/gate-runbook.md](tests/gate-runbook.md) |
| 为什么这样设计 | [decision-log.md](decision-log.md) |
| 发布前还差什么 | [LAUNCH.md](LAUNCH.md) |
