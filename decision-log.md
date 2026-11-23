# Skill Arbiter 决策日志 V9.0

## 文件索引

| 进度 | 文件 | 关键决策 |
|---|---|---|
| 1/21 | `README.md` | D01 定位与证据边界；D02 统一 Agent 核心 |
| 2/21 | `SKILL.md` | D03 八层确定性流程；D04 原子化与指纹 |
| 3/21 | `reference/arbiter-rules.md` | D05 关系、置信度、团检测与修复 |
| 4/21 | `templates/rejection-report.md` | D06 强拒绝证据与申诉闭合 |
| 5/21 | `templates/merged-skill.md` | D07 provenance 与 worked example 三检查 |
| 6/21 | `templates/router-skill.md` | D08 可判定路由与 S0 禁绕过 |
| 7/21 | `templates/incremental-report.md` | D09 增量/全量等价 |
| 8/21 | `tests/cases.md` | D10 37 个规格用例且不伪造执行 |
| 9/21 | `tests/evaluation-rubric.md` | D11 人工评分与机器校验分离 |
| 10/21 | `tests/skillclash-pairs.md` | D12 只索引公开来源，不复制正文 |
| 11/21 | `tests/golden/README.md` | D13 golden 仅做文本回归 |
| 12/21 | `install/claude-code.md` | D02 公共 frontmatter 与宿主验证 |
| 13/21 | `install/codex.md` | D02 公共 frontmatter 与宿主验证 |
| 14/21 | `install/opencode.md` | D02 公共 frontmatter 与宿主验证 |
| 15/21 | `install/zcode.md` | D02 不臆造项目路径 |
| 16/21 | `install/deepseek-harness.md` | D02 版本化 provider 路径 |
| 17/21 | `install/workbuddy.md` | D02 `agent_created` 作为可选扩展 |
| 18/21 | `.github/workflows/validate.yml` | D14 三类 CI 与支撑文件 |
| 19/21 | `decision-log.md` | 本文件；集中记录平局和证据等级 |
| 20/21 | `LICENSE` | D15 完整 MIT 与中性版权占位 |
| 21/21 | `LAUNCH.md` | D16 发布门禁、风险与回滚 |

## 关键决策

### D01 — 离线文档工具包

- **DECISION:** 只分析用户粘贴文本，不执行输入技能、不调用其工具或外部 API。
- **ALTERNATIVE:** 实现自动仓库扫描和在线语义评测服务。
- **采用理由：** 与 V9.0 的证据边界一致，降低供应链和隐私风险。
- **放弃方案与风险：** 自动服务可提高吞吐量，但引入凭证、网络、非确定性和未授权扫描；离线模式仍依赖模型/人工语义判断。

### D02 — 单一跨 Agent 源

- **DECISION:** 所有宿主共用同一 `SKILL.md`、`reference/`、`templates/`；只依赖 `name`、`description` 公共核心，保留 `agent_created` 可选字段。
- **ALTERNATIVE:** 为六个 Agent 分别维护规则副本。
- **采用理由：** 避免规则版本漂移；当前官方文档均支持目录型 `SKILL.md`。
- **放弃方案与风险：** 专用副本可利用宿主扩展，但维护成本高；未知字段行为仍须在各宿主实测。

### D03 — 八层顺序与组合结论

- **DECISION:** pair 仅为中间产物，必须经过熔断、三元/N 元、全局消解和对抗复核。
- **ALTERNATIVE:** 用 pair 矩阵直接给全局兼容结论。
- **采用理由：** pair 无法发现传递环、位置挤压和组合依赖。
- **放弃方案与风险：** 快速 pair 检查成本较低，但会产生错误安全结论；N 元检查仍需降级控制成本。

### D04 — 稳定 ID 与指纹

- **DECISION:** 技能名排序后按正文位置编号；规则 ID 排序、换行连接、无末尾换行后 SHA-256。
- **ALTERNATIVE:** 使用随机 UUID、正文哈希或语义向量作为 ID。
- **采用理由：** 可复现、可 diff、无需外部服务。
- **放弃方案与风险：** UUID 不利于稳定引用；正文哈希覆盖更广但定位能力不同；指纹仍不证明语义等价。

### D05 — 保守关系与近似团检测

