# Walkthrough · TeamPulse 端到端叙事

> 用 1 篇连续叙事跑完 5 段。比读 5 个独立 dogfood example 快 5 倍。
> 时间线：2026-05-03 想起这件事 → 2026-08-03 跑第一次 verdict。

**虚构产品**：TeamPulse · 5-15 人小团队的会议→action 自动化工具。

读完这篇你应该能脑内跑通整个 product-flow，再去看 5 个独立 example 时只需要查细节。

---

## 周日晚上 · 2026-05-03 21:00 · A 段 spark 启动

**起因**：周日晚上整理下周计划，李工（你）想起最近 3 周一直被会议笔记拖累——但脑子里有点散。

打开 Claude Code，说：

> "我有几个 AI 工具想法不知道做哪个"

→ 触发 **product-spark**。

agent 问：「输入源选哪个？」你选 **3 · 混合**——脑子里 4 条 + obsidian 翻 1 条。

### Capture

倒出来 5 条：
1. 团队周会忘记提的 action item，没人跟进
2. "上次老王说要做的那件事" 想搜搜不到
3. PM 1:1 后想自动提取 follow-up
4. 给老板做 weekly report 翻笔记 2 小时
5. （来自 obsidian fleeting）远程同事看不完会议录像

### Cluster · 连点成线

agent 找模式：

```
线 1（主）：会议→action 链路自动化     [#1, #3, #5]
线 2（次）：会议历史不可搜              [#2, #4]
```

你选**线 1**（最痛、最常见）。

### Converge · 4 题多选

| Q | 选项 | 你选 |
|---|------|------|
| target | EM/Tech Lead, 跨时区远程, AI 工具开发者, 团队管理者 | **2 · 5-15 人小团队 EM/Tech Lead** |
| 场景 | 周会后 markdown / 中途搜历史 / 跨时区摘要 | **1 · 周一晨会后 30 分钟内拿派工 markdown** |
| 对手 | Otter / 飞书 AI / 不做 / 自己手写 | **1 + 4** Otter + 不做忍着 |
| 差异化 | 速度 / 中文 + 团队 context / 小众 / 新技术 | **2 · 中文 + 团队 context** |

5 件套候选 1 出来了。HARD GATE 4 项全过（target 具体 ✓ / 场景真实 ✓ / 对手真名 ✓ / 候选 ≤ 3 ✓）。

**待审切入语**复制到剪贴板，进入 B 段。

---

## 周日晚上 · 21:30 · B 段 audit 启动

粘切入语：

> "我想做 TeamPulse —— 5-15 人小团队 EM/Tech Lead 用的会议→action 自动化。target = 5-15 人小团队 EM。场景 = 周一晨会 30 分钟内拿到派工 markdown。最强对手 = Otter / 不做忍着。差异化 = 中文 + 团队 context。审一下方向。"

### Pass 1 · Scaffold（PM 教练态）

agent 把切入语改成 Geoffrey Moore 5 行，标红了 2 处太抽象：

- 🔴 target = "5-15 人小团队 EM" → 是哪种 EM？产品 / 工程？
- 🔴 benefit = "拿到 markdown" → 拿到完做什么？

你改紧：target = "**5-15 人产品技术团队的 EM**"，benefit = "**让他不用挨个问、不用自己手写**"。

### Pass 2 · Stress-test（Jobs 视角）

> "现在切到 Steve Jobs 视角，会比较直接，不 hedging。"

#### Step 2.1 · 调研（磁盘优先）

agent 静默扫磁盘：
- `D:/AI/code/agent-skills` 没有专门会议工具
- `D:/AI/code/hermes-agent` 有 memory + cron 但没会议总结

WebSearch 补 Otter / Fireflies / tldv：
- Otter 中文准确率 70%、action 提取靠关键词
- 飞书自带 AI 摘要 v1 通用化
- **空白点**：中文 + 中小团队 + action（非摘要）

#### Step 2.2 · 三问三砍

**Q1 一句话**：你说"会议→action 30 分钟"——8 字 ✓ 通过（但不够性感，记账 3 个月内换）

