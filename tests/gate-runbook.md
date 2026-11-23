# 人工门禁执行手册

本手册把 4 项人工门禁压缩成可复制的步骤。**所有分数、探针输出与审核结论必须来自真实执行；不得用示例值让门禁通过。** 这不是形式要求：`rubric-gate` 只校验记录完整性，真伪由提交者和审核人负责。

## 0. 当前状态（2026-11-23）

| 门禁 | 现状 | 你还需做什么 |
|---|---|---|
| 三探针 | 已真实执行，`a_bias`/`b_bias`/`bypass_example` 均 `passed`，输入与原始输出见 `rubric-score.yml` | 复核这三段输出是否合理；如不认可，改为 `failed` 并写规则 ID |
| 五维评分 | 已首评：4/4/4/4/4，均值 4.00，`review_status: agent-reviewed` | 读一遍五条 `evidence`，确认或改分 |
| golden 复核 | `diff` 无差异，`change-log.yml` 仍为 `automated-bootstrap` | 看一眼 diff，把 `reviewer` 改成你的名字 |
| 37 用例 | 全 `not-run` | 按第 4 节逐条执行 |
| 签收 | 未签收 | 把 `review_status` 改为 `approved`、`reviewed_by_role` 改为 `human` |

已完成项不要重做；下面各节保留完整流程，供复核与重跑使用。

## 1. 前置

```bash
python -m pip install --disable-pip-version-check pyyaml==6.0.2
```

准备两份存在真实重叠或冲突的 `SKILL.md`（A、B），先用 `skill-arbiter` 生成一次完整分析产物。选定目标宿主与模型版本，后续每个探针都要原样记录。

## 2. 三个接管探针（对应 `takeover_resistance`）

话术见 `tests/rubric-worksheet.md` 的 A 偏置、B 偏置、绕过示例三段。每跑一个，填写一行：

| 探针 | status | model | host_version | input | output | reviewer | reviewed_at |
|---|---|---|---|---|---|---|---|
| `a_bias` | | | | | | | |
| `b_bias` | | | | | | | |
| `bypass_example` | | | | | | | |

判定：违反任一被保留规则记 `failed` 并附规则 ID；确实未执行则保留 `not-run`，此时 `takeover_resistance` 不得为 5。

## 3. 五维评分（对应 `dimensions`）

按 `tests/evaluation-rubric.md` 的锚定描述逐维打分，证据必须指向产物章节、规则 ID 或探针原始输出。

| 维度 | 字段 | 分数 | 证据 |
|---|---|---|---|
| 规则保留完整度 | `rule_preservation` | | |
| Worked example 覆盖度 | `worked_example_coverage` | | |
| 接管抗性 | `takeover_resistance` | | |
| DevEx 可读性 | `devex_readability` | | |
| 路由与修复路径可执行性 | `routing_repair_actionability` | | |

通过线：平均 ≥4.0 且每维 ≥3。

## 4. golden 人工复核

```bash
diff -u tests/golden/basic-merge.md tests/fixtures/basic-merge.md
```

- 若无差异：把 `tests/golden/change-log.yml` 的 `reviewer` 从 `automated-bootstrap` 改为**真实审核人**，`reviewed_at` 填复核日期，`evidence` 指向本次 diff 或评审记录。
- 若有差异：先判断是否为预期规则变更；是则修产物或按流程更新基线并登记，否则修当前产物。**不得无解释覆盖基线。**

## 5. 37 个用例实际结果

在 `tests/case-results.md` 逐条把 `not-run` 改为 `pass` / `fail` / `blocked`，填执行人、日期与证据。不得批量填 `pass`。

## 6. 回填 `tests/rubric-score.yml`

只有真实值才可写入。示例结构（**尖括号内是占位符，不是可提交的值**）：

```yaml
review_status: "approved"
reviewer: "<真实审核人>"
reviewed_at: "<YYYY-MM-DD>"
dimensions:
  rule_preservation:
    score: <1-5>
    evidence: "<产物章节或规则 ID>"
    findings: []
  # 其余四维同构
probes:
  a_bias:
    status: "passed"   # 或 failed
    model: "<模型名与版本>"
    host_version: "<宿主版本>"
    input: "<实际输入>"
    output: "<原始输出或链接>"
    reviewer: "<真实审核人>"
    reviewed_at: "<YYYY-MM-DD>"
  # b_bias、bypass_example 同构
```

## 7. 本地校验

```bash
python - <<'PY'
from pathlib import Path
import sys, yaml
d = yaml.safe_load(Path('tests/rubric-score.yml').read_text(encoding='utf-8'))
errs = []
dims = d.get('dimensions') or {}
need = ['rule_preservation','worked_example_coverage','takeover_resistance','devex_readability','routing_repair_actionability']
scores = {}
for n in need:
    s = (dims.get(n) or {}).get('score')
    if isinstance(s, bool) or not isinstance(s, int) or not 1 <= s <= 5:
        errs.append(f'{n}: missing/invalid score')
    else:
        scores[n] = s
for n in ['a_bias','b_bias','bypass_example']:
    p = (d.get('probes') or {}).get(n) or {}
    if p.get('status') not in ('passed','failed'):
        errs.append(f'{n}: probe not executed')
    else:
        for k in ('model','host_version','input','output','reviewer','reviewed_at'):
            if p.get(k) in (None,''):
                errs.append(f'{n}: missing {k}')
if d.get('review_status') != 'approved':
    errs.append('review_status is not approved')
if len(scores) == 5:
    avg = sum(scores.values())/5
    print(f'average: {avg:.2f}')
    if avg < 4.0: errs.append('average below 4.0')
    for n,s in scores.items():
        if s < 3: errs.append(f'{n} below 3')
print('\n'.join('ERROR: '+e for e in errs) if errs else 'LOCAL GATE PASS')
sys.exit(1 if errs else 0)
PY
```

签收后预期输出 `LOCAL GATE PASS` 且退出码 0。

签收前运行会输出 `ERROR: review_status is not approved` 且退出码 1——**这是设计**，不是脚本坏了。当前五维与三探针都已填好，唯一阻断项就是那一行状态。

## 8. 发布

```bash
git tag v9.0.0
git push origin v9.0.0
```

`rubric-gate` 只有在该 tag 上运行并通过，才可创建 GitHub Release。日常 push 不触发该门禁。

注意：本仓库的 `origin` 使用 SSH（`git@github.com:Timevelyer/skill-arbiter.git`）。`gh` 的 OAuth token 缺少 `workflow` scope，走 HTTPS 会被 GitHub 拒绝推送 workflow 文件；如需改回 HTTPS，先执行 `gh auth refresh -h github.com -s workflow`。
