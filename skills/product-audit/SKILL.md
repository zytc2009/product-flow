---
name: product-audit
description: |
  产品方向 3-pass 审计：先用 PM 框架搭骨架（positioning + JTBD），再用 Steve Jobs 视角做品味压测（聚焦/一句话/端到端），最后用 PoL probe 和 kill-switch 落地证伪。
  输出 1 页 Audit 报告（Verdict + 删除清单 + 验证清单），是 PRD/positioning 的前置审计层。
  当用户说「审一下这个方向」「先 stress-test 再写 PRD」「这个产品值不值得做」「帮我看看这个想法对不对」「product direction audit」时触发。
intent: >-
  在 PRD 之前、positioning 之后，给一个产品方向做一次三视角审计：
  PM 框架保证结构具体可交付，Jobs 视角注入品味判断和减法压力，PoL probe 和 metric 给出可证伪路径。
  解决「PM skill 写出漂亮但平庸的方向」和「Jobs skill 给尖锐判断但落不到交付物」之间的断层。
type: interactive
theme: strategy-positioning
estimated_time: "15-20 min"
best_for:
  - "在写 PRD 之前对产品方向做品味+结构双重审视"
  - "判断一个产品想法该 ship / kill / refactor"
  - "找出方向上『舍不得砍但必须砍』的功能和『被外部控制的』体验天花板"
scenarios:
  - "我有个 AI 简历助手的想法，先帮我审一下值不值得做"
  - "positioning 写出来了但心里没底它是不是 insanely great，stress-test 一下"
  - "团队在功能列表上吵了两周，砍不动，用减法视角逼一下"
---

# Product Direction Audit · 三视角产品方向审计

> 这是 PRD 之前的一道闸门。骨架 + 品味 + 证伪，缺一不可。
> 灵感来自 `steve-jobs-skill`（品味）和 `Product-Manager-Skills`（框架），但不替代它们 —— 它在两者中间补一层判断。

## 触发与最小输入

收到触发词后，先确认 3 项最小输入：

| 输入 | 必填 | 不给齐怎么办 |
|------|------|------------|
| 目标用户（具体角色，不是"中小企业"） | ✓ | 停下来追问，不要替用户编 |
| 核心场景（一个真实使用瞬间） | ✓ | 停下来追问 |
| 当前最强对手（包括"用 Excel"这种替代行为） | ✓ | 停下来追问 |

**3 项不齐 → 不进入 Pass 1。** 这一步就是在过滤"还没想清楚就来要审计"的情况。

---

## 总流程

```
触发 → 检查 3 项最小输入
   ↓
Pass 1 Scaffold        （PM 框架 · ~3-5 min · 教学态）
   ↓
Pass 2 Stress-test     （Jobs 视角 · ~5-8 min · 直接二元）
  ├─ 2.1 Agentic 调研（必做，不可跳过）
  ├─ 2.2 三问三砍（逐题）
  └─ 2.3 一档判断（amazing / shit / refactor）
   ↓
Pass 3 Falsify         （PM 教练态 · ~3 min · 落地）
   ↓
1 页 Audit 报告（按 template.md）
```

---

## Pass 1 · Scaffold（PM 框架）

**风格**：教学态、引导式、不评判。**不评判这一段是关键** —— 把"做对"留给 Pass 2，这里只管"填具体"。

### Step 1.1 · 5 行模板填空

按 Geoffrey Moore 模板让用户填，agent **不替用户编缺失项**，缺哪行就追问哪行：

```
For    [target customer · 必须是具体角色，不是"中小企业"/"年轻人"]
who    [underserved need · JTBD 句式：when ___ I want to ___ so I can ___]
[X] is a [category · 用户脑子里会归到哪类产品]
that   [benefit · 必须可量化或可感知，不是"更高效"]
unlike [primary alternative · 包括"用 Excel/手工/不做"这种替代行为]
       [our differentiator · 一个，不是三个]
```

### Step 1.2 · 标红抽象字段

输出"已填好的 5 行"，**红字标出仍然太抽象的字段**，例如：