**Q2 砍 80%**：你列了 7 个功能（ASR / 摘要 / action / 派工 / 搜索 / report / 术语）——砍到 1 个：**action 提取**。Jobs 评：「ASR 不是你的事，让 Otter 做。」 ✓ 通过

**Q3 体验链**：

| # | 步骤 | 谁控制 |
|---|------|------|
| 1 | 听说 | 社区 |
| 2 | 安装 | 用户 |
| 3 | 导入 transcript | **会议平台 API** ❗ |
| 4 | ASR 转录 | **ASR 服务** ❗ |
| 5 | LLM 提取 | **OpenAI/Claude** ❗ |
| 6 | 派工到 task 系统 | **Notion/Lark/Jira** ❗ |
| 7 | 团队执行 | 用户 |

🔴 **4 个外部接口**——这是体验天花板。Jobs："你的产品在 4 个外人肩膀上跳舞。"

#### Pass 2.3 · Verdict

**REFACTOR** —— 3 颗螺丝必须拧：

- **D1** 产品形态 = 不做 ASR，专注 transcript 后处理（让用户粘）
- **D2** 集成点 = 不做平台 API，避免 4 个外部依赖
- **D3** 护城河 = 团队 context 学习曲线（6 个月时间壁垒）

### Pass 3 · Falsify（PM 教练态）

> "退出 Steve Jobs 视角，切回结构化模式。"

- **Probe**: Vibe-Coded · 1 周搓 dogfood
- **3 个 kill-switch**：
  1. 自己 dogfood 周用次数 ≥ 3 / 周（连续 4 周）
  2. 5 个 EM 朋友试用 → ≥ 3 实际用过
  3. Otter 中文版风险 ≤ 6 个月内未发

audit 报告落盘 `docs/audits/2026-05-03-teampulse.md`。

---

## 周日晚上 · 22:30 · C 段 frame

audit 给了 refactor + 3 颗螺丝。你想趁热把 5 件套 frame 出来：

> "frame 一下 teampulse"

→ 触发 **product-frame**。

### Step 1 · Persona

agent 给 5 字段模板。你填：

- **李工**，36 岁产品技术 EM，7 人团队
- 真实瞬间：周一 10:00 站会刚开完，邮件 / 面试 / PR 同时来——下午 4:00 想"今天会上王哥说要做啥来着"
- 工具栈：飞书 / Notion / Linear / Cursor / ChatGPT
- 在乎：团队不被会议拖死 / action 真执行 / 周一节奏感
- 抗拒：又一个 SaaS 账号 / 看不懂数据怎么"被学习"

**Jobs 检查**：闭眼脱口而出"李工，36 岁 EM，周一开完会下午就忘了王哥说要做啥" ✓ 通过

### Step 2 · JTBD

你写：
> When 周一晨会刚结束（11:00 之前），I want to 拿到一份"谁要做什么"的 markdown，so I can 把 action 派给团队成员、不用挨个问 / 自己写笔记。

**Jobs 检查**：真痛点？你 12 周里 11 次中招 + 3 个 EM 朋友说"对对对" → ✅

### Step 3 · Problem（**演示退回**）

第 1 次你写：
> LOOK INWARD: 用户没有自动化工具帮他从 transcript 提取 action

**Jobs 退回**：「Stop. '用户没有自动化工具' 是方案描述不是问题描述。'缺工具' 不是问题，'丢失 action' 才是。退回。」

第 2 次写：
> LOOK INWARD: EM 同时承担"主持 + 整理 + 派发 + 跟进" 4 个角色，会议刚结束时正是注意力最分散的时候。
>
> LOOK OUTWARD: 现有工具是为大团队设计，把"会后空档" 误读为"摘要需求"。
>
> REFRAME: 表面问题 = "AI 工具不够好"。**真问题 = "主持人没 30 分钟空档"**。
>
> **啊瞬间**：解决方案不是"做更好的 AI 摘要"，是"把整理的人换成 AI"。

**Jobs 检查**：是问题不是方案 ✓ Reframe 是真翻转 ✓

