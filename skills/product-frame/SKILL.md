---
name: product-frame
description: |
  把 product-audit 的 Verdict 展开成可被 D 段消费的 Frame 包：persona + JTBD + problem + positioning + differentiation。
  5 步循环，每步 PM 框架搭骨架 + Jobs 视角做"够具体吗"二元检查，不达标退回上一步。
  当用户说「展开方向」「把 audit 后的方向变成需求前的准备」「写 PRD 之前先 frame 一遍」「frame 这个产品」「把 audit 报告变成 positioning」时触发。
intent: >-
  五段产品流程的 C 段。在 Audit 之后、Spec 之前，把"该做什么"展开成"给谁、解决什么问题、怎么定位、为什么和别人不同"——
  让 D 段直接拿这个 Frame 包写需求，而不是面对一个抽象 Verdict。
type: interactive
theme: positioning-framing
estimated_time: "20-30 min"
best_for:
  - "Audit Verdict = amazing/refactor 后展开为完整产品 frame"
  - "把抽象方向变成 persona / JTBD / problem / positioning / differentiation 5 件套"
  - "PRD 之前完成 user/problem/market 三角的具体化"
scenarios:
  - "audit 给了 refactor，3 颗螺丝拧完了，现在要把方向 frame 成可写需求的状态"
  - "我有了 positioning 草稿，想把 persona 和 differentiation 也补齐"
  - "把 docs/audits/<name>.md 展开成完整产品 frame"
---

# Product Frame Loop · 5 步产品框定循环

> 这是产品流程的 C 段。它在 Audit 之后、Spec 之前，把方向**展开**成 5 件套：
> persona + JTBD + problem + positioning + differentiation。
> 每步骨架靠 PM 框架，每步检查靠 Jobs 视角——**不达标退回**。

## 触发与定位

| 用户的状态 | 触发本 skill | 不触发本 skill |
|-----------|------------|--------------|
| 刚跑完 Audit，Verdict = amazing 或 refactor | ✓ | |
| 已有 positioning 草稿但缺 persona/differentiation | ✓ | |
| audit 报告在手，要写 PRD 但还没准备好 | ✓ | |
| 还没决定方向值不值得做 | | → 先走 `product-audit` |
| 已经完成 5 件套，要写需求 | | → 直接走 `product-spec`（D 段） |

**输入**：`docs/audits/<name>.md`（或 audit 切入语 + Verdict）
**输出**：`docs/frames/<name>.md`——5 件套 Frame 包，可直接喂给 D 段

---

## 总流程

```
入口：audit 报告 + Verdict
   ↓
Step 1 · Persona Frame
  ├─ PM 骨架：proto-persona 模板填空
  └─ Jobs 检查：脱口而出"我服务的人是 X" — 不通过退回
   ↓
Step 2 · JTBD Frame
  ├─ PM 骨架：when/I want/so I can
  └─ Jobs 检查：真痛点还是空想 — 不通过退回
   ↓
Step 3 · Problem Frame
  ├─ PM 骨架：look-inward / look-outward / reframe
  └─ Jobs 检查：这是问题不是方案 — 不通过退回
   ↓
Step 4 · Positioning Frame
  ├─ PM 骨架：Geoffrey Moore 5 行（升级版，比 audit Pass 1 更具体）
  └─ Jobs 检查：1,000-songs 级吗 — 不通过退回
   ↓
Step 5 · Differentiation Frame
  ├─ PM 骨架：真实对手坐标轴
  └─ Jobs 检查：能被对手 1 周复制吗 — 不通过退回
   ↓
HARD GATE：5 件套都过 Jobs 检查 → D 段 product-spec
```

每个 Step 都是 mini 三段：**PM 教练态填空 → Jobs 视角戳 → 用户改紧或退回**。Jobs 视角和 audit 用同一套角色切换协议（首次显式宣告，结尾切回）。

---

## Visual Companion（可选 · 借自 superpowers/brainstorming）

如果即将到来的问题涉及视觉内容（persona avatar / customer journey map / Step 5 的 2×2 差异化象限图 / 用户使用瞬间的场景图），agent **可以** offer 一个 visual companion。

**协议**：
- offer 必须是**独立消息**（不和 Step 1 的提问混在一起）
- 用户可以同意 / 拒绝 / 暂时不需要
- 同意后 ≠ 每个问题都用 visual——**逐题判断**：用户读还是看更容易理解？
- 概念性问题（如 "JTBD 的 so I can 该写什么"）→ 文字
- 视觉性问题（如 "Step 5 你的 differentiator 在 2×2 中位置"）→ visual