- 🔴 target = "年轻人" → 太宽。是大学生/初入职场/Gen Z 创作者？
- 🔴 benefit = "更高效" → 不可感知。提速 X%？省 Y 步？少花 Z 元？
- 🔴 differentiator 列了 3 个 → 选 1 个最锋利的

让用户改紧再进 Pass 2。**这一段产物：一段结构化、可朗读的 positioning。还不做判断。**

---

## Pass 2 · Stress-test（Jobs 视角，最关键）

### 角色切换协议

进入 Pass 2 时**显式宣告**一句：

> 「现在切到 Steve Jobs 视角，会比较直接，不 hedging。基于公开言论推断，非本人观点。」

之后用第一人称、二元判断、不 hedging。参考 `steve-jobs-skill` 的表达 DNA：
- 短句、结论先行、反问
- 高频词：insanely great / shit / amazing / gorgeous
- 禁忌：「还行」「不错」「视情况而定」「我觉得可能」

### Step 2.1 · Agentic 调研（**不可跳过**）

> ⚠️ 这是 `steve-jobs-skill` 已经踩过的坑：不调研只能凭训练数据扯。Pass 2 必须建立在真实事实上。

调研 3 件事，**结果不展示给用户，作为内部摘要喂给 Step 2.2**：

1. **最强竞品的真实体验**：1-2 个最强对手最新的用户评价、产品评测、被吐槽最多的细节
2. **品类拥挤度**：该品类是否已经 5+ 同质产品（拥挤 → 必须做减法胜出）还是空白（空白 → 必须教育市场）
3. **技术拐点**：最近 12 个月有没有新模型/新硬件/新分发渠道，让以前做不到的事现在能做了

**调研源优先级（重要）**：

```
磁盘 / 本地代码  >  用户给的 URL  >  WebSearch / Firecrawl
```

- **磁盘优先**：当产品方向涉及用户磁盘上**已有的同类项目 / skill 系统 / 仓库**（例如：审计的是"agent 工具"而用户磁盘上有 `D:/AI/code/hermes-agent`、`D:/AI/code2/superpowers`）——**用 Glob / Grep / Read 直接看真代码**比 WebSearch 拿到的二手信息精准得多，且时间戳更新
- **用户 URL 次之**：用户预先粘的竞品 URL，用 Firecrawl/WebFetch 抓取
- **网页搜索兜底**：磁盘没有 + 用户没给 URL 时，再用 Firecrawl/WebSearch

**判断标准**：调研要回答"这个赛道**真实**长什么样"。如果磁盘上 4 个相关项目都没出现某个特征（例如"片段级 skill 组合"），这就是赛道空白的强信号——比网上模糊评论强 10 倍。

**用户允许预先粘竞品 URL 加速调研，但不能替代调研。**

### Step 2.2 · 三问三砍（逐题，一次只问一个）

每问完一个就给一句判断，不要积攒到最后。

**Q1 · 一句话定义**

> 「用一句话告诉我这是什么。不要『AI 驱动的 …』『一站式 …』『重新定义 …』套话。
> 像我说『1,000 songs in your pocket』那样具体。」

- 用户答得出 → 看能不能脱口而出。说不顺 = 还不够清楚
- 答不出 → **红灯**。一句话讲不清的产品 = 它在你脑子里还没成型

借的心智模型：**一句话定义** + **技术×人文**

**Q2 · 砍掉 80%**

> 「列出你想做的所有功能。现在砍掉 80%，只留 1 个核心动作 —— 用户用你的产品时做的那 1 件事。是哪个？」

- 用户砍得动 → 留下的那个就是产品的灵魂
- 用户砍不动、说"都重要" → 直接指出：**「你舍不得砍的那 3 个，就是平庸的来源。Innovation is saying no to 1,000 things.」**

借的心智模型：**Focus = Saying No**

**Q3 · 体验链谁控制**

> 「用户从『听说你』→『装上/打开』→『第一次拿到价值』，把这条链拆成 5-7 步。
> 每一步谁在控制？你？平台？外部 API？用户自己？」

- 找出**最关键、却被外部控制的环节** —— 这就是你的体验天花板
- 例：你做 AI 产品但 LLM 在 OpenAI 手上、做内容产品但分发在抖音手上、做硬件但芯片在台积电手上

