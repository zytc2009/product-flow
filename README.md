# Product Flow · 5 段产品开发管道

> 把"灵感 → 上线后迭代"完整切成 5 段，每段一个 skill。
> PM 框架搭骨架（Geoffrey Moore / JTBD / MITRE）+ Steve Jobs 视角做品味压测 + superpowers/brainstorming 的硬门协议。
> 解决"切来切去"的振荡 —— 5 段衔接清晰，每段唯一下游，承认振荡但限定预算。

---

## 5 段地图

```
A. spark    →  B. audit   →  C. frame  →  D. spec   →  E. verdict
   灵感         方向审计       5 件套        需求         上线后迭代
   收敛         3 档判决       展开          PRD         持守/调整/杀
```

| 段 | Skill | 输入 | 输出 | 用时 |
|----|-------|------|------|------|
| **A · Spark** | [`product-spark`](skills/product-spark/SKILL.md) | obsidian 笔记 / 口头想法 | 1-3 个候选 | 10-15 min |
| **B · Audit** | [`product-audit`](skills/product-audit/SKILL.md) | 候选切入语 | Verdict + kill-switch + 删除清单 | 15-20 min |
| **C · Frame** | [`product-frame`](skills/product-frame/SKILL.md) | audit 报告 | 5 件套 Frame Pack | 20-30 min |
| **D · Spec** | [`product-spec`](skills/product-spec/SKILL.md) | Frame Pack | 1.5 页 PRD + sub-agent dispatch | 30-60 min |
| **E · Verdict** | [`product-verdict`](skills/product-verdict/SKILL.md) | audit kill-switch + 实际 metric | 持守 / 调整 / 杀 | 10-15 min |

---

## 何时用哪段

| 你的状态 | 该用 |
|---------|------|
| 脑子里有几个想法都模糊、obsidian 攒了一堆笔记 | **A** `product-spark` |
| 有了具体方向，不知道值不值得做 | **B** `product-audit` |
| audit 给了 amazing/refactor，要展开成 persona/JTBD/positioning | **C** `product-frame` |
| Frame 完了要写需求/PRD | **D** `product-spec` |
| 产品上线了，月度/季度回顾或 metric 触及阈值 | **E** `product-verdict` |
| 已经决定杀方向 | 回 **A** 找新方向 |
| 关键环境变化（如平台原生功能发布） | 回 **B** 重审 |

---

## 跨段一致性设计（5 个产品保证）

1. **角色切换协议统一**——所有段在 Jobs 视角段都是同一句宣告 + 退出语
2. **二元判断门**——所有段的 Jobs 检查只有"通过 / 退回"，没有"软建议"
3. **HARD GATE → 唯一下游**——避免跳段（A→B、B→C 或杀、C→D、D→实施 / E）
4. **诚实标 ⚠️ 黄灯**——不是非黑即白，黄灯通过但记账
5. **允许向上回退**——D 发现 C 错回 C、E 发现环境变回 B，不硬撑

---

## 振荡保护机制（D 段独有）

D 段是高频振荡区（你写需求时切来切去最多）。承认是常态，但加预算上限：

```
同对相邻 Phase 来回 ≥ 3 次  →  红灯，回 C 段
总跨 Phase 跳转    ≥ 5 次  →  强制 stop，回 C 段
同 Phase Jobs 退回 ≥ 3 次  →  红灯，回上一 Phase
```

振荡上限不是惩罚，是诊断信号——超过 = 上游 Frame 没成型。

---

## Sunk cost 防御（E 段独有）

```
连续 2 次 verdict = "再观察 / 持守"   →  允许，记账
第 3 次想说"再观察"                  →  禁止，强制变 "调整" 或 "杀"
```

对抗"舍不得杀方向"的本能，用历史 verdict log 做事实依据。

---

## 文件结构

```
product-flow/
├── .claude-plugin/
│   └── plugin.json
├── README.md                          ← 本文件
├── skills/
│   ├── product-spark/                 ← A 段
│   ├── product-audit/                 ← B 段
│   ├── product-frame/                 ← C 段
│   ├── product-spec/                  ← D 段
│   └── product-verdict/               ← E 段
└── docs/                              ← 5 段产物落盘
    ├── sparks/      *.md              ← A 段输出
    ├── audits/      *.md              ← B 段输出
    ├── frames/      *.md              ← C 段输出
    ├── prds/        *.md              ← D 段输出
    └── verdicts/    *.md              ← E 段输出
                     <name>-history.md ← E 段历史 index（sunk cost 防御依据）
```