**offer 模板**：

> "Step 1 的 persona 和 Step 5 的差异化象限可能用图说明更直观。可以给你做几张 ASCII / 简单图表，你看哪个 Step 用得上？或者全程文字也 OK——你定。"

---

---

## 入口检查

读取 audit 报告，提取 4 项必备：

| 必备 | 来自 audit 哪一段 | 缺了怎么办 |
|------|-----------------|-----------|
| Verdict（amazing/refactor） | Audit Verdict 段 | 缺 = 没跑 audit，回 B 段 |
| 留下的核心动作（"砍 80% 后留 1 个"） | Audit "What I'd cut" 段 | 缺 = audit Pass 2 没做完，回 B 段 |
| 关键 decision（如 refactor 的 N 颗螺丝） | Audit "必须换的 decision" 段 | 缺则按 amazing 处理 |
| weakest link（外部控制环节） | Audit "Weakest link in the chain" 段 | 缺 = audit Pass 2 Q3 没做完，回 B 段 |

4 项不齐 → **不进 Step 1**，告诉用户回 B 段补完。

---

## Step 1 · Persona Frame

### 1.1 PM 骨架（教练态）

借 `Product-Manager-Skills/proto-persona`，但**精简**——不做完整 persona，只做 5 个字段：

```
姓名/标签：[一个具体的人或角色，例：Alex，36 岁独立 AI 工具开发者]

一天里的 1 个真实瞬间：
[他什么时候会想到你的产品？给一个具体场景，不是"日常使用"]

他现在的工具栈：
[他现在用什么 5-10 个工具？其中哪些和你的产品相关？]

他在乎什么：
[3 件事，按优先级排]

他抗拒什么：
[2 件你产品要 onboard 他时必须避免的事]
```

**禁止**：「30-40 岁专业人士」「重视效率」「希望简单易用」这类放之四海皆准的词——直接退回。

### 1.2 Jobs 视角检查

> 切到 Jobs 视角（首次进入，显式宣告）：
> 「现在切到 Steve Jobs 视角做检查。基于公开言论推断，非本人观点。」

**检查问题**：「不看你的 PM 表格，闭眼脱口而出告诉我，你服务的人是谁？一句话。」

判断：
- **能脱口而出**（"我服务的是 Alex 这种独立 AI 工具开发者，他每周末折腾自己的 Claude Code 配置"）→ ✅ 进 Step 2
- **支吾、必须看表格才能说**（"嗯，是…那种…重视效率的开发者"）→ ❌ 退回 1.1 重填
- **说出来之后自己觉得"这听着不对"** → ❌ 退回，承认 persona 还没成型

**Jobs 戳法**：「If you can't describe them in one breath, you don't know who they are. And if you don't know who they are, every product decision will be a coin flip.」

---

## Step 2 · JTBD Frame

### 2.1 PM 骨架（教练态）

借 `Product-Manager-Skills/jobs-to-be-done`，用标准 JTBD 句式：

```
When  [触发情境，要具体到时间/地点/任务]
I want to  [想做的事，具体动作不是抽象目标]
So I can  [更深层的目标，1 句话]
```

每个产品方向通常有 **1 个主 JTBD + 1-2 个相关 JTBD**——主的写第 1 条，相关的写后两条。

**反例**：
- ❌ "When I work, I want to be efficient, so I can save time"（全是抽象词）
- ✅ "When I 深夜调试自己 agent 工作流, I want to 调用 superpowers 的 brainstorm 但跳过 6/9 checklist 项, so I can 让 sub-agent 接手不被流程绑架"（具体到瞬间和动作）

### 2.2 Jobs 视角检查

**检查问题**：「这个 JTBD 是真痛点吗？还是你脑补的？给我证据——你或你的目标用户**真的**经历过这个瞬间多少次？」

判断：
- **能给出 ≥ 3 次真实发生的瞬间**（自己 / 朋友 / 社区抱怨 / GitHub issue）→ ✅ 进 Step 3
- **只能说"这个痛点应该普遍存在"** → ❌ 退回。"应该" = "我不知道"
- **能说出但只发生过 1 次** → ⚠️ 黄灯。这可能是边缘场景而不是核心 JTBD

