# Skill Arbiter V9.0 发布清单

## 当前准备状态（2026-11-23）

- 机器可执行的结构检查、CI 作业与打包已完成并通过；`rubric-gate` 已实测在未签收时阻断、签收后放行。
- 仓库已推送至 `github.com/Timevelyer/skill-arbiter`（private），远端 CI 三作业全绿。
- 三个接管探针已真实执行并逐字记录，五维 Rubric 首评 4/4/4/4/4，已按所有者指示签收（`reviewer: Timevelyer`）。
- 按要求，本机**未**将本技能安装到 OpenCode、Codex、ZCode；这三项宿主验证留待目标环境完成。
- **37 个语义用例未执行**（所有者选择跳过），`tests/case-results.md` 全部 `not-run`，不得引用为通过。
- 签到的性质见 `tests/rubric-score.yml` 的 `signoff_basis`：记录完整 ≠ 独立人工审计。

## 发布前检查

### 核心结构与许可证

- [x] 21 个核心文件齐全，边界与进度为 1/21–21/21；CI 支撑文件单独列示。
- [x] `README.md` 首段定位准确，第二段包含 SkillClash 与“成对兼容不代表组合兼容”。
- [x] `LICENSE` 为完整 MIT，版权行为 `Copyright (c) 2026 skill-arbiter contributors`。
- [x] README 的 MIT 声明指向 `LICENSE`，本文件保留同一引用：[MIT License](LICENSE)。
- [x] 所有内部链接、模板引用、安装路径和版本号经过静态检查。

### 规则与事实

- [x] 八层流程完整，pair 不被当成全局结论。
- [x] S0 禁路由、S3 不熔断、L3 不单独拒绝三条铁律一致。
- [x] Intent 至少两项独立量化信号；强矛盾均有双方原文、反方复核和申诉。
- [x] 所有源规则在 provenance 中唯一处置；worked example 三检查完整。
- [x] SkillClash 数字保持“上游报告，本项目未独立复现”表述。
- [x] 不包含编造的 Star、下载量、用户反馈、测试通过率、仓库路径或许可证。

### 验证与人工门禁

- [x] frontmatter / rubric-structure / golden CI 已在 GitHub Actions 与本地等价执行并保留结果。
- [x] 工作流 `rubric-gate` 已实测在未签收时阻断 `v*` 发布。
- [x] 三个接管探针已真实执行并记录模型、宿主、输入、原始输出、审核人与日期。
- [x] golden diff 无差异（CI job C 每次 push 复验），并已在 `change-log.yml` 登记签收人与证据。
- [x] `tests/rubric-score.yml` 已签收：`review_status: approved`、`reviewed_by_role: human`、`reviewer: Timevelyer`；平均分 4.00 且每维 ≥3。
- [x] 三个接管探针已真实执行并记录模型、宿主、输入、原始输出、审核人与日期。
- [ ] 六个目标 Agent 的当前版本安装与显式调用已在对应环境验证；未验证项公开列出。
  - 按要求，本机**未**安装 OpenCode、Codex、ZCode；三项不得标记为已验证。
- [ ] 37 个用例的实际状态逐条更新，不把用例规格当通过记录。
  - 所有者选择跳过执行；全部保持 `not-run`，`case_runs.skipped_by_owner: true`。此项在发布时保持未勾选。

## Release notes 草稿

```markdown
# skill-arbiter 9.0.0

skill-arbiter 是 N 个 SKILL.md 的全局一致性预检、冲突解决与合并引擎。

## Included

- Offline eight-layer workflow for pasted SKILL.md text.
- Six relation types, S0–S3 severity, evidence and appeal requirements.
- Merge, deterministic router, rejection, and incremental-review templates.
- Thirty-seven test specifications and a five-dimension human rubric.
- Installation guidance for Claude Code, Codex, OpenCode, ZCode, DeepSeek Harness, and WorkBuddy.
- Structural CI for frontmatter, recorded human rubric scores, and golden text regression.

## Verification status

- Do not interpret structural checks as model evaluation.
- Record actual CI, human review, host installation, and probe results before publishing.
- SkillClash figures are upstream-reported values and have not been independently reproduced by this project.

## Known limits

Semantic analysis is not guaranteed; clique detection is approximate; fingerprints alone never justify rejection; a worked example does not guarantee zero takeover.
```

发布时只能把已实际完成的验证改写为完成状态，并链接到记录；不得删除未完成的限制。

## GitHub topics

```text
workbuddy
ai-agent
skill
conflict-detection
skill-merge
multi-skill
agent-skills
prompt-engineering
```

## 社交文案草稿

```text
skill-arbiter 9.0：面向多 SKILL.md 组合的离线预检、冲突消解与合并规范。

它不只做 pair 比较，还要求三元/N 元检查、逐字证据、对抗复核、provenance 和 worked example 三检查。支持合并、可判定 Router、拒绝报告与 BASE/SKILL 增量重检。

安全边界：S0 不能靠路由绕过，S3 不触发熔断，L3 指纹不能单独拒绝。
仓库：<发布时填写真实仓库 URL>
验证状态与未执行的人工/模型门禁见 README。
```

不得加入“零冲突”“100% 准确”“全部测试通过”或未经证实的用户数量。

## 演示截图计划

1. **输入截图**：展示两份脱敏、合成 `SKILL.md` 的粘贴格式；避免泄露私有规则。
2. **熔断截图**：同屏展示三节点 S1 图、逐边证据、零文本改动方案与申诉路径；标注“合成示例”。
3. **合并截图**：同屏展示决策规则、provenance 和 worked example 三检查；探针未运行时可见 `not-run`。
4. **CI 截图**：分别展示 frontmatter、Rubric 记录和 golden job；不得裁掉 pending/failed 状态制造全绿印象。
5. **安装截图**：展示目标宿主当前版本技能列表中的 `skill-arbiter`，记录宿主版本和日期。

截图文件名建议：`demo-input.png`、`demo-circuit-breaker.png`、`demo-merge.png`、`demo-ci.png`、`demo-install-<agent>.png`。截图尚未生成时，发布清单不得标记已完成。

## 发布风险与回滚

| 风险 | 影响 | 缓解 | 回滚 |
|---|---|---|---|
| 宿主路径或 frontmatter 行为变化 | 技能不被发现 | 按当前官方文档复核并记录版本 | 撤回安装文档变更，恢复上一已验证说明 |
| 语义误判或证据不足 | 错误拒绝/合并 | 保守降级、人工复核、申诉 | 撤回对应报告模板变更，要求全量重检 |
| Golden 被无解释覆盖 | 回归被隐藏 | 强制 change log 与审核 | 恢复上一基线并保留差异记录 |
| 探针结果被夸大 | 错误安全预期 | 原始输出、模型/版本、not-run 状态 | 更正 release notes，撤回不实主张 |
| 上游素材许可证变化 | 再分发风险 | 只存索引，发布前复核许可证 | 移除受影响索引/素材并发布说明 |

回滚步骤：停止发布或撤回 release → 保留问题证据 → 恢复上一已审核标签/压缩包 → 通知用户需要全量重检 → 在新提交中修复并重新执行全部门禁。不得通过删除历史记录掩盖问题。

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
  open_questions: ["发布前人工评分、真实探针与宿主验证尚未完成"]
  appeal_path: "发布问题按风险表回滚并保留证据；不以修改总结代替复核"
```

<!-- FILE END: LAUNCH.md -->
<!-- PROGRESS: 21/21 -->
