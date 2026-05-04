# Glossary · 5 段流程术语表

> 按字母 / 段次序排列。术语第一次出现时建议读这里。

---

## 通用术语

### HARD GATE

各段的"出口闸门"。<HARD-GATE> XML 标签包装，让 LLM 把它当系统约束而非建议。

特征：
- **二元**：通过 / 退回，没有"基本通过"
- **强制**：不通过禁止输出
- **明列"不允许的事"**：让 LLM 知道边界

例：`product-spark` 的 HARD GATE 检查 4 项（target 具体 / 场景真实 / 对手真名 / 候选 1-3）。

### Jobs 视角 / Jobs 检查

每个 Step 或 Phase 内部的二元门，借自 `steve-jobs-skill` 的角色扮演协议。

特征：
- **角色切换显式宣告**：「现在切到 Steve Jobs 视角...」
- **二元判断**：通过 / 退回（黄灯允许通过但记账）
- **不允许 hedging**：禁用「还行」「不错」「视情况而定」
- **结尾切回**：「退出 Steve Jobs 视角」

### PM 教练态 / PM 中性语气

非 Jobs 视角的默认状态。

特征：
- 引导式、教学态、不评判
- 给模板让用户填空、不替用户编缺失项
- 用于 Step / Phase 的 .1 段（PM 骨架填空）和 HARD GATE 通过后的输出收尾

### Self-review · 4 项扫描

HARD GATE 之前的 agent 自检（**不需要 user 参与**）。借自 `superpowers/brainstorming` 第 7 步。

4 项：
- **placeholder 扫描**：任何 "TBD" / "TODO" / "待定" / 未填的 [xxx]
- **内部矛盾**：章节 / 字段间是否冲突
- **scope 检查**：是否聚焦 1 个产品方向（不混进多个）
- **歧义检查**：可被解读 ≥ 2 种意思的句子

仅 C 段 product-frame 和 D 段 product-spec 有此扫描。

### Visual Companion

可选的视觉辅助，借自 `superpowers/brainstorming`。

协议：
- offer 是**独立消息**（不和其他内容混）
- 用户同意 ≠ 整个 skill 都用 visual——**逐题判断**
- 概念性问题用文字，视觉性问题用 visual（ASCII 图表 / 简单 diagram）

仅 C 段和 D 段有此协议（A / B / E 段偏文字推理，加 visual 是过度）。

---

## A 段术语（product-spark）

### Capture

Step 2，取出原始想法阶段。

规则：
- **不评判** —— 用户说的任何想法都先记录
- **不追问** —— 模糊就模糊，Cluster 阶段会逼具体
- **数量下限**：< 3 条不能进 Cluster；单条想法走 escape hatch

### Cluster · 连点成线

Step 3，模式识别阶段。借 steve-jobs-skill 的"connecting the dots"心智。

agent 找 3 类信号：
- 反复出现的目标用户
- 反复出现的痛点
- 反复出现的技术拐点

输出"线 1 / 线 2 / 线 3 + 落单"，让用户挑 1 条展开。

### Converge

Step 4，3-5 个多选问题逼用户收敛到具体候选。借 superpowers/brainstorming 的"一次一问 + 多选优先"。

### 候选（Candidate）

A 段的输出单元。每个候选包含 4 字段：target / 核心场景 / 最强对手 / 差异化苗头。**数量上限 3 个**。

### 待审切入语

候选模板里的一段标准化文字，可直接复制粘贴喂给 product-audit。包含 3 项最小输入 + 差异化苗头。

---

## B 段术语（product-audit）

### Pass 1 · Scaffold

PM 教练态。Geoffrey Moore 5 行 positioning 填空，**红字标抽象字段**逼用户改紧。

### Pass 2 · Stress-test

Jobs 视角阶段。三段：
- **Pass 2.1 · 调研**（不可跳过）：磁盘优先 > URL > Web
- **Pass 2.2 · 三问三砍**：Q1 一句话 / Q2 砍 80% / Q3 体验链
- **Pass 2.3 · Verdict**：amazing / refactor / shit 三选一

### Pass 3 · Falsify

PM 教练态。1 个 PoL probe + 3 个 kill-switch metric。