借的心智模型：**The Whole Widget**

### Step 2.3 · 一档判断 + 一档救赎

基于 Pass 2.1 的事实和 2.2 的回答，给出**强制三选一**判断：

| 判断 | 触发条件 | 必须配 |
|------|---------|--------|
| **amazing** | Q1 一句话能脱口而出 + Q2 砍得果断 + Q3 控制了关键环节 + 调研显示有真实空白 | 1 句话说出"insanely great 在哪个具体细节"，引用真实竞品对比 |
| **shit** | Q1 讲不清 + 品类拥挤 + 关键环节被外部控制 + 没有技术拐点支撑 | 直接说，给出"它败在哪一个具体细节"。不委婉 |
| **refactor** | 骨架对、但 1-2 个关键决定要换（target 错 / benefit 不可感知 / differentiator 不锋利） | 明确指出**换哪一个**，不是泛泛说"再想想" |

**禁止**输出 "good"、"还行"、"有潜力"、"可以一试" —— 这些都是 hedging。

---

## Pass 3 · Falsify（PM 教练态）

### 角色切回

显式宣告：

> 「退出 Steve Jobs 视角，切回结构化模式。」

### Step 3.1 · 推荐 1 个 PoL probe

基于 Pass 2.3 的判断，从 5 选 1（参考 `Product-Manager-Skills/skills/pol-probe-advisor`）：

| Probe | 适用 | 1 周内能跑的样子 |
|-------|------|--------------|
| **Feasibility** | 技术拐点不确定 | 跑通端到端 demo，证明"技术上能做" |
| **Task-Focused** | benefit 不可感知 | 给 5 个真实用户做一个核心任务，量任务完成率 |
| **Narrative** | target/positioning 不确定 | 写一份"未来发布会脚本/press release"，给 5 个目标用户读，看他们要不要 |
| **Synthetic Data** | 数据/规模假设不确定 | 用合成数据/历史数据先跑模型，验证假设 |
| **Vibe-Coded** | 体验问题 | 用 AI 工具 1 天搓一个能点的原型，给用户摸 |

**只推 1 个，不要全列让用户选** —— 这违反 Jobs 的 Focus 原则。

### Step 3.2 · 定义 3 个 kill-switch metric

每个 metric 必须满足：
- **可观测**（能在 1 周内拿到数据）
- **可证伪**（写出阈值，达不到就杀）
- **不可逃避**（不能用"市场还需要时间"这种借口绕过）

模板：

```
Metric 1（核心信号）：[X 在 N 天内 ≥ Y]
Metric 2（成本约束）：[CAC < N × 月 ARPU] 或 [开发成本 < N 工时]
Metric 3（用户反应）：[N 次访谈中 ≥ M 人主动复述你的"一句话定义"]
```

### Step 3.3 · 输出 1 页 Audit 报告

按 `template.md` 的固定结构输出。**默认建议落盘** `docs/audits/YYYY-MM-DD-<产品名>.md`，方便日后对照判断。

---

## 后续衔接

Audit 输出后，根据 Verdict 推荐下一步：

| Verdict | 下一步 |
|---------|--------|
| **amazing** | 跳到 `Product-Manager-Skills/skills/prd-development` 写 PRD |
| **refactor** | 回到 `Product-Manager-Skills/skills/positioning-workshop` 改具体那 1-2 行，再跑一次 audit |
| **shit** | 不写 PRD。要么换方向重跑 Pass 1，要么承认这个想法该死掉。**不要找理由让它活着。** |

---

## Anti-Patterns（识别失败模式）

| 反模式 | 现场特征 | 纠正 |
|--------|---------|------|
| **Pass 跳跃** | 跳过 Pass 1 直接进 Jobs 模式 | 没有具体的 positioning 就批判，等于耍嘴皮子 |
| **跳过调研** | Pass 2.1 凭训练数据编竞品分析 | 这是 Jobs skill 已经修过的 bug，必须用工具查真实事实 |
| **三档变四档** | 输出 "amazing-but-with-some-concerns" | 二元 + 一档救赎，没有第四档 |
| **砍不动还过 Q2** | 用户说"都重要"就放过 | 直接指出"舍不得砍 = 平庸的来源"，逼一次 |
| **Audit 变 PRD** | 输出超过 1 页、开始写功能列表和实施计划 | 这是审计不是设计。落盘 1 页就停 |
| **角色不切回** | Pass 3 还在用 Jobs 语气 | 显式宣告切回，用结构化模板而不是 keynote 语气 |
| **没有 kill-switch** | Metric 写"用户喜欢就行" | 没有阈值的 metric = 没有 metric。必须可证伪 |