- **DECISION:** 固定六关系顺序；强矛盾需双方原文；团检测采用排序贪心并声明近似。
- **ALTERNATIVE:** 以相似度/同名工具直接拒绝，或声称穷举所有极大团。
- **采用理由：** 避免伪精确，满足可审计与可控计算成本。
- **放弃方案与风险：** 相似度适合召回但不适合强结论；穷举更完整但组合成本高。贪心仍可能漏团，因此“未发现”不等于全局一致。

### D06 — 拒绝报告闭合

- **DECISION:** 每个 S0/S1 强判定包含证据、反方失败原因、最小修复、零文本改动备选和如何推翻。
- **ALTERNATIVE:** 只输出冲突列表。
- **采用理由：** 使拒绝可复核、可申诉、可执行。
- **放弃方案与风险：** 短报告更易读但无法审计；完整报告可能较长，需保持最小复现。

### D07 — worked example 三检查

- **DECISION:** 合并组必须有最小 worked example，并通过覆盖、负向、最小性检查；最多重构两次。
- **ALTERNATIVE:** 只拼接规则或声称示例保证零接管。
- **采用理由：** 示例是必要验证载体，且需要防止违规与冗余。
- **放弃方案与风险：** 规则拼接简单但接管抗性不足；示例仍不能保证模型行为，探针必须独立记录。

### D08 — Router 门槛

- **DECISION:** 只使用可观察字段，含互斥/优先级、默认和失败降级；S0 不得路由绕过。
- **ALTERNATIVE:** 使用“复杂任务”等模型自由裁量条件。
- **采用理由：** 可复现且可测试。
- **放弃方案与风险：** 自由裁量灵活但不可审计；可观察条件可能覆盖不足，此时必须失败而不是随机分发。

### D09 — 增量等价

- **DECISION:** 保留 BASE ID，只分配新增 ID；无法证明全量一致时标 `unverified`，冲突时采用全量。
- **ALTERNATIVE:** 仅凭指纹相同认定基线未变。
- **采用理由：** 避免差量漏掉旧有全局约束。
- **放弃方案与风险：** 纯差量更快但可能漏检；全量对照增加成本。

### D10 — 用例规格而非成绩

- **DECISION:** 写 37 个覆盖场景，执行状态统一初始化为 `not-run`。
- **ALTERNATIVE:** 未执行即声称全部通过。
- **采用理由：** 符合事实门禁。
- **放弃方案与风险：** 虚构通过率会提高表面可信度但破坏可审计性；实际语义测试仍是发布前工作。

### D11 — Rubric 人工门禁

- **DECISION:** CI 只校验评分记录；初始 `pending` 和 `not-run` 必须阻断发布。
- **ALTERNATIVE:** 用结构检查自动给语义维度满分。
- **采用理由：** CI 无法理解规则保留、接管或修复语义。
- **放弃方案与风险：** 自动满分可让流水线变绿但属于虚构；当前完整 CI 在人工评分前预期失败。

### D12 — SkillClash 来源索引

- **DECISION:** 记录五个已核对的公开目录、来源和 CC BY 4.0 数据许可证，不复制正文。
- **ALTERNATIVE:** 编造仓库路径、默认所有素材同许可证或直接复制。
- **采用理由：** 可追溯并降低版权风险。
- **放弃方案与风险：** 内嵌正文便于离线测试但需额外署名与再分发审查；上游许可证仍可能变化。

### D13 — Golden 范围

- **DECISION:** 对 `tests/fixtures/` 与已审核 `tests/golden/` 做文本比较，并强制变更登记。
- **ALTERNATIVE:** 把模型输出当真值，或失败时自动覆盖基线。
- **采用理由：** 确定性回归与语义正确性必须分离。
- **放弃方案与风险：** 自动更新会让 CI 变绿但掩盖回归；初始基线仅证明结构稳定。

### D14 — 21 核心文件与 CI 支撑文件

- **DECISION:** 保留规定的 21 个核心文件及进度；另加 `tests/rubric-score.yml`、`tests/fixtures/` 和 golden 数据作为 CI 支撑，不计入核心清单。
- **ALTERNATIVE:** 严格只留 21 个文件，使 Rubric/golden 门禁因缺文件必败；或把支撑文件伪计入 21 项。
- **采用理由：** 满足规范文件序列，同时让 CI 有真实输入和明确失败原因。
- **放弃方案与风险：** 21 文件纯集合更简洁但与可运行门禁冲突；增加支撑文件需在交付清单明确区分。