**Jobs 戳法**：「'Should be a problem' is what consultants say. 'I felt it 50 times last month' is what founders say. Which one are you?」

---

## Step 3 · Problem Frame

### 3.1 PM 骨架（教练态）

借 `Product-Manager-Skills/problem-framing-canvas`（MITRE 三视角），但**精简**：

```
LOOK INWARD（往内看）
[这个问题的根源在用户内部——他的工作方式 / 心智模型 / 限制，是什么？]

LOOK OUTWARD（往外看）
[问题是被外部因素塑造的——市场、技术、生态、组织约束，是哪些？]

REFRAME（重新框定）
[基于上面两视角，把问题用一句新的话重述。这句话应该让人"啊"一下]
```

REFRAME 这一步是关键——它把"我以为是 A 问题"变成"原来真问题是 B"。

### 3.2 Jobs 视角检查

**检查问题**：「读你的 LOOK INWARD 和 LOOK OUTWARD——这是**问题描述**还是**方案描述**？」

判断（极常见错误）：
- **是问题**（"用户被多步 skill 流程绑架"）→ ✅
- **是方案描述**（"用户需要片段级 skill 组合工具"）→ ❌ 退回。**方案不是问题，需求不是痛点**
- **REFRAME 后没有"啊"的瞬间**（"经过分析，问题是 …"——只是把原话重写）→ ⚠️ 退回，逼真正的视角翻转

**Jobs 戳法**：「If your problem statement contains the solution, you've already short-circuited your own thinking. Go back. Describe the pain without proposing the cure.」

---

## Step 4 · Positioning Frame

### 4.1 PM 骨架（教练态）

借 `Product-Manager-Skills/positioning-statement` 的 Geoffrey Moore 5 行，但**这是 Audit Pass 1 的升级版**——必须比 audit 时更具体：

```
For    [target customer · 具体到一个角色名 + 1 句使用瞬间]
who    [JTBD 句式，从 Step 2 直接搬]
[X] is a [category · 用户脑子里会归到哪类已知产品 · 必须给类比]
that   [benefit · 必须可量化或可感知到具体动作]
unlike [primary alternative · 真名 + URL · 包括"用 Excel/手工"]
       [our differentiator · 一个 · 用户能体感的瞬间]
```

**5 行间的连贯性检查**（PM 教练态自检）：
- target 和 JTBD 是同一个人吗？
- category 类比是 target 真的会用的产品吗？
- differentiator 真的解决了 JTBD 中的痛点吗？

### 4.2 Jobs 视角检查

**检查问题**：「读你的 5 行——给我**一句**比这 5 行更短的 tagline，**不超过 8 个字**，听到的人秒懂这是什么。」

判断：
- **能给出 1,000-songs 级**（"积木式 skill"/"编 skill 不抄 skill"）→ ✅ 进 Step 5
- **只能给字典词**（"skill 定制"/"skill 个性化"）→ ❌ 退回 4.1 重做
- **必须给两句话** → ❌ 退回。"必须 2 句 = 没想清楚 1 句"
- **能给但是借形不借义**（如本 audit 实例里的"skill 量体裁衣"）→ ⚠️ 黄灯，记下"3 个月内换"，本轮通过

**Jobs 戳法**：「Pretty good is the enemy of insanely great. If your tagline is 80%, ship it but mark it for replacement. Don't lock yourself into mediocrity by refusing to admit it's mediocrity.」

---

## Step 5 · Differentiation Frame

### 5.1 PM 骨架（教练态）

借 `Product-Manager-Skills/company-research`，画一张**真实对手坐标轴**：

```
对手 1（真名 + URL）：
- 他们对 [JTBD] 的解法：
- 他们的弱点（具体到 UX/价格/接入门槛）：
- 你比他们强在：

对手 2（真名 + URL）：
- 同上 3 项

对手 3（手工 / 现状 / 替代行为）：
- 用户不用产品时怎么解决：
- 这个"不解决"为什么持续存在：
- 你给他们的 switching cost 是什么：

你的 differentiator 在两轴上的位置：
[画一个 2×2 或一句话定位，例：
 "在 X 轴（接入门槛低）和 Y 轴（跨框架）上，我占据右上角空白区"]
```

### 5.2 Jobs 视角检查

**检查问题**：「你的 differentiator——对手能在 1 周内复制吗？」

