# PRD — [产品名]

> 日期：YYYY-MM-DD ｜ 上游 frame：`docs/frames/<name>.md` ｜ 上游 audit：`docs/audits/<name>.md`
> Verdict 引用：[amazing/refactor] ｜ Tagline：「[8 字以内]」

---

## Oscillation Log（振荡记录）

```
[YYYY-MM-DD HH:MM] · [Phase X → Phase Y]（reason: [...]）
[YYYY-MM-DD HH:MM] · [Phase Y → Phase X]（reason: [...]）
...
```

**总跳转数**：[N] / 5
**最大相邻 Phase 来回数**：[N] / 3
**HARD GATE 通过状态**：[ ☐ 通过 / ☐ 红灯回 C 段 ]

---

## 1 · One-pager

**Tagline**：[8 字以内]

**Problem**（来自 Frame Pack Step 3）：
[1 段。问题不是方案。]

**Solution**（来自 Frame Pack Step 4 + Phase 1 hypothesis）：
[1 段。聚焦核心动作，不展开功能列表。]

---

## 2 · Hypothesis（来自 Phase 1）

**主 hypothesis**：

> We believe **[persona, 来自 Frame]**
> needs **[capability]**
> in order to **[outcome]**

**Leading indicator**（1 周内可观察）：
- [...]

**Lagging indicator**（1-3 月可衡量）：
- [...]

**1-week falsification plan**（来自 audit Pass 3 的 PoL probe）：
- Probe type: [Feasibility / Task-Focused / Narrative / Synthetic Data / Vibe-Coded]
- Day-by-day plan: [...]

---

## 3 · Stories — MUST HAVE（MVP 范围，来自 Phase 4）

### S001 · [story title]

**Story**：
> As [persona], I want to [action], so that [benefit].

**Acceptance Criteria**（Gherkin）：
```gherkin
Given [初始状态]
When  [用户动作]
Then  [预期结果，必须可验证]
```

**Sub-agent dispatch hints**：
```yaml
type: code-writer | researcher | reviewer | tester
primary_skill: [skill 路径，例：superpowers/test-driven-development]
reference_skills:
  - [可选辅助 skill]
context_files:
  - [可选参考文件路径]
acceptance_check: [自动化验收命令，例：pytest 全过 + 覆盖率 ≥ 80%]
estimated_complexity: low | medium | high
human_review_gate: [何时必须人工 review，可填 "无" 表示全自动]
```

---

### S002 · [story title]
[同上结构]

---

### S003 · [story title]
[同上结构]

> ⚠️ MUST 列表上限 3-5 个。如果你写了 6 个以上，回 Phase 4 砍 50%。

---

## 4 · Stories — SHOULD HAVE（强化版有，可选实现）

按 story title 列出，1 行简介即可，不展开 acceptance：

- [story title 1] — [1 行简介]
- [story title 2] — [1 行简介]

---

## 5 · Stories — COULD HAVE（锦上添花）

- [story title 1] — [1 行简介]

---

## 6 · OUT OF SCOPE — WON'T HAVE（来自 Phase 4）

**写下来就是为了拒绝引诱**：

- [story title 1] — 为什么不做：[...]
- [story title 2] — 为什么不做：[...]

---

## 7 · Risks & Open Questions

来自 audit weakest link + Frame Risks + Phase 1-4 累积疑问：

| # | 风险 / 疑问 | 应对 / 决策 |
|---|----------|-----------|
| R1 | [来自 audit weakest link] | [...] |
| R2 | [来自 Frame differentiation 红/黄灯] | [...] |
| R3 | [Phase 1-4 中 hypothesis 的不确定点] | [...] |
| Q1 | [需要在实施时回答的开放问题] | [谁回答 / 什么时候] |

---

## 8 · Definition of Done（DoD）

完整的 ship 条件，verifiable：

- [ ] **MUST** 列表全部 acceptance 通过
- [ ] Leading indicator 1 周内观测到（具体阈值：[...]）
- [ ] Sub-agent dispatch 全部完成且 human review gate 通过
- [ ] Kill-switch metric（来自 audit Pass 3）未触发
- [ ] 任何 R1-R3 风险都有应对落地

---

## 9 · Sub-agent Dispatch Plan（聚合视图）

把 MUST 列表里所有 sub-agent hints 聚合成一张表，方便派单：

| Story | type | 适合派给 | 复杂度 | human gate |
|-------|------|--------|------|-----------|
| S001 | code-writer | [agent type / skill 路径] | medium | [是 / 否 / 何时] |
| S002 | researcher | [...] | low | [...] |
| S003 | tester | [...] | high | [...] |

**派单建议**（用于 Task tool / 子 agent 调度）：
1. **并行可行**：S001 + S002（独立无依赖）
2. **必须串行**：S003 在 S001 完成后（依赖 schema）
3. **人工守门**：S001 schema 设计 merge 前需 user review

---

## 10 · 上游一致性自检

| 链条 | 通过？ |
|------|--------|
| Phase 1 hypothesis ↔ Frame Step 3 problem：解决的是同一个问题吗？ | ☐ |
| Phase 2 stories ↔ Frame Step 2 JTBD：用户动作和 JTBD 一致吗？ | ☐ |
| Phase 4 MUST scope ↔ audit "留下的核心动作"：核心动作在 MUST 里吗？ | ☐ |
| Phase 5 PRD ↔ audit Verdict：refactor 的 N 颗螺丝在 PRD 里都有应对吗？ | ☐ |
| Sub-agent dispatch ↔ 用户工作模式：每个 MUST 都能派给 sub-agent 吗？ | ☐ |

任一不通过 → 回相应 Phase 重做。

---

## 下一步

- [ ] **HARD GATE 通过** → 派单：用 Task tool 把 MUST 故事派给对应 sub-agent
- [ ] **HARD GATE 红灯** → 回 C 段 / B 段
- [ ] **PRD 路径**：`docs/prds/YYYY-MM-DD-<产品名>.md`
- [ ] **完成实施后** → 进入 E 段 `product-verdict`

---

## 元数据

- 角色切换：本次 PRD 在每个 Phase 的 .2 段进入 Steve Jobs 视角检查（基于公开言论推断，非本人观点）；HARD GATE 通过后退出
- 振荡总数：[N 次 ↕]（评估流程效率，不评估个人）
- 总用时：[实际多少分钟]
- 重审建议：实施开始后若发现 PRD 不准（hypothesis 错 / story 漏 / scope 估错）→ 允许回相应 Phase 重做；不要硬撑