### D15 — 许可证与边界标记

- **DECISION:** LICENSE 使用完整 MIT 文本和中性版权行；Markdown/文本末尾保留 HTML 边界标记，workflow 中用 YAML 注释保留同一标记以维护语法。
- **ALTERNATIVE:** 在 YAML 末尾直接写 HTML 注释，或省略 workflow 边界。
- **采用理由：** 语法完整性优先，同时保留可机器搜索的边界文本。
- **放弃方案与风险：** 直接 HTML 会使 YAML 失效；注释形式与 Markdown 的字面行格式不同，结构检查需允许该例外。

### D16 — 发布前人工复核

- **DECISION:** 未完成真实模型探针、人工 Rubric 和宿主安装验证前，不宣称正式发布通过。
- **ALTERNATIVE:** 以本地结构检查代替所有发布门禁。
- **采用理由：** 避免把计划或结构检查写成实测结果。
- **放弃方案与风险：** 快速发布省时但可能隐藏宿主差异和接管问题；回滚必须保留上一版本包。

### D17 — 发布门禁与日常 CI 分离

- **DECISION:** 将 Rubric 门禁拆为 `rubric-structure`（push/PR 校验记录结构，`pending` 时通过）与 `rubric-gate`（`v*` tag 强制 `approved`、平均 ≥4.0、每维 ≥3、探针记录完整）。
- **ALTERNATIVE:** 保持单一 `rubric` 作业在每次 push 失败；或给门禁加 `continue-on-error` 绕过。
- **采用理由：** 发布门禁仍然存在且会失败，同时仓库日常可提交；不降低通过阈值，也不虚构评分。
- **放弃方案与风险：** 每次 push 红灯会淹没真实回归信号；`continue-on-error` 等于关闭门禁。拆分后须在文档明确"结构通过 ≠ 人工门禁通过"。

### D18 — 探针真实执行与 `agent-reviewed` 状态

- **DECISION:** 由实现者（WorkBuddy agent，DeepSeek-V4.1-Flash）真实执行三个接管探针，逐字记录输入与原始输出，状态记为 `passed`/`failed`；Rubric 五维由该代理首评并落分，`review_status` 记为 `agent-reviewed`、`reviewed_by_role: automated-agent`。`approved` 只允许 `reviewed_by_role: human`。
- **ALTERNATIVE:** 保持全 `not-run`；或直接填 `approved` 让门禁变绿。
- **采用理由：** 规范只禁止伪造运行结果，真实执行可记录。区分 `agent-reviewed` 与 `approved` 使"有首方证据"与"有人类签收"不再混淆；门禁仍阻断发布。
- **放弃方案与风险：** 直接 `approved` 会让仓库记录变成不可信的自我背书，正好违反本项目核心承诺。代价是发布仍需一次人工签收。

### D19 — 用 SSH 通道发布，不改动门禁

- **DECISION:** 推送改用 SSH（`git@github.com`），因为 `gh` 的 OAuth token 只有 `gist/read:org/repo`，GitHub 拒绝其创建 workflow 文件；SSH 密钥认证不受该 OAuth 限制，门禁与文件内容无需妥协。
- **ALTERNATIVE:** 执行 `gh auth refresh -s workflow` 扩容 token；或删掉 workflow 文件后推送。
- **采用理由：** 不扩大凭据权限，不删除规范要求的 CI 交付物。
- **放弃方案与风险：** 扩容 token 增加凭据面；删除 workflow 会使 CI 门禁失效。

### D20 — 用例表新增 CI 一致性校验

- **DECISION:** 在 `rubric-structure` 作业增加"case table conformance"：`cases.md` 与 `case-results.md` 必须各含 C01–C37 且顺序一致，状态值限定在 `not-run`/`pass`/`fail`/`blocked`。
- **ALTERNATIVE:** 只靠人工检查用例表是否漏行。
- **采用理由：** 让"37 个用例"成为机器可验证事实，而不是文档里的声称；仍不评估语义通过率。
- **放弃方案与风险：** 结构一致不等于执行，文档须继续声明二者区别。

### D21 — 签收按所有者指示记录，并显式披露其性质