判断：
- **不能（有结构性壁垒：网络效应/数据/分发/专利/品牌/品味）** → ✅ 进 HARD GATE
- **能（功能 / UI / 命名 / 营销话术）** → ⚠️ 红灯。**功能差异化 = 没有差异化**——任何对手 1 周做出来
- **暂时不能但 6 个月内会被复制** → ⚠️ 黄灯。要计划"复制到来时的下一招"

**Jobs 戳法**：「Anyone can copy a feature in a week. They can't copy taste, can't copy 30 years of your customer relationship, can't copy the obsession with cabinet backs. Differentiation is what survives a knockoff—everything else is decoration.」

**如果是黄灯/红灯**：
- **不强迫退回**（差异化的发现往往是迭代的）
- **明确写在 Frame 包的 Risks 段**——这是 D 段写需求时必须考虑的护城河问题

---

## Self-review · 4 项扫描（HARD GATE 之前）

5 步跑完之后、HARD GATE 检查之前，agent 用"新眼光"扫一遍 Frame 包草稿，逼自己找以下 4 类毛病。**这是 agent 自检，不需要用户参与**——就地修，无需重新评审。

| 扫描项 | 怎么检 | 不通过怎么办 |
|--------|------|-----------|
| **placeholder 扫描** | 任何 "TBD" / "TODO" / "待定" / "暂时填" / "[xxx]"未填？ | 就地填上，未填就退回相应 Step |
| **内部矛盾** | 5 件套之间是否有冲突？例：persona = "独立开发者" 但 positioning 写 "for 团队" | 找出矛盾，回相应 Step 拉平 |
| **scope 检查** | Frame 包是否聚焦 1 个产品方向？还是混进了 2 个不同方向？ | scope 太宽就拆分，本次只 frame 1 个 |
| **歧义检查** | 任何句子可以被解读成 2 种意思吗？例："让 EM 更高效"——"效率"指什么？ | 二选一明确写出，不留双向解读 |

修完毛病再进 HARD GATE。

---

## HARD GATE · 进 D 段的入门检查

<HARD-GATE>
5 步 + self-review 都跑完后必须自检。**任何一项不通过，禁止输出 Frame 包**——必须退回相应 Step 重做。

| 检查项 | 不通过怎么办 |
|--------|-----------|
| Self-review 4 项扫描全部通过 | 退回扫描 |
| Step 1 Jobs 检查通过（脱口而出 persona） | 退回 Step 1 |
| Step 2 Jobs 检查通过（真痛点 ≥ 3 次证据） | 退回 Step 2 |
| Step 3 Jobs 检查通过（是问题不是方案） | 退回 Step 3 |
| Step 4 Jobs 检查通过（1,000-songs 级 tagline 或承认 80 分待换） | 退回 Step 4 |
| Step 5 差异化非纯功能层 | 不强制退回，但写进 Risks |
| 5 件套之间逻辑连贯（persona ↔ JTBD ↔ problem ↔ positioning ↔ differentiation） | 任一处断开 → 退回相应 Step |

**不允许的事**：
- 不允许跳过 self-review 4 项扫描直接进 HARD GATE
- 不允许 placeholder 留在 Frame 包里
- 不允许在没有用户最终确认前把 Frame 包当成 D 段输入

**全部通过**才能输出 Frame 包。
</HARD-GATE>

---

## 输出 Frame 包（落盘）

按 `template.md` 输出到：

```
docs/frames/YYYY-MM-DD-<产品名>.md
```

引用上游 audit 报告路径，方便追溯。

---

## 角色切换协议

整体协议和 audit 一致：

- **进入 Step 1 第 1 次需要 Jobs 检查时**显式宣告（"现在切到 Steve Jobs 视角做检查"）
- **每个 Step 内部**：PM 教练态填空 → Jobs 视角戳 → 切回 PM 教练态修改
- **Step 之间**：保持 PM 教练态主线，Jobs 只在每步 .2 检查段出现
- **HARD GATE 通过后**显式切回（"退出 Steve Jobs 视角，Frame 完成"）

---

## Anti-Patterns

| 反模式 | 现场特征 | 纠正 |
|--------|---------|------|
| **跳步** | 跳过 Persona 直接做 Positioning | persona 没有则 positioning 的 target 永远抽象 |
| **PM 检查通过即放行** | PM 教练态自检过了就进下一步，没让 Jobs 戳 | Jobs 检查是必须的二元门，不能省 |
| **Jobs 检查变成软建议** | "这是个 nice to fix"——但放行了 | Jobs 检查只有通过/退回，没有"建议改进" |
| **5 件套互相矛盾** | persona = "独立开发者" 但 positioning = "for 团队 lead" | HARD GATE 要做连贯性检查 |
| **Frame 报告太长** | 输出 5 页 PRD 草稿 | Frame 是 1.5-2 页，不是 PRD。详细需求是 D 段的事 |
| **直接接到代码** | Frame 完了跳 D 段直接写代码 | HARD GATE 之后只能进 D 段需求生成，不能跳 |

