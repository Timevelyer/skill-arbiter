# 贡献指南

本项目是**纯文档 + 确定性 CI** 的技能包，不包含运行时脚本。任何改动都必须能通过 `.github/workflows/validate.yml` 的三道结构作业。

## 本地验证

```bash
python -m pip install --disable-pip-version-check pyyaml==6.0.2
python - <<'PY'
from pathlib import Path
import sys, yaml, re
root = Path('.')
errors = []
core = ['README.md','SKILL.md','reference/arbiter-rules.md','templates/rejection-report.md',
        'templates/merged-skill.md','templates/router-skill.md','templates/incremental-report.md',
        'tests/cases.md','tests/evaluation-rubric.md','tests/skillclash-pairs.md','tests/golden/README.md',
        'install/claude-code.md','install/codex.md','install/opencode.md','install/zcode.md',
        'install/deepseek-harness.md','install/workbuddy.md','.github/workflows/validate.yml',
        'decision-log.md','LICENSE','LAUNCH.md']
for index, rel in enumerate(core, 1):
    text = (root / rel).read_text(encoding='utf-8')
    if f'<!-- FILE END: {rel} -->' not in text: errors.append(f'{rel}: missing end marker')
    if f'<!-- PROGRESS: {index}/21 -->' not in text: errors.append(f'{rel}: missing progress marker')
    if text.count('```') % 2: errors.append(f'{rel}: unbalanced code fences')
yaml.safe_load((root / '.github/workflows/validate.yml').read_text(encoding='utf-8'))
yaml.safe_load((root / 'tests/rubric-score.yml').read_text(encoding='utf-8'))
print('\n'.join('ERROR: ' + e for e in errors) if errors else 'LOCAL STRUCTURE PASS')
sys.exit(1 if errors else 0)
PY
```

行尾必须是 LF：本仓库 `.gitattributes` 强制 `text=lf`，请勿用会写入 CRLF 的编辑器配置。golden 回归是逐字节比较，CRLF 会直接导致 CI 失败。

## 改动规则

1. **规则语义**改动必须同步更新 `reference/arbiter-rules.md`、相关模板与 `decision-log.md`，并在 `tests/cases.md` 增加或修正对应用例。
2. **不得**为了让 CI 变绿而：填造评分、把 `not-run` 改成 `passed`、无解释覆盖 `tests/golden/`、删除用例。
3. 修改 `tests/fixtures/` 中被 golden 覆盖的文件时，必须在 `tests/golden/change-log.yml` 登记 `reviewer`、`reviewed_at`、`evidence`。
4. 新增宿主的安装说明时，只写**可核实**的路径与命令；未验证项必须在文中显式标注"未验证"。

## Pull Request 检查清单

- [ ] `LOCAL STRUCTURE PASS`，且 `git diff` 未引入 CRLF
- [ ] 若改了规则：`decision-log.md` 已追加决策与理由
- [ ] 若改了 golden：`change-log.yml` 已登记审核证据
- [ ] 若改了安装路径：注明来源链接与验证日期
- [ ] 未把"预期"写成"已通过"

## 提交信息

采用 `type: subject` 形式，`type` 取 `feat` / `fix` / `docs` / `test` / `ci` / `chore`。
