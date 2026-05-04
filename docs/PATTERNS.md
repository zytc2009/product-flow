# Patterns · 借鉴的具体模式致谢

> product-flow 不是从零创造，是把多个已有 skill 的精华编排成 5 段流程。
> 这份文档详细列出每个借用的 pattern，方便：
> - 理解每段为什么这么设计
> - 找到 pattern 的原始上下文
> - 在改进时知道改了什么会影响"借鉴一致性"

---

## 借自 superpowers / brainstorming

`D:/AI/code2/superpowers/skills/brainstorming/SKILL.md`

### Pattern 1 · 一次一问（One Question Per Message）

**原文**：
> "Only one question per message - if a topic needs more exploration, break it into multiple questions"

**用在**：
- A 段 product-spark · Step 1（选输入源）/ Step 4 Q1-Q5（每题独立）
- C 段 product-frame · 5 步 Jobs 检查（每步一问）
- D 段 product-spec · 5 phase Jobs 检查
- E 段 product-verdict · Step 2 三问（Q1 真增长 / Q2 sunk cost / Q3 环境）

**为什么借**：用户认知负载有限。一次问 5 个 = 用户脑内自动选 1 个回答，其他 4 个被浪费。一次一问强迫"逐个解决"。

---

### Pattern 2 · 多选优先（Multiple Choice Preferred）

**原文**：
> "Prefer multiple choice questions when possible, but open-ended is fine too"

**用在**：
- A 段 Step 4 全部 5 题都给 3-5 候选 + "都不对，我说"兜底
- C 段 Persona 标签 / Step 5 对手列表用枚举
- E 段触发类型分类用枚举

**为什么借**：多选比开放快 5 倍——用户挑数字比想答案容易。但要给"都不对，我说"出口避免被框住。

---

### Pattern 3 · `<HARD-GATE>` XML 标签

**原文**：
```html
<HARD-GATE>
Do NOT invoke any implementation skill, write any code, scaffold any project,
or take any implementation action until you have presented a design and the
user has approved it.
</HARD-GATE>
```

**用在**：
- A 段 product-spark · "进 Audit 入门检查"段
- C 段 product-frame · "进 D 段入门检查"段
- D 段 product-spec · "进 sub-agent / E 段入门检查"段

**为什么借**：markdown 文字描述容易被 LLM 当成"建议"。XML 标签让 LLM 把它当系统约束（rigid）。配合"不允许的事"明列边界，执行纪律强 10 倍。

---

### Pattern 4 · Spec self-review（4 项扫描）

**原文**（brainstorming Step 7）：
> "1. Placeholder scan: Any 'TBD', 'TODO', incomplete sections, or vague requirements? Fix them.
>  2. Internal consistency: Do any sections contradict each other?
>  3. Scope check: Is this focused enough for a single implementation plan?
>  4. Ambiguity check: Could any requirement be interpreted two different ways?"

**用在**：
- C 段 product-frame · HARD GATE 之前
- D 段 product-spec · HARD GATE 之前

**为什么借**：人写完文档很容易留 placeholder / 矛盾 / 歧义。让 agent "用新眼光" 扫一遍 catch 这些 bug，比 user 自己 review 更可靠。

**修改点**：原 brainstorming 是 user review 前的 self-review；本套是 HARD GATE 前 agent 自检（不需 user）。

---

### Pattern 5 · Visual Companion 协议

**原文**：
> "**This offer MUST be its own message.** Do not combine it with clarifying questions, context summaries, or any other content."
> "**Per-question decision:** Even after the user accepts, decide FOR EACH QUESTION whether to use the browser or the terminal."

**用在**：
- C 段 product-frame · 适用于 persona avatar / 2×2 差异化象限
- D 段 product-spec · 适用于 user flow / sub-agent 派单依赖图

**为什么借**：视觉信息密度 > 文字。但**逐题判断**是关键——不是开了 visual 就所有问题都用 visual，概念性问题（如 "JTBD 该怎么写"）仍然文字更好。

**修改点**：本套未实现 browser-based mockup（superpowers 用的），仅 ASCII 图表 + 简单 diagram。

---

### Pattern 6 · 落盘 design doc

**原文**：
> "Write the validated design (spec) to `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`"

**用在**：5 段全部都落盘到 `docs/<段名>/YYYY-MM-DD-<产品名>.md`

