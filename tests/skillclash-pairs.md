# SkillClash 公开素材索引

本文件只记录公开素材的路径、用途、许可证和来源链接，不复制技能正文。以下路径与许可证于 2026-11-23 通过项目公开仓库页面核对；本项目未独立复现论文实验。

- 上游仓库：https://github.com/prazmara/skillclash
- 论文：https://arxiv.org/abs/2610.25064
- 数据目录：`skills/<job>/A.md` 与 `skills/<job>/B.md`
- 数据许可证：上游 `LICENSE-DATA` 标注 CC BY 4.0；代码许可证为 MIT。使用前仍须复核上游当前许可证与署名要求。
- 许可证链接：https://github.com/prazmara/skillclash/blob/main/LICENSE-DATA

| 对编号 | 仓库内路径 | 来源 URL | 用途 | 许可证 | 适用测试编号 |
|---|---|---|---|---|---|
| P01 | `skills/commit/A.md`, `skills/commit/B.md` | https://github.com/prazmara/skillclash/tree/main/skills/commit | 同任务 commit 技能对的授权复核与 N=2 关系测试 | CC BY 4.0（上游数据） | C01–C15；按实际规则选择 |
| P02 | `skills/sql/A.md`, `skills/sql/B.md` | https://github.com/prazmara/skillclash/tree/main/skills/sql | 同任务 SQL 技能对的关系与 worked example 测试 | CC BY 4.0（上游数据） | C01–C15、C37 |
| P03 | `skills/changelog/A.md`, `skills/changelog/B.md` | https://github.com/prazmara/skillclash/tree/main/skills/changelog | 同任务 changelog 技能对的冲突与合并测试 | CC BY 4.0（上游数据） | C01–C15、C37 |
| P04 | `skills/review/A.md`, `skills/review/B.md` | https://github.com/prazmara/skillclash/tree/main/skills/review | 同任务 review 技能对的接管探针素材 | CC BY 4.0（上游数据） | C01–C15、探针协议 |
| P05 | `skills/testcase/A.md`, `skills/testcase/B.md` | https://github.com/prazmara/skillclash/tree/main/skills/testcase | 同任务 testcase 技能对的覆盖与负向检查 | CC BY 4.0（上游数据） | C01–C15、C37 |

## 使用纪律

1. 在线获取素材是独立的数据准备动作，不属于 skill-arbiter 离线分析运行时。
2. 不把上游数据许可证扩展到本项目代码，也不把本项目 MIT 许可证强加给上游素材。
3. 若链接、路径或许可证无法复核，将对应记录改为 `license-unverified`，不得声称 CC BY 4.0。
4. 不在本仓库粘贴上游技能正文；实际测试须保留来源、版本/提交、许可证和必要署名。
5. 论文中的 93–99%、61–62%、1–15% 等数字是上游报告值，不是 skill-arbiter 自测成绩。

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
  open_questions: ["上游素材实际测试: not-run"]
  appeal_path: "本索引无强不一致判定；来源变化时重新核对链接与许可证"
```

<!-- FILE END: tests/skillclash-pairs.md -->
<!-- PROGRESS: 10/21 -->