### Step 4 · Positioning

5 行 + 8 字 tagline："**会后 30 分钟派工**"

**Jobs 检查**：8 字、可朗读、动词明确 ✓ 绿灯

### Step 5 · Differentiation

对手坐标：Otter（左上：通用 + 全周期）/ 飞书（左上：通用 + 全周期）/ **你（右下：团队定制 + 30 分钟空档）**

**Jobs 检查**：1 周复制？
- 功能层（中文 ASR + LLM 提取）→ 1 周可复制
- 团队 context 学习层 → 6 个月学习曲线，不可直接复制
- ⚠️ **黄灯**通过 + 写进 Risks

### Self-review 4 项 + HARD GATE

agent 自检：
- placeholder ✓ 无 TBD
- 内部矛盾 ✓ 5 件套连贯
- scope ✓ 聚焦 1 个产品
- 歧义 ✓ 句子无双向解读

HARD GATE 通过。Frame Pack 落盘 `docs/frames/2026-05-03-teampulse.md`。

---

## 周一上午 · 2026-05-04 09:00 · D 段 spec

> "frame 完了，写需求"

→ 触发 **product-spec**。读取 Frame Pack。

### Phase 1 · Hypothesis

> We believe **EM 李工** needs **会后 30 分钟内 markdown 派工清单** in order to **不丢失 action / 不花 1 小时整理**。
>
> Leading: dogfood 1 周后主动用 ≥ 3 次/周 + 5 个 EM 朋友 ≥ 3 人愿意保留下周接着用
> Lagging: action 派工时间 2h → 30min；团队 action 完成率 +20%

**Jobs 检查**：1 周可证伪？dogfood + 5 朋友试用，2 周内能拿到 leading ✓

### Phase 2 · Stories

写了 4 个 detail story（粘 → 提取 → 编辑 → 派发）。

**Jobs 检查**：一句话讲完每个？✓

### Phase 3 · Split

把 epic story 拆成 7 个小 story。但 Jobs 检查发现 2/5 split 后不独立——比如 "S005 编辑界面渲染" 和 "S006 编辑后同步" 必须一起 ship 才有用户价值。

**Jobs 退回 Phase 2** ↕（振荡 #1）。重写 story 后再 split。

```
14:32 · Phase 3 → Phase 2（reason: split 后 2/5 不独立）
14:42 · Phase 2 → Phase 3（重写完成）
```

### Phase 4 · Scope

MoSCoW MUST 列了 5 个，Jobs："砍 50%。" 你心痛地砍到 3 个。

但砍完发现 **S004 split 粒度不对了** —— 原本 S004 是"高级编辑功能"，现在 S004 移到 SHOULD 但 split 还按"高级"粒度，不合理。

**Jobs 退回 Phase 3** ↕（振荡 #2）。重 split 后再回 Phase 4。

```
14:58 · Phase 4 → Phase 3（reason: MUST 砍后 split 粒度不对）
15:05 · Phase 3 → Phase 4（重 split）
```

### Phase 5 · PRD（含 sub-agent dispatch）

输出 1.5 页 PRD：3 个 MUST story + acceptance + sub-agent dispatch hints + WON'T 列表 + Risks + DoD。

每个 MUST story 标好 sub-agent type / context_files / acceptance_check / human_review_gate。

### Self-review 4 项 + HARD GATE

oscillation log 累计 8 次 / 5 阈值 ⚠️——总跳转超阈值。

但每对相邻 ≤ 3（Phase 2↔3 = 2 次，Phase 3↔4 = 1 次） → **黄灯通过**，记账"下次 frame 多花时间"。

PRD 落盘 `docs/prds/2026-05-04-teampulse.md`。可以派单给 sub-agent 实施了。

---

## 接下来 1 周 · Vibe-Coded probe

你按 audit Pass 3 的推荐，1 周搓 dogfood：

- Day 1-2: Markdown transcript parser + LLM prompt v1
- Day 3-5: sqlite memory（团队术语）+ 跑 5 个真实会议 transcript
- Day 6-7: 自己用 1 周——每周开 3 个会议都用它

