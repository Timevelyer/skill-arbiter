# 37 个验收用例的实际执行状态

本文件记录 `tests/cases.md` 中 C01–C37 的**实际执行结果**。`cases.md` 只定义规格与预期；此处不得预先填 `pass`，也不得用一次模型输出直接当真值。

## 状态约定

| 状态 | 含义 |
|---|---|
| `not-run` | 尚未执行 |
| `pass` | 实际产物满足该用例验收标准，并附证据位置 |
| `fail` | 实际产物不满足，附差异与回溯记录 |
| `blocked` | 依赖未就绪无法执行，说明阻塞项 |

## 记录表

| 用例 | 状态 | 执行人 | 日期 | 证据 / 备注 |
|---|---|---|---|---|
| C01 | `not-run` | | | |
| C02 | `not-run` | | | |
| C03 | `not-run` | | | |
| C04 | `not-run` | | | |
| C05 | `not-run` | | | |
| C06 | `not-run` | | | |
| C07 | `not-run` | | | |
| C08 | `not-run` | | | |
| C09 | `not-run` | | | |
| C10 | `not-run` | | | |
| C11 | `not-run` | | | |
| C12 | `not-run` | | | |
| C13 | `not-run` | | | |
| C14 | `not-run` | | | |
| C15 | `not-run` | | | |
| C16 | `not-run` | | | |
| C17 | `not-run` | | | |
| C18 | `not-run` | | | |
| C19 | `not-run` | | | |
| C20 | `not-run` | | | |
| C21 | `not-run` | | | |
| C22 | `not-run` | | | |
| C23 | `not-run` | | | |
| C24 | `not-run` | | | |
| C25 | `not-run` | | | |
| C26 | `not-run` | | | |
| C27 | `not-run` | | | |
| C28 | `not-run` | | | |
| C29 | `not-run` | | | |
| C30 | `not-run` | | | |
| C31 | `not-run` | | | |
| C32 | `not-run` | | | |
| C33 | `not-run` | | | |
| C34 | `not-run` | | | |
| C35 | `not-run` | | | |
| C36 | `not-run` | | | |
| C37 | `not-run` | | | |

## 填写要求

1. 只在真实执行后改为 `pass` / `fail`，并填写执行人、日期与证据（产物章节、规则 ID、日志或工单链接）。
2. 不得批量填 `pass`；不得用"预期"替代"实际"。
3. 任一 `fail` 必须回溯到规则、模板或测试规格，按 `tests/evaluation-rubric.md` 的失分回溯规则处理。
4. 本文件不参与 CI 自动评分，它是人工执行记录；CI 只校验 `rubric-score.yml` 的评分记录结构。