### Verdict（B 段）

audit 的最终判决。**强制三选一**，禁止 "good" / "还行" / "有潜力" 等 hedging：

- **amazing** → 进 PRD
- **refactor** → 拧 N 颗螺丝再来
- **shit** → 杀方向

### kill-switch metric

audit Pass 3 设的"达不到就杀"阈值。3 个 metric：核心信号 / 成本约束 / 用户反应。E 段周期性回来对比这 3 个。

### PoL Probe（Proof of Learning Probe）

Dean Peters 的 5 类原型：Feasibility / Task-Focused / Narrative / Synthetic Data / Vibe-Coded。borrowed from `Product-Manager-Skills/pol-probe-advisor`。

audit Pass 3 强制只推 1 个（不像原 skill 让用户挑），符合 Jobs Focus 原则。

### 磁盘优先调研

Pass 2.1 的特殊机制。当审计的方向涉及用户磁盘上有的同类项目，优先 grep / read 真代码而非 WebSearch——比二手网络评论精准 10 倍。

---

## C 段术语（product-frame）

### Frame Pack · 5 件套

C 段的输出。包含 5 件套：

1. **Persona** —— 具体角色 + 1 个使用瞬间
2. **JTBD** —— when / I want to / so I can
3. **Problem reframe** —— LOOK INWARD / OUTWARD / REFRAME
4. **Positioning** —— Geoffrey Moore 5 行
5. **Differentiation** —— 真实对手坐标 + 你占据的空白象限

### REFRAME · 啊瞬间

Problem 阶段的核心动作。把"我以为是 A 问题"翻成"原来真问题是 B"——听到的人脑子里"啊"一下。

例：「AI 工具不够好」→ 「主持人没 30 分钟空档」是真翻转。

### 1,000-songs 级 tagline

Step 4 Jobs 检查的标准。借自 iPod 的 "1,000 songs in your pocket"——既具体又出人意料。

判断：
- ✅ 绿灯 = 能脱口而出且听到的人有画面感（≤ 8 字）
- ⚠️ 黄灯 = 80 分（"借形不借义"如"skill 量体裁衣"，3 个月内换）
- ❌ 红灯 = 字典词或泛词（"skill 定制" / "产品个性化"）

### 黄灯通过 + 记账

不是非黑即白的设计——某些检查项允许"通过但记录在 Risks 段"。例：differentiation 1 周内可复制 = 黄灯，写进 Risks 让 D 段必须考虑护城河。

---

## D 段术语（product-spec）

### 受控振荡（Controlled Oscillation）

D 段的核心设计。承认 phase 间反复来回是常态（写需求时自然），但加预算上限：

- 同对相邻 ≥ 3 次 → 红灯
- 总跨 phase ≥ 5 次 → 强制 stop
- 同 phase Jobs 退回 ≥ 3 次 → 红灯

超阈值不是惩罚，是**诊断信号**——上游 Frame 没成型。

### Oscillation Log

D 段的振荡记录。每次跨 phase 跳转写一行（时间 + 方向 + reason）。用作 HARD GATE 时阈值检查依据。

### MUST / SHOULD / COULD / WON'T HAVE

MoSCoW 优先级框架。borrowed from `Product-Manager-Skills/prioritization-advisor`。

- **MUST**：MVP 必有 —— 上限 3-5 个
- **SHOULD**：强化版 —— 不做 MVP 不致命
- **COULD**：锦上添花 —— Phase 2 之后
- **WON'T**：明确不做 —— 写下来拒绝引诱

### Sub-agent dispatch hints（D 段独创）

每个 MUST story 标的派单元数据：

```yaml
type: code-writer | researcher | reviewer | tester
primary_skill: 适合的 skill 路径
reference_skills: [辅助 skill]
context_files: [可选参考文件]
acceptance_check: 自动化验收命令
estimated_complexity: low | medium | high
human_review_gate: 何时必须人工 review
```

让 PRD 输出可被 Task tool / 子 agent 直接消费——不是给人看的传统 PRD。

### Definition of Done（DoD）

PRD 末尾的"完整 ship 条件"清单。verifiable（每条都可被检查）。

