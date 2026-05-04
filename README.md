# Product Flow · 5 段产品开发管道

> 把"灵感 → 上线后迭代"完整切成 5 段，每段一个 skill。
> PM 框架搭骨架（Geoffrey Moore / JTBD / MITRE）+ Steve Jobs 视角做品味压测 + superpowers/brainstorming 的硬门协议。
> 解决"切来切去"的振荡 —— 5 段衔接清晰，每段唯一下游，承认振荡但限定预算。

**版本**：v0.1.0 · 2026-05-03 首发 ｜ [CHANGELOG](CHANGELOG.md) ｜ [GLOSSARY](docs/GLOSSARY.md) ｜ [WALKTHROUGH](docs/WALKTHROUGH.md) ｜ [PATTERNS](docs/PATTERNS.md)

---

## 60 秒上手

**问自己 1 个问题：你现在在哪一段？**

```
┌─ 想法很模糊，散点多 ─────────────→ 说「帮我整理一下想法」     → A · product-spark
├─ 有具体方向，不知值不值得做 ──→ 说「审一下这个方向」         → B · product-audit
├─ audit 给了判决，要展开 ─────→ 说「frame 一下」              → C · product-frame
├─ Frame 完了，要写需求 ──────→ 说「写需求 / 写 PRD」          → D · product-spec
└─ 产品上线一段时间了 ────────→ 说「跑一次 verdict」           → E · product-verdict
```

**第一次用？** 直接读 [WALKTHROUGH.md](docs/WALKTHROUGH.md)，跟着虚构产品 TeamPulse 走完一遍 5 段，比读 5 个 SKILL.md 快 5 倍。

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

## FAQ

**Q1 · 必须 5 段都跑吗？**

不必须。但跨段时要遵循"唯一下游"规则：

- 跳过 A：你已经有具体方向（不是 N 个散点）→ 直接进 B
- 跳过 B：**禁止**——这是"值不值得做"的唯一闸门，跳过 = 蒙眼 ship
- 跳过 C：你已经有 Frame Pack → 直接进 D（但通常没人这么用）
- 跳过 D：你不写 PRD 直接 ship？除非你是 1 人公司且全部脑内规划 OK
- 跳过 E：上线后不迭代？**最大的浪费**——产品 80% 价值在 metric 反馈循环里

**Q2 · audit 一直 refactor 怎么办？**

refactor 是好的——它告诉你哪 N 颗螺丝要拧。**死循环 refactor**（连续 3 次 audit 都 refactor 但拧不动）的真问题是 spark 阶段方向选错。

应对：
1. 把 audit 的 refactor 螺丝列出来对比——是不是同一颗反复拧？
2. 如果是，回 A 段重做 spark，承认这个方向**根本上有 bug**
3. 如果是不同螺丝（这次是 D1 下次是 D3）→ 真在迭代，没问题

**Q3 · 振荡 log 超阈值要回 C 段，太麻烦怎么办？**

阈值（3 / 5 / 3）就是用来防止你"硬撑"。回 C 段比硬写一份糟糕 PRD 强得多。

如果觉得阈值太严，**记录在案**：「我这次明知超阈值但选择继续，原因是 X」。下次复盘时看是不是常见模式——如果常见，可能要调阈值，但**不要为单次方便就破规则**。

**Q4 · sunk cost 防御让我没法慢慢观察，怎么处理？**

"慢慢观察"是 sunk cost 的伪装。机制不是阻止你观察，是阻止你**没观察依据地继续投入**。

如果你真在等具体信号（比如"等下个月用户量 ≥ 100"），把这个写成 metric 阈值——下次 verdict 时就用这个 metric 检查，而不是模糊"再观察"。

**Q5 · 这套和 Product-Manager-Skills 的 47 个 skill 有啥关系？**

product-flow 是 PM-Skills 的**编排层**：

- PM-Skills = 47 个原子级 skill（写 user story / 选优先级框架 / 做 SWOT 等）
- product-flow = 5 个段，每段 invoke 多个 PM-Skills 完成一个产品工作阶段

类比：PM-Skills 是函数库，product-flow 是 main()。你两者都用，但通过 product-flow 进入 PM-Skills 比直接挑 47 个里哪个用更有方向感。

**Q6 · D 段振荡和 E 段 sunk cost 是不是一回事？**

不是。它们防的是**不同方向的偏差**：

- 振荡防：**写 PRD 时**反复改 hypothesis / story / scope（前进时打转）
- sunk cost 防：**上线后**舍不得杀方向（继续投入时偏执）

两者对称——一个防进入死循环，一个防退出失败。

**Q7 · 我能在多个产品方向上并行跑这套吗？**

可以。每个方向独立产生：
- `docs/sparks/<方向>.md`
- `docs/audits/<方向>.md`
- `docs/frames/<方向>.md`
- `docs/prds/<方向>.md`
- `docs/verdicts/<方向>-YYYY-MM.md` + `<方向>-history.md`