**为什么借**：会话结束后 context 丢失。落盘 = 跨会话连贯性 + 历史可追溯。

---

### Pattern 7 · YAGNI 心法

**原文**：
> "**YAGNI ruthlessly** - Remove unnecessary features from all designs"

**用在**：
- B 段 audit Pass 2 Q2 砍 80%
- D 段 spec Phase 4 MoSCoW MUST 砍 50%

**为什么借**：默认人会过度设计。YAGNI 是"未来不需要的东西现在不要做"——配合 Jobs Focus 心法（"不做"是积极动作不是消极动作）。

---

## 借自 superpowers / using-superpowers

`D:/AI/code2/superpowers/skills/using-superpowers/SKILL.md`

### Pattern 8 · "1% 适用就 invoke" 心法

**原文**：
> "Even a 1% chance a skill might apply means that you should invoke the skill to check."

**用在**：本套各段触发协议——用户口语表达只要有 1% 像 "审一下方向"，就应该 invoke product-audit 而不是直接讨论。

**为什么借**：让 agent 优先走 process skill 而不是 ad-hoc 即兴回答——避免 "这个简单不用 skill" 的 rationalization。

---

### Pattern 9 · Process skill 先于 implementation skill

**原文**：
> "Process skills first (brainstorming, debugging) - these determine HOW to approach the task
>  Implementation skills second (frontend-design, mcp-builder) - these guide execution"

**用在**：5 段全部是 process skill。**实施（写代码）不在 product-flow 范围内**——D 段输出 sub-agent dispatch hints 后交给 implementation skill 或 sub-agent 实施。

**为什么借**：边界清晰——product-flow 不抢 superpowers/test-driven-development 的活。

---

### Pattern 10 · Red flag 反 rationalize

**原文**：12 条 red flag（"这个简单" / "我记得" / "先看代码" 等）

**用在**：分布在各段的 anti-pattern 表里。例：
- spark "Q1-Q5 全开放（不给候选只问'你觉得呢'）"
- audit "Pass 跳跃（跳 Pass 1 直接进 Jobs 模式）"
- spec "PM 检查通过即放行（不做 Jobs 检查就进下一 Phase）"

**为什么借**：把"agent 偷懒"的常见模式列出来，让用户和 agent 都能识别。

---

## 借自 steve-jobs-skill

`D:/AI/skill/steve-jobs-skill/SKILL.md`

### Pattern 11 · 角色扮演协议

**原文**：
> "此Skill激活后，直接以Steve Jobs的身份回应。"
> "免责声明仅首次激活时说一次"
> "退出角色：用户说"退出""切回正常""不用扮演了"时恢复正常模式"

**用在**：
- B / C / D 段进入 Jobs 视角检查段时显式宣告
- HARD GATE 通过后显式切回 PM 教练态

**为什么借**：单 skill 内多角色切换容易让用户困惑。显式宣告 = 你知道现在 agent 是哪个人格。

---

### Pattern 12 · 6 心智模型

| 心智模型 | 用在哪段 |
|---------|---------|
| Focus = Saying No（聚焦即说不） | B 段 Pass 2 Q2 砍 80%；D 段 Phase 4 MUST 砍 50% |
| The Whole Widget（端到端控制） | B 段 Pass 2 Q3 体验链谁控制 |
| Connecting the Dots（连点成线） | A 段 Step 3 Cluster 模式识别 |
| Death as Decision Tool（死亡过滤器） | E 段 sunk cost 防御 + "杀产品的勇气" |
| Reality Distortion Field（现实扭曲力场） | （未直接用——RDF 不适合用户决策辅助） |
| Technology × Liberal Arts（技术×人文） | C 段 Step 4 tagline 的"insanely great"判断 |

**为什么借**：6 心智模型是 audit / frame 决策的判断维度，每个对应一类常见产品问题。

---

### Pattern 13 · 表达 DNA · 二元判断

**原文**：
> "禁忌词：不用「还行」「不错」「有待改进」。只有「amazing」和「shit」两档"

**用在**：
- B 段 Verdict 三档（amazing / refactor / shit）
- C 段 5 步 Jobs 检查（通过 / 退回，黄灯单独标）
- D 段 5 phase Jobs 检查
- E 段 Verdict 三档（持守 / 调整 / 杀）

**为什么借**：模糊判断 = 推迟决策 = 沉没成本。二元判断逼出真态度。