---

## 借鉴关系

每段 skill 借了哪些已有 skill：

| 段 | 借自 PM-Skills | 借自 superpowers | 借自 steve-jobs-skill | 借自其他 |
|----|--------------|----------------|--------------------|---------|
| A · spark | `opportunity-solution-tree`, `jobs-to-be-done` | **`brainstorming`** 的一次一问 + 多选优先 + HARD GATE | "连点成线"心智 | `obsidian` skill 作输入源 |
| B · audit | `positioning-statement`, `jobs-to-be-done`, `pol-probe-advisor` | — | 角色扮演协议 + 6 心智模型 | — |
| C · frame | `proto-persona`, `jobs-to-be-done`, `problem-framing-canvas`, `positioning-statement`, `company-research`, `pestel-analysis` | — | 表达 DNA + 二元判断 | — |
| D · spec | `epic-hypothesis`, `lean-ux-canvas`, `user-story`, `user-story-splitting`, `prioritization-advisor`, `prd-development` | `writing-plans` 衔接 | 表达 DNA + Focus 心法 | sub-agent dispatch hints 是新设计 |
| E · verdict | `business-health-diagnostic`, `saas-revenue-growth-metrics`, `feature-investment-advisor` | — | "杀产品的勇气" + 二元判断 | `cron` skill 做时间触发 |

---

## 不替代谁

- 不替代 `superpowers/brainstorming`——那是"从 1 个想法到 1 个设计"，本套是"从 N 个想法到 1-3 个候选 → 审计 → 展开 → 落地 → 迭代"
- 不替代 `Product-Manager-Skills`——本套是把 PM-Skills 的 47 个原子 skill 编排成有方向感的流程
- 不替代 `steve-jobs-skill`——纯 Jobs 视角对话仍然切到那个 skill
- 不替代具体实施——D 段输出 PRD + sub-agent dispatch 后，**实施交给 sub-agent / 其他 agent / 用户自己**

---

## Dogfood 端到端 example

虚构产品 **TeamPulse · 会议→action 自动化** 走完完整 5 段，每段 example 演示一个典型失败-恢复模式：

| 段 | Example | 演示要点 |
|----|---------|---------|
| A · spark | [dogfood-teampulse.md](skills/product-spark/examples/dogfood-teampulse.md) | 多想法（5 条）→ Cluster 找主线 → 输出 2 候选 |
| B · audit | [dogfood-teampulse.md](skills/product-audit/examples/dogfood-teampulse.md) | refactor verdict + 3 颗螺丝（关键环节都在外人手上） |
| C · frame | [dogfood-teampulse.md](skills/product-frame/examples/dogfood-teampulse.md) | 5 件套 + Step 3 Problem 退回 1 次（写成方案被退） |
| D · spec | [dogfood-teampulse.md](skills/product-spec/examples/dogfood-teampulse.md) | 5 phase + 2 次振荡（Phase 3↔2、4↔3）+ sub-agent dispatch |
| E · verdict | [dogfood-teampulse.md](skills/product-verdict/examples/dogfood-teampulse.md) | 第 1 次 verdict 持守 + sunk cost 防御预警 |

**读完这 5 个 example 你应该能脑内跑通整个流程**——比读 5 份 SKILL.md 快得多。

---

## 触发示例

```
用户："我有几个 AI 工具想法不知道做哪个"
→ 触发 A. product-spark

用户："这个方向值不值得做？帮我审一下"
→ 触发 B. product-audit

用户："audit 给了 refactor 3 颗螺丝，frame 一下"
→ 触发 C. product-frame

用户："写需求 / 写 PRD"
→ 触发 D. product-spec

用户："上线 1 个月了，跑一次 verdict"
→ 触发 E. product-verdict
```

---

## License

MIT

---

**版本**：v0.1.0 · 2026-05-03 首发