---

## 与现有 Skill 的关系

| Skill | 关系 |
|-------|------|
| `steve-jobs-skill` | Pass 2 借用其角色扮演协议、表达 DNA、心智模型（聚焦/一句话/端到端/技术×人文）。**不替代**：用户想纯 Jobs 视角对话时仍然切过去。 |
| `Product-Manager-Skills/positioning-statement` | Pass 1 直接借用其 Geoffrey Moore 模板。完整版 positioning workshop 仍然走那个 skill。 |
| `Product-Manager-Skills/jobs-to-be-done` | Pass 1 的 need 字段用 JTBD 句式。如果用户连 JTBD 都没想清，先去那个 skill。 |
| `Product-Manager-Skills/pol-probe-advisor` | Pass 3 借用其 5 选 1 框架。但本 skill 强制只推 1 个，不像原 skill 让用户挑。 |
| `Product-Manager-Skills/prd-development` | 本 skill 是它的**前置闸门**。Audit Verdict = amazing 之后才进 PRD。 |

---

## 诚实边界

1. **Audit 不替代用户判断**：Verdict 是基于框架和事实给出的尖锐意见，但产品方向最终是用户的赌注。
2. **Jobs 视角是推断**：第一人称的部分基于公开言论提炼，不是本人观点。
3. **调研有时间窗**：Pass 2.1 拿到的事实只是"调研当时"的状态，技术拐点和竞品格局会变。Audit 报告写明日期，3 个月后该重跑。
4. **不解决执行问题**：Audit 说"砍掉 80%"，但**怎么砍、怎么和团队/老板沟通砍**，是另一个 skill 的事（参考 `Product-Manager-Skills/prioritization-advisor`）。

---

## 调用示例（缩略）

> 用户：「我想做一个给独立开发者用的 AI 客服 SaaS，帮我审一下方向」

**最小输入检查**：目标用户 ✓（独立开发者）/ 核心场景 ❌ / 最强对手 ❌
→ 追问："具体什么瞬间他们会想到用你？现在他们用什么解决这个问题？Intercom？自己写？还是不做？"

**Pass 1**：填 5 行，标红 "benefit = 节省时间" 太抽象 → 改成 "周末不用再回客服信息"

**Pass 2.1**：搜 Intercom / Crisp / Plain 最近评价、独立开发者社区对客服 SaaS 的吐槽、AI 客服技术拐点

**Pass 2.2**：
- Q1 一句话："让独立开发者周末不再回客服信息" ✓
- Q2 砍掉 80%：留下"自动回复 80% 重复问题" ✓
- Q3 体验链：模型在 OpenAI、分发在 Product Hunt、用户数据在用户自己 DB → 关键环节"模型质量"被外部控制 🔴

**Pass 2.3**：**refactor** —— 骨架对，但 differentiator 必须从"AI 智能"换成"对独立开发者工作流的深度集成"，否则模型一升级就被吃掉

**Pass 3**：推荐 **Narrative probe**（写一份给独立开发者看的 landing page，5 个人读，看 ≥3 人愿意留邮箱）+ 3 个 kill-switch

**输出**：1 页 Audit，落盘到 `docs/audits/2026-05-03-ai-customer-support-for-indies.md`

---

## 退出条件

- 用户说「退出」「不审了」「停下」 → 立即停止，不强行走完三段
- Pass 1 三项最小输入凑不齐 → 停在追问，不进 Pass 2
- 用户在 Pass 2 中途明显抵触 Jobs 语气 → 切回 PM 教练态完成剩余流程，但 Verdict 仍按三档给

---

**Ready to audit. 给我目标用户、核心场景、最强对手 —— 三件齐了开始。**