第 1 周结束：自己用了 4 次 ✓ 团队术语解析准确率 60% ✓ 决定继续投。

---

## 一段时间过去 · 3 个月后 · 2026-08-03 · E 段 verdict

cron 触发月度 verdict：

> "上线 3 个月了，跑一次 verdict"

→ 触发 **product-verdict**。

### Step 1 · Compare

读取 audit kill-switch + 你提供现实数据：

| # | Metric | 阈值 | 现实 | 色标 |
|---|--------|------|------|------|
| 1 | dogfood 周用次数 | ≥ 3 次 | **5 次/周连续 12 周** | 🟢 |
| 2 | 5 EM 朋友 try ≥ 3 | ≥ 3/5 | **4/5（其中 2 持续 ≥ 6 周）** | 🟢 |
| 3 | Otter 中文版风险 | 6 月内未发 | **未发** | 🟢 |

附加输入：
- 朋友反馈："yml 不直观，能交互式生成吗"
- 飞书 7 月发了"会议 AI v2"——但仍通用化
- 你自己工作模式没变化

### Step 2 · Diagnose（Jobs 视角）

> "现在切到 Steve Jobs 视角做诊断。"

- **Q1 真增长**：12 周稳定 + 自然扩散 ✓ 真信号
- **Q2 sunk cost**：第 1 次 verdict，sunk cost 防御未触发 ✓
- **Q3 环境**：所有 audit 假设仍成立 ✓

### Step 3 · Verdict

> "退出 Steve Jobs 视角。"

**持守（Continue Investing）**

下个月动作：
1. 加交互式 yml wizard（响应朋友反馈）
2. 加腾讯会议 transcript 格式支持

**不做的**：飞书 API / ASR / weekly digest

下次 verdict：2026-09-01

⚠️ **预警**：第 2 次"持守"会被记账，第 3 次禁止——届时必须出新增长信号或战略改进。

verdict 落盘 `docs/verdicts/teampulse-2026-08.md`。历史 index 加 1 行。

---

## 总结：5 段全跑完用了多少时间？

| 段 | 实际用时 | 说明 |
|----|--------|------|
| A · spark | 周日晚 ~15 min | 输入混合 + 5 想法 + 2 候选 |
| B · audit | 周日晚 ~25 min | 含磁盘 + Web 调研 |
| C · frame | 周日晚 ~30 min | Step 3 退回 1 次 |
| D · spec | 周一上午 ~50 min | 2 次振荡 + 完整 sub-agent dispatch |
| (Vibe-Coded probe) | ~12 工时（1 周晚 + 周末） | dogfood 验证 |
| E · verdict | 3 个月后 ~12 min | 月度 cron 触发 |

**纯文档时间**：~120 min 五段全跑完一遍——比传统从 0 写 PRD 快 3 倍。

---

## 这一篇 walkthrough 想让你看到 5 件事

1. **5 段是连续的，不是孤立的**——每段输出直接喂下一段，无需重述背景
2. **Jobs 退回 / 振荡是常态**——本例 frame 退回 1 次 / spec 振荡 2 次都是健康的
3. **黄灯通过 + 记账**比"红灯硬退" / "绿灯纵容"都好——本例 differentiator 黄灯 + spec 总跳转黄灯都用这个机制
4. **sub-agent dispatch hints 是 D 段的核心**——PRD 不是给人看的，是给 sub-agent 派单的清单
5. **E 段的 sunk cost 防御从第 1 次就开始记账**——本例预警"第 2 次会记账"，让你知道往后是有约束的

---

## 看完这个 walkthrough 之后

- 想看更细的某段？读对应的 `skills/<段名>/examples/dogfood-teampulse.md`
- 想知道某术语？查 [GLOSSARY.md](GLOSSARY.md)
- 想看具体借了哪些 pattern？查 [PATTERNS.md](PATTERNS.md)
- 想看版本历史？查 [CHANGELOG.md](../CHANGELOG.md)
- 想直接跑一个真实方向？读 [README.md 的 60 秒上手](../README.md)