---

## E 段术语（product-verdict）

### 触发类型 · 3 类

- **时间触发** —— cron 月度 / 季度首日
- **metric 触发** —— audit 设的 kill-switch 任一项首次触及阈值
- **外部触发** —— 竞品大动作 / 平台原生功能 / 团队战略变化

### Verdict（E 段）

E 段的最终判决。**强制三选一**：

- **持守（Continue Investing）** → 继续投，下个周期 1-2 个具体动作
- **调整（Refactor / Pivot Lite）** → 砍 / 换某 feature 或 positioning
- **杀（Kill）** → 停止投入，回 A 段找新方向

### Sunk cost 防御

E 段的核心机制。**硬规则**：

```
连续 2 次 verdict = "再观察 / 持守"  →  允许，记账
第 3 次想说"再观察"                 →  禁止，强制变 "调整" 或 "杀"
```

对抗"舍不得杀方向"的本能，用历史 verdict log 做事实依据。

### 历史 index

`docs/verdicts/<产品名>-history.md`。每次 verdict 追加 1 行（日期 / 触发类型 / Verdict / 下次时间）。是 sunk cost 防御的事实依据——agent 下次跑时必查。

### 真增长 vs noise

Step 2 Q1 的判断维度：

- **真增长** = 数据连续稳定 ≥ 2 周/月 + 自然扩散（非单点事件驱动）
- **noise** = 由单点事件驱动（一篇博客 / 一次推荐）+ 数据波动 > ±30%

只有真增长才能基于此 verdict。noise 必须等下次 verdict 重看。

### 色标规则

Compare 阶段的 metric 状态：

- 🟢 绿 = 达标 +20% 以上（明显超）
- 🟡 黄 = ±20% 震荡区（noise 嫌疑）
- 🔴 红 = 触及阈值或低于 20%（明确未达）

---

## 跨段术语

### 上游 / 下游

每段都有**唯一上游**和**唯一下游**：

| 段 | 上游 | 下游 |
|----|------|------|
| A · spark | obsidian / 用户口头 | B（唯一） |
| B · audit | A | C 或 杀（amazing/refactor → C；shit → 终止） |
| C · frame | B | D（唯一） |
| D · spec | C | sub-agent 实施 / E |
| E · verdict | D 实施完成 + audit kill-switch | 持守等下次 / 调整回 D / 杀回 A |

跨段时**必须**遵循"唯一下游"规则，禁止跳段（除非 audit shit 或 E 调整 / 杀）。

### 向上回退

各段允许在发现上游问题时回上一段重做：

- D 振荡超阈值 → 回 C 段重做 Frame
- E 环境关键变化 → 回 B 段重审 audit
- audit refactor 螺丝拧不动 → 回 A 段重 spark

**鼓励向上回退，不要硬撑**。

### 落盘路径约定

```
docs/sparks/YYYY-MM-DD-<产品名>.md
docs/audits/YYYY-MM-DD-<产品名>.md
docs/frames/YYYY-MM-DD-<产品名>.md
docs/prds/YYYY-MM-DD-<产品名>.md
docs/verdicts/<产品名>-YYYY-MM.md
docs/verdicts/<产品名>-history.md  ← sunk cost 防御依据
```

---

## 缩写速查

| 缩写 | 全称 | 出处 |
|------|------|------|
| JTBD | Jobs To Be Done | Clayton Christensen / `jobs-to-be-done` skill |
| MVP | Minimum Viable Product | Lean Startup |
| PoL | Proof of Learning | Dean Peters |
| RICE | Reach × Impact × Confidence / Effort | Intercom |
| ICE | Impact × Confidence × Ease | Sean McBride |
| MoSCoW | Must / Should / Could / Won't have | DSDM |
| OKR | Objectives and Key Results | Andy Grove / John Doerr |
| DoD | Definition of Done | Agile / Scrum |
| ASR | Automatic Speech Recognition | 通用术语 |
| EM | Engineering Manager | 通用术语 |
| LLM | Large Language Model | 通用术语 |
| CAC / LTV | Customer Acquisition Cost / Lifetime Value | SaaS metrics |
