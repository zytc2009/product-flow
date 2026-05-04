# Product Direction Audit — [一句话产品定义]

> 日期：YYYY-MM-DD ｜ 审计人：[用户/团队] ｜ 调研时间窗：YYYY-MM-DD ~ YYYY-MM-DD

## Verdict

**[ amazing / shit / refactor ] —— 选一个，不要 hedging**

[一句话说出为什么是这个判断，引用 Pass 2 中最关键的那一条事实/缺陷/亮点]

---

## Positioning（来自 Pass 1）

For    [target customer · 具体角色]
who    [underserved need · JTBD 句式：when ___ I want to ___ so I can ___]
[X] is a [category]
that   [benefit · 可量化或可感知]
unlike [primary alternative]
       [our differentiator · 一个]

---

## What I'd cut（来自 Pass 2 · Q2）

砍掉的功能 / 该砍但用户舍不得砍的功能：

- **[功能 1]** —— 理由：[为什么它不该留]
- **[功能 2]** —— 理由：[为什么它不该留]
- **[功能 3]** —— 理由：[为什么它不该留]

留下的核心动作：**[1 个，不是 3 个]**

---

## Weakest link in the chain（来自 Pass 2 · Q3）

体验链拆解：

1. [步骤 1] —— 谁控制：[你 / 平台 / 外部 API / 用户]
2. [步骤 2] —— 谁控制：...
3. [步骤 3] —— 谁控制：...
4. [步骤 4] —— 谁控制：...
5. [步骤 5] —— 谁控制：...

🔴 **被外部控制的关键环节**：[哪一步 + 谁在控制]
**这是体验天花板。** 应对方案：[绕开 / 接受 / 垂直整合 / 换方向]

---

## Cheapest falsification（来自 Pass 3）

**Probe type**：[Feasibility / Task-Focused / Narrative / Synthetic Data / Vibe-Coded] · 选 1 个

**1-week plan**：
- Day 1-2：[准备什么]
- Day 3-5：[跑什么]
- Day 6-7：[拿什么数据 + 谁来评估]

预期成本：[多少工时 / 多少钱 / 涉及几个人]

---

## Kill-switches（达不到就停）

| # | Metric | 阈值 | 测量方式 |
|---|--------|------|---------|
| 1 | [核心信号 · 例：N 天内主动复述率] | ≥ X | [谁/怎么测] |
| 2 | [成本约束 · 例：CAC / 开发工时] | ≤ Y | [谁/怎么测] |
| 3 | [用户反应 · 例：访谈中主动提及竞品对比] | ≥ Z | [谁/怎么测] |

**任何一个达不到 → 杀掉这个方向，不要找借口让它活着。**

---

## Next step

- [ ] **若 amazing** → 进入 `prd-development` 写 PRD，把上面的 5 行 positioning + 留下的核心动作作为 PRD 的 Vision 段落
- [ ] **若 refactor** → 回到 `positioning-workshop` 改 [具体哪一行]，再跑一次 audit
- [ ] **若 shit** → 复盘 Pass 1 的 5 行哪里最先开始虚，记到 `docs/audits/_lessons.md`，换方向

---

## 元数据

- 调研来源：[列出 Pass 2.1 用过的关键 URL/数据]
- 角色切换：本次 Audit 在 Pass 2 进入了 Steve Jobs 视角（基于公开言论推断，非本人观点）
- 重审时间：建议 [3 个月后 / 关键 metric 拿到后] 重跑一次