---

### Pattern 14 · 1,000-songs 级 tagline 标准

**原文**：
> "iPod = '1,000 songs in your pocket'，不是'5GB 存储的便携式 MP3 播放器'"

**用在**：
- B 段 Pass 2 Q1 一句话定义检查
- C 段 Step 4 tagline 检查（≤ 8 字 + 既具体又出人意料）

**为什么借**：好 tagline = 既具体又出人意料。字典词（"产品定制"）= 80 分待换。

---

## 借自 Product-Manager-Skills

`D:/AI/skill/Product-Manager-Skills/skills/<...>/SKILL.md`

### Pattern 15 · Geoffrey Moore 5 行 positioning

**来自**：`positioning-statement` (Component skill)

**用在**：
- A 段 Step 5 候选模板的 positioning 雏形
- B 段 Pass 1 Scaffold 完整 5 行
- C 段 Step 4 Positioning Frame 升级版

---

### Pattern 16 · JTBD 句式

**来自**：`jobs-to-be-done` (Component skill)

**句式**：When [触发情境] / I want to [想做的事] / So I can [更深层目标]

**用在**：
- A 段候选的 need 字段
- C 段 Step 2 JTBD Frame
- D 段 Phase 1 hypothesis 的 capability/outcome 描述

---

### Pattern 17 · Proto-persona

**来自**：`proto-persona` (Component skill)

**用在**：C 段 Step 1 Persona Frame（精简版 5 字段）

**修改点**：原 skill 是完整 persona 工作坊，本套精简到 5 字段（姓名 / 真实瞬间 / 工具栈 / 在乎 / 抗拒）。

---

### Pattern 18 · MITRE 三视角 Problem Framing

**来自**：`problem-framing-canvas` (Component skill)

**三视角**：LOOK INWARD / LOOK OUTWARD / REFRAME

**用在**：C 段 Step 3 Problem Frame

**最关键**：REFRAME 的"啊瞬间"——把"我以为是 A 问题"翻成"原来真问题是 B"。

---

### Pattern 19 · PoL Probe 5 类

**来自**：`pol-probe-advisor` (Interactive skill, by Dean Peters)

**5 类**：Feasibility / Task-Focused / Narrative / Synthetic Data / Vibe-Coded

**用在**：B 段 Pass 3 推荐 1 个 probe

**修改点**：原 skill 让用户挑 5 选 1，本套**强制只推 1 个**，符合 Jobs Focus 原则。

---

### Pattern 20 · Mike Cohn user story + Gherkin

**来自**：`user-story` (Component skill)

**用在**：D 段 Phase 2 Stories

**句式**：As [persona], I want to [action], so that [benefit] + Given/When/Then acceptance

---

### Pattern 21 · 8 splitting patterns

**来自**：`user-story-splitting` (Component skill, by Richard Lawrence)

**8 patterns**：role / workflow / data / operation / happy-path / acceptance / business-rule / quality-of-service

**用在**：D 段 Phase 3 Split 选合适 pattern

---

### Pattern 22 · MoSCoW

**来自**：`prioritization-advisor` (Interactive skill)

**4 档**：Must / Should / Could / Won't

**用在**：D 段 Phase 4 Scope（MVP 范围决策）

**修改点**：本套强制 MUST ≤ 5 个 + 必须配 WON'T 列表（拒绝引诱）。

---

### Pattern 23 · 1.5 页 PRD（精简版）

**来自**：`prd-development` (Workflow skill)

**用在**：D 段 Phase 5 PRD 输出

**修改点**：原 skill 是完整 PRD（10+ 页），本套**强制压缩到 1.5 页**——剩下细节由 sub-agent 在实施时即时 brainstorm。

---

### Pattern 24 · SaaS Health Diagnostic

**来自**：`business-health-diagnostic` / `saas-revenue-growth-metrics` / `saas-economics-efficiency-metrics`

**用在**：E 段 Step 1 Compare 的 metric 选择参考（如果产品是 SaaS）

---

### Pattern 25 · feature-investment-advisor

**来自**：`feature-investment-advisor` (Interactive skill)

**用在**：E 段 Verdict = "调整" 时，决定砍哪个 feature 调用此 skill 辅助。

---

## 借自 obsidian skill

`C:/Users/hongb/.claude/skills/obsidian/SKILL.md`