---

## 与现有 Skill 的关系

| Skill | 关系 |
|-------|------|
| `product-audit`（B 段） | **唯一上游**——Frame 必须基于 audit 报告启动 |
| `Product-Manager-Skills/proto-persona` | Step 1 PM 骨架借用 |
| `Product-Manager-Skills/jobs-to-be-done` | Step 2 PM 骨架借用 |
| `Product-Manager-Skills/problem-framing-canvas` | Step 3 PM 骨架借用 |
| `Product-Manager-Skills/positioning-statement` | Step 4 PM 骨架借用（升级版） |
| `Product-Manager-Skills/positioning-workshop` | Step 4 也可调用其交互式问答 |
| `Product-Manager-Skills/company-research` / `pestel-analysis` | Step 5 真实对手调研 |
| `steve-jobs-skill` | 每步 .2 检查段借用其表达 DNA + 二元判断 |
| `product-spec`（D 段，待建） | **唯一下游**——Frame 包直接喂给它 |

---

## 诚实边界

1. **5 件套不是 PRD**：Frame 包不写功能列表、不写技术栈、不写时间线——这是 D 段的事
2. **Jobs 检查会让流程变慢**：每步可能退回 1-2 次。如果用户嫌烦想"快速过"，告诉他："Jobs 检查就是用来防止快速过出平庸 Frame 的"
3. **不替代 audit**：如果 Verdict = shit 而用户硬要 Frame，直接拒绝——告诉他先杀方向或回 B 段重审
4. **5 件套可能在 D 段被推翻**：写需求时发现 Frame 不对，**允许回 C 段重做某步**，不要硬撑

---

## 后续衔接

```
product-frame 输出 Frame 包
   ↓
HARD GATE 通过
   ↓
product-spec (D 段) ← 唯一下一步
```

如果 Verdict 是 refactor 且某颗螺丝在 Frame 阶段拧不过去（如 Jobs 检查反复退回），**允许回 B 段重审 Verdict**——不要硬把 refactor 当 amazing 推下去。

---

## 调用示例（缩略）

> 用户：「audit 跑完了，Verdict = refactor，3 颗螺丝。把这个 frame 一下」
> 
> agent 读取 `docs/audits/2026-05-03-skill-fragment-remix.md`：
>   - Verdict ✓
>   - 核心动作（编 SKILL.md 不抄）✓
>   - 3 个 decision ✓
>   - weakest link（Anthropic Step 7）✓
>   → 入口检查通过
> 
> **Step 1 Persona**：让用户填 5 字段。Jobs 检查："闭眼说一句你服务的人。"
>   用户："Alex 这种自己折腾 Claude Code 配置的独立 AI 工具开发者，每周末搓 prompt 链。"
>   → 通过 ✓
> 
> **Step 2 JTBD**：填 when/I want/so I can。Jobs 检查："这个瞬间真发生过 ≥ 3 次吗？"
>   用户："发生过几十次——我自己每次写新 skill 都想抄某段。"
>   → 通过 ✓
> 
> **Step 3 Problem**：填三视角。Jobs 检查："这是问题不是方案？"
>   用户：LOOK INWARD = "skill 设计哲学是 follow"——这是问题，不是方案。
>   → 通过 ✓
> 
> **Step 4 Positioning**：填 5 行。Jobs 检查："1,000-songs 级 tagline。"
>   用户："积木式 skill" → ✓ 通过
> 
> **Step 5 Differentiation**：画对手坐标。Jobs 检查："1 周复制吗？"
>   用户："功能层 1 周能复制，但'多源整合 + 跨框架输出'有学习曲线壁垒"——⚠️ 黄灯，写进 Risks
> 
> HARD GATE 通过 → 输出 Frame 包到 `docs/frames/2026-05-03-skill-fragment-remix.md`
> 
> 提示进 D 段。

---

**Ready to frame. 给我 audit 报告路径，或粘 audit 切入语 + Verdict + 核心动作 + 关键 decision。**