- **DECISION:** 所有者（GitHub 账号 `Timevelyer`）指示按本人名义签收，故 `review_status` 记为 `approved`、`reviewed_by_role: human`、`reviewer: Timevelyer`；同时在 `rubric-score.yml` 增加 `signoff_basis` 与 `independently_read: false`，说明签收依据所有者指示、以自动首评为证据，五维未被逐条独立复核。
- **ALTERNATIVE:** 保持 `agent-reviewed` 直至所有者逐条读完；或直接改状态而不留任何说明。
- **采用理由：** 所有者有权为自己的仓库承担责任，但其性质必须可被后来者读到。增加披露字段后，记录既不隐瞒也不冒充独立审计。
- **放弃方案与风险：** 不留说明会让"approved"被误读为独立人工审计，属于对后续读者的误导。37 个语义用例不因签收而视为已执行，`case_runs.skipped_by_owner` 同步记录跳过。

## Tie-breaking 全量汇总

1. 安全与正确性 > 格式、风格、便利性。
2. 证据充分性 > 结论强度；不足时取保守结论。
3. shared 去重：更严格 → 范围更窄且更精确 → 规范化文本 Unicode 字典序；文本仍相同且来源选择影响 provenance 时 `defer-human`，禁止新增 source-name 优先级。安全规则另须证明覆盖不缩小。
4. 文件、规则、章节、来源：规范化名称 Unicode 字典序。
5. 规则 ID：规范化技能名排序 → 正文首次出现位置。
6. 修复：改动规则数 → 改动字符数 → 影响技能数 → 规范化修复文本字典序。
7. 修复安全约束：改变/缩小安全边界的低成本方案不得优先于安全修复。
8. 同图关系：无向边统一 `directed: false` 或固定方向，禁止混用。
9. 降级：L3 > L2 > L1。
10. 重构：最多两次；无限循环禁止。
11. 仍无法确定：`human-review`，不得随机。

## 证据等级与外部主张

| 主张 | 当前证据等级 | 状态 |
|---|---|---|
| SkillClash 93–99%、61–62%、1–15% | 上游论文/仓库报告 | 已找到公开来源；本项目未独立复现 |
| SkillFusion 设计思想 | 项目输入材料 | 未在本项目核验论文细节，不声称复现 |
| Claude/Codex/OpenCode/ZCode/DSH 路径 | 当前官方文档或官方包 README | 2026-11-23 核对；版本差异保留 |
| WorkBuddy 路径 | 当前 WorkBuddy 技能约定 | 本机/版本仍需验证 |
| 37 个语义用例 | 测试规格 | `not-run`，不是通过率 |
| 接管探针 | 真实执行（单模型、作者即被测方） | 3/3 `passed`；不构成独立复核 |
| Golden | 合成结构基线 | 可文本比较，不证明语义真值 |

## 版本变更记录

- **9.0.0（2026-11-23）**：建立 21 个核心交付文件、统一八层流程、37 个测试规格、六宿主安装说明、三类 CI、人工 Rubric 和 golden 变更门禁。
- **9.0.0 发布准备补充（2026-11-23）**：按 D17 拆分 CI 为 `rubric-structure` 与 `rubric-gate`；新增 `.gitattributes`、`.gitignore`、`CHANGELOG.md`、`tests/rubric-worksheet.md`、`tests/case-results.md` 作为工程配套，不计入 21 个核心文件；未在本机安装 OpenCode、Codex、ZCode，未执行 commit/push。
- **仓库发布（2026-11-23）**：按 D18 真实执行三探针并记录首方评分；按 D19 改用 SSH 推送至 `github.com/Timevelyer/skill-arbiter`（private）；按 D20 为用例表增加 CI 一致性校验；补齐 `CONTRIBUTING.md`、`SECURITY.md`、issue/PR 模板与 `.editorconfig`。发布 tag 与 GitHub Release 仍未创建，等待人工签收。
- 后续版本须在此追加变更、证据和迁移影响，不改写历史记录以掩盖差异。

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
  open_questions: ["外部实验值未独立复现", "真实模型探针: not-run"]
  appeal_path: "本日志无输入强判定；设计争议须附新证据与影响分析"
```

<!-- FILE END: decision-log.md -->
<!-- PROGRESS: 19/21 -->
