# Golden 回归目录

本目录保存已审核的确定性文本基线，用于发现非预期产物变化。golden 比较不调用模型、不验证语义正确性，也不证明接管探针或 37 个语义用例已通过。

## 当前产物与关键用例

- 当前产物放在 `tests/fixtures/`，基线放在 `tests/golden/`。
- 每个当前文件必须有同名 golden 文件；每个 golden 文件也必须有对应当前文件。
- 初始 `basic-merge.md` 是合成结构样例，仅覆盖 frontmatter、provenance、worked example 三检查和摘要的文本回归；核心文件的边界/进度由独立结构检查验证。
- C01–C37 的关键语义产物若加入 golden，文件名应包含用例编号，例如 `C24-circuit-breaker.md`、`C26-router.md`、`C32-incremental.md`。
- 语义 golden 必须由审核人确认原文证据和预期处置后才能提交，不得把模型一次输出直接当真值。

## 命名规范

- 使用小写字母、数字和连字符；语义用例保留大写用例编号前缀以与 `tests/cases.md` 对齐。
- 当前产物与基线使用相同相对文件名和扩展名。
- 禁止时间戳、随机 ID、绝对路径和环境相关文本，以保证跨 runner 确定性。

## 审核元数据

每次新增或更新 golden，必须在 `tests/golden/change-log.yml` 增加一条：

```yaml
- file: "example.md"
  change: "说明预期变化及原因"
  source_case: "C00 或 structural-fixture"
  reviewer: "真实审核人标识"
  reviewed_at: "YYYY-MM-DD"
  evidence: "指向审核记录、issue 或产物章节"
```

审核人和审核日期不得留空，不得用 Agent 自评冒充人工审核。初始结构基线可标为维护者引导记录，但其语义仍须独立复核。

## 更新流程

1. 先运行当前检查并保存完整差异。
2. 判断差异是否符合预期规则变更；非预期变化应修复当前产物，而不是修改 golden。
3. 对预期变化更新对应规范/用例，添加变更记录并取得人工审核。
4. 只更新受影响文件，避免无关格式化噪声。
5. 重跑 frontmatter、Rubric 记录校验和 golden 比较，并在提交说明中引用变更记录。

只有在规范变更、已确认的错误修复或新增经审核用例时允许更新 golden。**禁止通过无解释地覆盖 golden 来“修复”失败。**

## CI 比较方式

`.github/workflows/validate.yml` 的文本比较使用 Python 标准库，变更记录解析使用 workflow 中固定版本的 PyYAML：

1. 检查 `tests/fixtures/` 与 `tests/golden/` 非空。
2. 比较相对文件集合，缺失或多余均失败并提示初始化/登记方式。
3. 将 UTF-8 文本的 CRLF 规范化为 LF 后逐字比较，输出统一 diff。
4. 检查每个基线在 `change-log.yml` 有显式登记。
5. 任一差异失败；不得自动更新仓库文件。

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
  open_questions: ["golden 仅验证文本稳定性，不验证语义真值"]
  appeal_path: "对基线差异提交证据化变更记录并人工复核"
```

<!-- FILE END: tests/golden/README.md -->
<!-- PROGRESS: 11/21 -->