### Pattern 26 · query / topic-scout 模式作输入源

**用在**：A 段 product-spark · Step 2 · 当用户选输入源 = obsidian 时

**调用**：
```bash
python ~/.claude/skills/obsidian/memory_manager.py \
  --vault "${OBSIDIAN_VAULT_PATH:-~/obsidian}" \
  --mode query \
  --keywords "<用户给的关键词>"
```

---

## 借自 hermes-agent

`D:/AI/code/hermes-agent/`

### Pattern 27 · sub-agent 编排心法

**用在**：D 段 sub-agent dispatch hints 设计

**借鉴**：hermes-agent 的 multi-agent 编排（主 agent 派任务给子 agent）—— D 段输出的 PRD 不是给人看的，是给 Task tool / 子 agent 的派单清单。

---

### Pattern 28 · memory pattern 参考

**用在**：dogfood example TeamPulse 的 S002 story（团队术语 memory）的 sub-agent dispatch hints 引用了 hermes-agent 的 memory 系统作为 context_files。

---

## 借自 cron skill

### Pattern 29 · 时间触发自动化

**用在**：E 段 product-verdict 的"时间触发"模式

**调用**：用 `cron` skill 设月度 / 季度首日自动启动 product-verdict。

---

## 没借鉴但值得提的（独创设计）

下面这些是 product-flow 的**新设计**，没有直接 source 可借鉴：

### Original 1 · 5 段统一编排

把"灵感 → 上线后迭代"切成 5 段、每段唯一下游、HARD GATE 闸门——这个**整体架构**是新的。PM-Skills 有 47 个原子 skill 但没编排，superpowers 有流程但只覆盖 brainstorm + plan + impl 3 段。

### Original 2 · 振荡保护机制

D 段的"相邻 ≥ 3 / 总 ≥ 5 / 同 phase ≥ 3"阈值——基于"承认振荡是常态但要限定预算"的判断。原 PM-Skills 不承认振荡（线性流程假设），原 superpowers 不量化（只有"design approval"硬门）。

### Original 3 · Sunk cost 防御机制

E 段的"连续 2 次再观察 → 第 3 次禁止"硬规则——结合历史 verdict log 做事实依据。

### Original 4 · Sub-agent dispatch hints

D 段 PRD 里每个 MUST story 标 type / primary_skill / context_files / acceptance_check / human_review_gate——把 PRD 从"给人看的文档"变成"给 sub-agent 派单的元数据"。

### Original 5 · 磁盘优先调研

B 段 Pass 2.1 的"磁盘 > URL > Web"优先级——基于"磁盘上有相关项目时 grep 比 WebSearch 准 10 倍"的实测发现。

### Original 6 · 黄灯通过 + 记账

不是非黑即白的设计——某些检查项允许"通过但写进 Risks"。例：tagline 80 分（"借形不借义"）允许通过 + 记账"3 个月内换"。这是介于硬 PASS / 硬 RETURN 之间的中间态。

---

## 致谢声明

`product-flow` 是站在巨人肩膀上：

- **Jesse Vincent**（superpowers 作者）—— 一次一问 / HARD-GATE / self-review 协议
- **Dean Peters**（Product-Manager-Skills 作者）—— 47 个 PM 原子 skill 的形式化
- **Geoffrey Moore** —— Crossing the Chasm + 5 行 positioning 模板
- **Clayton Christensen** —— Jobs To Be Done 框架
- **Richard Lawrence** —— 8 splitting patterns
- **Steve Jobs（已故）** —— 6 心智模型 + 表达 DNA + Focus 心法
- **Walter Isaacson / Brent Schlender** —— Jobs 一手资料整理
- **MITRE** —— 三视角 Problem Framing
- **DSDM** —— MoSCoW

product-flow 的价值在**编排 + 升级**，而非原创。如果某个 pattern 你觉得有问题，先去找原作者的 skill 看是不是借鉴时丢了上下文——大概率是。

---

## 这份文档想让你看到 1 件事

> **product-flow 的强度 = 每个 pattern 的强度 × 编排的连贯性**。

单独某段质量 ≤ 它借的 source skill。但 5 段编排的**复合价值** = 你不用在 47 个 PM-Skills + N 个 superpowers + 1 个 steve-jobs-skill 之间手动决定何时切换。**这才是 product-flow 的产品价值**。