不会冲突。但 Jobs 的 Focus 心法说："并行跑 ≥ 3 个方向 = 没在 focus"——除非你是 portfolio 投资者，不然建议同时只 deep work 1 个方向。

---

## Troubleshooting · 卡壳排错

| 症状 | 可能原因 | 应对 |
|------|---------|------|
| spark Step 4 Q1 用户答"都不对，但我也说不出" | 想法还没成型 | 退回 Step 2 capture，让用户多倒 5 条想法或先去 brainstorming |
| audit Pass 1 红字段太多（target/benefit/category 都标红）| spark 阶段 HARD GATE 没拦住 | 立即回 spark 重收敛，不要硬填 |
| audit Pass 2 Q2 用户砍不动（"都重要"） | hypothesis 不存在或太多 | 退到 Pass 1，先 reframe 核心动作 |
| audit Verdict 卡在 amazing/refactor 间反复 | 你在自我欺骗 | 用 hard rule：能给 1 句话讲清「insanely great 在哪个具体细节」就 amazing；讲不出就 refactor |
| frame Step 3 Problem 写成方案 | 这是最常见错误 | 强制 reframe："去掉解决方案描述后，问题本身是什么？" |
| frame Step 4 tagline 想不出 ≤ 8 字 | 价值主张不清楚 | 退回 Step 1 重 persona——通常是不知道为谁做 |
| spec Phase 4 MUST 砍不到 5 个以下 | 你不相信 hypothesis 的核心动作只有 1 个 | 退回 Phase 1 重写 hypothesis，列 1 个不是 3 个 |
| spec 总跳转 ≥ 5 次但每对相邻 ≤ 3 次 | Frame 还可以更准 | 黄灯通过 + 记账，下次 frame 多花时间 |
| spec 总跳转 ≥ 5 次且某对相邻 ≥ 3 次 | Frame 错了 | 红灯，必须回 C 段重做 |
| verdict 第 3 次"持守"被禁，但你真觉得没问题 | 隐藏 sunk cost | 强制选 a) 调整：找最弱的 1 个 metric 改进它 b) 杀：诚实承认到顶 |
| verdict 显示一切都 🟢 但你心里慌 | 信号不真，noise 嫌疑 | 跑 Step 2 Q1 再问一次"连续稳定吗？由单点事件驱动吗？" |

---

## 文件结构

```
product-flow/
├── .claude-plugin/
│   └── plugin.json                    ← v0.1.0 元数据
├── README.md                          ← 本文件（60 秒上手 + 5 段地图 + FAQ + 排错）
├── CHANGELOG.md                       ← 版本变更
├── skills/
│   ├── product-spark/                 ← A 段
│   │   ├── SKILL.md
│   │   ├── template.md
│   │   └── examples/dogfood-teampulse.md
│   ├── product-audit/                 ← B 段
│   ├── product-frame/                 ← C 段
│   ├── product-spec/                  ← D 段
│   └── product-verdict/               ← E 段
└── docs/                              ← 5 段产物落盘 + 项目文档
    ├── GLOSSARY.md                    ← 术语表
    ├── WALKTHROUGH.md                 ← TeamPulse 端到端叙事
    ├── PATTERNS.md                    ← 借鉴的具体 pattern 致谢
    ├── sparks/      *.md              ← A 段产物
    ├── audits/      *.md              ← B 段产物
    ├── frames/      *.md              ← C 段产物
    ├── prds/        *.md              ← D 段产物
    └── verdicts/    *.md + history.md ← E 段产物 + sunk cost 防御依据
```

---

## 借鉴关系（概览）

详见 [docs/PATTERNS.md](docs/PATTERNS.md)。

| 段 | 借自 PM-Skills | 借自 superpowers | 借自 steve-jobs-skill | 借自其他 |
|----|--------------|----------------|--------------------|---------|
| A · spark | `opportunity-solution-tree`, `jobs-to-be-done` | **`brainstorming`** 的一次一问 + 多选优先 + HARD GATE | "连点成线"心智 | `obsidian` skill 作输入源 |
| B · audit | `positioning-statement`, `jobs-to-be-done`, `pol-probe-advisor` | — | 角色扮演协议 + 6 心智模型 | — |
| C · frame | `proto-persona`, `jobs-to-be-done`, `problem-framing-canvas`, `positioning-statement`, `company-research`, `pestel-analysis` | `<HARD-GATE>` XML + self-review 4 项 + visual companion | 表达 DNA + 二元判断 | — |
| D · spec | `epic-hypothesis`, `lean-ux-canvas`, `user-story`, `user-story-splitting`, `prioritization-advisor`, `prd-development` | `writing-plans` 衔接 + `<HARD-GATE>` XML + self-review + visual companion | 表达 DNA + Focus 心法 | sub-agent dispatch hints 是新设计 |
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

或者读 [docs/WALKTHROUGH.md](docs/WALKTHROUGH.md)——把 5 个 example 串成 1 篇连续叙事。

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

**版本**：v0.1.0 · 2026-05-03 首发 ｜ 维护者：[your name]
