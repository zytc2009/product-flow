---
name: product-spec
description: |
  把 Frame Pack（5 件套）展开成 1 页可交给 sub-agent 执行的精简 PRD。5 phase 受控振荡：
  hypothesis → stories → split → scope → PRD。承认 phase 间切来切去是常态，但加振荡上限防无限循环。
  当用户说「写需求」「展开 frame 写 PRD」「把方向变成 user stories」「需求生成」「frame 完了写需求」时触发。
intent: >-
  五段产品流程的 D 段。在 Frame 之后、实施之前，把 5 件套展开成 testable hypothesis、user stories、
  splitting、MoSCoW scope、最终输出 1 页 sub-agent friendly PRD。承认振荡（你最高频切换的痛点区），
  但加预算上限：相邻 phase 来回 ≥3 次或总跳转 ≥5 次 → 红灯回 C/B 段。
type: interactive
theme: requirement-engineering
estimated_time: "30-60 min"
best_for:
  - "Frame Pack 完成后展开成可交付的需求"
  - "把抽象方向变成 testable hypothesis + user stories + MVP scope"
  - "输出 1 页 sub-agent friendly PRD（不是 10 页传统 PRD）"
scenarios:
  - "frame 跑完了，把 5 件套展开成需求"
  - "我有 hypothesis 但 user story 写不出来，需求生成卡住了"
  - "MVP 砍不动，需要强制收敛 scope"
---

# Requirement Generation Loop · 受控振荡式需求生成

> 这是产品流程的 D 段。在 Frame 之后、实施之前。
> 它不是线性流——它**承认振荡**（你最高频切换的痛点区），但**限定振荡预算**防无限循环。

## 触发与定位

| 用户的状态 | 触发本 skill | 不触发本 skill |
|-----------|------------|--------------|
| Frame Pack 完成（5 件套齐） | ✓ | |
| 已有 hypothesis 但 user story 写不出来 | ✓ | |
| MVP scope 砍不动 | ✓ | |
| 还没有 Frame Pack | | → 先走 `product-frame`（C 段） |
| PRD 写完了要交付 | | → 直接交给 sub-agent / 进 E 段 `product-verdict` |

**输入**：`docs/frames/<name>.md`（5 件套 Frame Pack）
**输出**：`docs/prds/<name>.md`——1 页精简 PRD，sub-agent friendly handoff 格式

---

## 总流程（5 Phase 受控振荡）

```
入口：Frame Pack from C 段
   ↓
Phase 1 · Hypothesis      We believe X needs Y to achieve Z. We'll know when [signal].
   ↕  ← 回 C 段如果 Frame 不准
Phase 2 · Stories         As [persona], I want [action], so that [benefit] + Gherkin AC
   ↕  ← 回 Phase 1 如果发现 hypothesis 错
Phase 3 · Split           大 story → 独立可交付小 story（8 patterns）
   ↕  ← 回 Phase 2 如果发现 story 写错粒度
Phase 4 · Scope           MoSCoW 砍 MVP（Must / Should / Could / Won't）
   ↕  ← 回 Phase 3 如果发现 split 粒度不对
Phase 5 · PRD             1 页 sub-agent friendly handoff
   ↓
HARD GATE → 交给 sub-agent 执行 / 进 E 段
```

每个 Phase 内部都是 mini 三段：**PM 教练态填空 → Jobs 视角戳 → 用户改紧或退回**。Jobs 视角和 audit/frame 用同一套角色切换协议。

---

## 振荡保护机制（D 段独有，**最重要**）

承认你说过的"切来切去最多在这段"，但加预算：

| 跳转类型 | 上限 | 触发后行为 |
|---------|------|----------|
| 同对相邻 Phase 来回（如 Phase 2 ↔ Phase 3） | **3 次** | 红灯：意味上游 Frame 或 Verdict 错了 → 回 C 段重审 5 件套 |
| 整个 D 段总跨 Phase 跳转数 | **5 次** | 强制 stop：回 C 段 |
| 同一 Phase 内 Jobs 检查退回（如 Phase 2 .2 检查不通过 → 回 .1 重填） | **3 次** | 红灯：当前 Phase 用户没想清，回上一 Phase |

**振荡计数器**：在 `template.md` 顶部维护一个 `oscillation_log`，每次跳转写一行：

```
2026-05-03 14:23 · Phase 2 → Phase 1（reason: hypothesis 不可证伪）
2026-05-03 14:35 · Phase 1 → Phase 2（reason: hypothesis 改完）
2026-05-03 14:48 · Phase 2 → Phase 3
...
```

**这不是惩罚，是诊断信号**——3 次 ↔ 表示问题不在 D 段，是 C 段 Frame 没成型。逼用户回 C 段比硬撑写 PRD 健康。

---

## 入口检查

读取 Frame Pack（`docs/frames/<name>.md`），提取 5 件套全部字段：

| 必备 | 缺了怎么办 |
|------|-----------|
| Persona（含使用瞬间） | 缺 = Frame 没做完，回 C 段 |
| JTBD（when/I want/so I can） | 缺 = 回 C 段 |
| Problem reframe | 缺 = 回 C 段 |
| Positioning 5 行 + Tagline | 缺 = 回 C 段 |
| Differentiation 对手坐标 | 缺 = 回 C 段 |
| audit Verdict + 关键 decision | 缺 = 回 B 段 |

5 件套不齐 → **不进 Phase 1**。

---

## Phase 1 · Hypothesis（假设）

### 1.1 PM 骨架（教练态）

借 `Product-Manager-Skills/epic-hypothesis` + `lean-ux-canvas`：

```
We believe [persona, 从 Frame 直接搬]
    needs [capability, 一个具体能力，不是产品名]
in order to [outcome, 具体可观察的结果]

We'll know we're right when:
- Leading indicator: [行为/信号，1 周内可观察]
- Lagging indicator: [业务结果，1-3 月可衡量]
```

写 **1 个主 hypothesis + 0-2 个相关 hypothesis**。多了 = 没收敛。

### 1.2 Jobs 视角检查

> 切到 Jobs 视角（首次进入显式宣告）：
> 「现在切到 Steve Jobs 视角检查。基于公开言论推断，非本人观点。」

**检查问题**：「这个 hypothesis 1 周内能证伪吗？给我证伪的具体动作。」

判断：
- **能给出 1 周内可执行的证伪动作**（"找 5 个 persona 给他们看 demo"）→ ✅ 进 Phase 2
- **只能说"等用户量上来再看"** → ❌ 红灯。**不可证伪 = 不是 hypothesis 是信仰**
- **leading indicator 是 lagging indicator 伪装的**（"7 天内 retention 50%"——这其实是 lagging）→ ⚠️ 退回 1.1 拆出真 leading

**Jobs 戳法**：「If your hypothesis takes 6 months to falsify, you've already burned 6 months. Find the leading indicator that lights up in days.」

**振荡风险**：如果 Phase 1 反复退回 ≥ 3 次 → 红灯：JTBD 或 Problem 没成型 → 回 C 段。

---

## Phase 2 · Stories（用户故事）

### 2.1 PM 骨架（教练态）

借 `Product-Manager-Skills/user-story`（Mike Cohn + Gherkin）：

```
As a [persona, 来自 Frame]
I want to [action, 用户层面的动作，不是技术实现]
So that [benefit, 用户感知到的价值]

Acceptance Criteria（Gherkin 三段式）：
- Given [初始状态]
- When [用户动作]
- Then [预期结果，必须可验证]
```

**禁止的写法**（直接退回）：
- ❌ "As a user, I want to use the system" — persona 抽象
- ❌ "I want to click the button" — action 是技术实现，不是用户层面
- ❌ "So it works" — benefit 不存在
- ❌ Acceptance 写"系统应该正常运行" — 不可验证

每个 hypothesis 通常对应 **1 个 epic-level story + 3-5 个 detail story**。

### 2.2 Jobs 视达检查

**检查问题**：「闭眼**一句话**讲完每个 story——给一个用户听，他能 understand 你在做什么吗？」

判断：
- **每个 story 一句话讲清** → ✅ 进 Phase 3
- **必须看 Gherkin 才知道在做什么** → ❌ 退回。**Gherkin 是验收标准不是核心，story 自己应该立得住**
- **某 story 跟 hypothesis 对不上** → ❌ 退回 Phase 1，hypothesis 漏了或 story 跑偏了

**Jobs 戳法**：「If you can't tell the story without reading the spec, the story isn't real. Real stories are verbal.」

---

## Phase 3 · Split（拆分）

### 3.1 PM 骨架（教练态）

借 `Product-Manager-Skills/user-story-splitting` 8 patterns + `epic-breakdown-advisor` 9 patterns。**先识别哪些 story 太大**，再用对应 pattern 拆：

| Story 太大的信号 | 适用拆分 pattern |
|----------------|----------------|
| 涉及多种用户角色 | Split by user role |
| 涉及多个工作流 | Split by workflow steps |
| 涉及多种数据类型 | Split by data variation |
| 涉及多个 CRUD 操作 | Split by operation (Create / Read / Update / Delete) |
| 涉及简单+复杂路径 | Split by happy path / edge case |
| 涉及多个验收标准 | Split by acceptance criteria |
| 涉及多种业务规则 | Split by business rule variation |
| 涉及性能/质量分级 | Split by quality of service |

每个 split 后的小 story 必须：
- 独立可交付（不依赖其他 split）
- 独立给用户价值（哪怕只是部分）
- 可在 1-3 工作日完成

### 3.2 Jobs 视角检查

**检查问题**：「每个 split 后的小 story——单独 ship 给用户能让他们说 'amazing' 还是 'where's the rest'？」

判断：
- **单独 ship 都有价值**（用户拿到了真东西，哪怕窄）→ ✅ 进 Phase 4
- **某些 split 单独 ship 用户会说"这是半成品"** → ❌ 退回。**不是 split，是切碎**
- **split 后总数超过 15 个** → ⚠️ 黄灯。MVP 之外的 split 暂存到 backlog，不进 Phase 4

**Jobs 戳法**：「Splitting a feature into 12 useless 1-day pieces is worse than shipping 1 useful 2-week piece. Each piece must stand on its own.」

---

## Phase 4 · Scope（MoSCoW）

### 4.1 PM 骨架（教练态）

借 `Product-Manager-Skills/prioritization-advisor` 推荐的 MoSCoW（适合 MVP 范围决策）：

```
MUST HAVE（MVP 必有）：
- [story X] — 不做就证不了 hypothesis
- [story Y] — 不做产品根本跑不起来

SHOULD HAVE（强化版有）：
- [story Z] — 不做 MVP 不致命，但用户体验明显差

COULD HAVE（锦上添花）：
- [story W] — Phase 2 之后再说

WON'T HAVE（明确不做）：
- [story V] — 写下来就是为了拒绝引诱
```

**MUST HAVE 上限**：3-5 个。超过 5 个 = MVP 不是 minimum。

### 4.2 Jobs 视角检查

**检查问题**：「你的 MUST HAVE 列了 N 个——再砍 50%，剩下哪些？」

判断：
- **能砍 50% 还能 ship** → ✅ 把砍掉的移到 SHOULD，进 Phase 5
- **砍不动，每个都"必须"** → ❌ 退回。**'每个都必须' = 没 prioritize**
- **能砍但不愿砍**（"舍不得"） → ❌ 退回。**舍不得 = 平庸的来源**（Jobs Focus 心法）

**Jobs 戳法**：「Innovation is saying no to 1,000 things. If you can't say no to half your MUST list, you don't have a MUST list — you have a wish list.」

**振荡风险**：如果 Phase 4 反复退回 Phase 3 ≥ 3 次 → 红灯：split 粒度不对或 hypothesis 太大 → 回 Phase 1 或 C 段。

---

## Phase 5 · PRD（1 页精简 PRD）

### 5.1 PM 骨架（教练态）

借 `Product-Manager-Skills/prd-development` 的核心结构，但**砍掉 80%**——输出 **1.5 页**精简 PRD（不是传统 10 页 PRD）。

按 `template.md` 输出，包含：

1. **One-pager**：tagline + 1 段 problem + 1 段 solution
2. **Hypothesis**：从 Phase 1 搬
3. **Stories（MUST 列表）**：从 Phase 4 搬
4. **Acceptance Criteria（Gherkin）**：从 Phase 2 搬
5. **Out of scope（WON'T 列表）**：从 Phase 4 搬
6. **Sub-agent dispatch hints**：每个 MUST story 标注**适合哪类 sub-agent / agent 类型**执行（按用户先前要求"后面交给子 agent"）
7. **Risks & Open questions**：从 Frame Pack 的 Risks + Phase 1-4 中累积的疑问
8. **Definition of Done**：完整、verifiable 的 ship 条件

### 5.2 Jobs 视角检查

**检查问题**：「这 1 页——给一个工程师 / 一个老板 / 一个 sub-agent 看，**5 分钟内**他们都能 understand 要做什么吗？」

判断：
- **5 分钟内三方都能 understand** → ✅ HARD GATE 通过
- **超过 1.5 页** → ❌ 退回精简
- **工程师能但老板看不懂**（全是术语）→ ❌ 加 1 段 plain English summary
- **老板能但 sub-agent 看不懂**（缺执行细节）→ ❌ 加 sub-agent dispatch hints

**Jobs 戳法**：「A great PRD is 1 page that 3 audiences each get value from. A mediocre PRD is 10 pages that nobody reads.」

---

## Self-review · 5 通道扫描（HARD GATE 之前）

> 升级自原 4 项。借 MultiAgent (D:/whb/github/MultiAgent) 的 `spec_review.py` **5 通道确定性正则**思路——把"概念性扫描"换成"具体模式枚举"。
> agent 用"新眼光"扫一遍 PRD 草稿，**这是 agent 自检，不需要用户参与**——就地修，无需重新评审。

### 通道 1 · Placeholders（blocking）

扫这些**精确模式**，不止"感觉是 placeholder"：

| 模式 | 例 |
|------|---|
| `\bTBD\b` (case-insensitive) | "thresholds: TBD" |
| `\bTODO\b` (case-insensitive) | "// TODO: fill metric" |
| `\bXXX\b` (case-insensitive) | "owner: XXX" |
| `\?\?\?` 三个或更多问号 | "delay ≤ ??? ms" |
| 中文：`待补充` / `待定` / `暂时填` | "metric 待定" |
| 数字阈值缺失：acceptance 里 `≥ ?` / `≤ ?` / `?ms` / `?%` | "≥ ? 用户" |

**任一命中 → blocking**，退回相应 Phase 填具体值。

### 通道 2 · Weasel words / Vague verbs（blocking）

不仅查 weasel words，还要查**正面但不可验证的 vague verb**——这种最隐蔽：

| 类型 | 词列表 | 例 |
|------|------|---|
| **Weasel words**（不确定 hedging）| 可能 / 大概 / 或许 / 也许 / 尽量 / 应该 | "应该 ≤ 5 秒" |
| **Vague verbs in acceptance**（正面 but 不可验证）| 正常运行 / 良好 / 顺畅 / 流畅 / 完善 / 完美 / 稳定 / 可靠 / 易用 / 快速 | "Then 系统稳定运行" / "Then 用户体验流畅" |

**Vague verb 的特殊规则**：在 GWT 的 THEN 子句中**必须搭配数字 / 具体动作**，否则 = blocking。
例：
- ❌ "Then 系统稳定运行" → 退回
- ✅ "Then 系统响应 ≤ 200ms 且 5xx ≤ 0.1%" → 通过

注意 RFC 2119 关键字（SHALL / MUST / SHOULD）在英文 spec 里是**精确**的，**不算 weasel**。

### 通道 3 · Scope（advisory · 不 block 但记账）

| 检查 | 阈值（参考 MultiAgent 经验值）| 不通过怎么办 |
|------|------|-----------|
| PRD 总字符数 | > 10,000 字符 → warn | 建议拆子需求 / 砍 MUST |
| Requirement 数量 | > 10 个 → warn | 建议拆子需求 |
| MUST 列表 | > 5 个（D 段 Phase 4 硬规则）| 退回 Phase 4 砍 50% |
| 是否聚焦 1 个 hypothesis | 多 hypothesis 混在一起 | 退回 Phase 1 拆 |

### 通道 4 · Consistency（blocking）

| 检查 | 怎么检 | 不通过怎么办 |
|------|------|-----------|
| Requirement 标题重复 | 正则 `^### Requirement: (.+)$` 后 dedupe | 合并或改名 |
| 内部矛盾 | hypothesis ↔ stories ↔ MUST scope 数字一致吗？例：hypothesis "30 分钟" vs acceptance "≤ 5 秒" | 退回相应 Phase 拉平 |
| Frame ↔ PRD 引用不漏 | persona 名 / JTBD 句式 / tagline 应该在 PRD 出现 | 找出漏点，加进去 |

### 通道 5 · Anti-patterns（blocking · content-quality）

正则 + 模式扫描以下"**机械通道扫不到的内容质量问题**"：

| 反模式 | 怎么检 |
|------|------|
| **必填章节为空** | `## 不做的事 / Out of Scope` / `## 成功指标 / Success Metrics` / `## 背景与动机 / Strategic Context` 段缺失或仅写"待补充"/"TBD" |
| **OoS 偷懒** | "## 不做的事" body 仅写 "无" / "无。" / "None" / "- 无"（无任何说明） |
| **Success Metrics 无数字** | 段 body 不含任何 `\d`（ baseline / goal 不可量化）|
| **Scenario 缺 GWT** | `#### Scenario:` 块缺 WHEN 或 THEN 关键字 |
| **acceptance 写实现细节** | acceptance 出现"调用 X API" / "查询 Y 表" / "返回 JSON" 等技术细节 → 退回（应该是用户视角，不是实现） |

---

修完毛病再进 HARD GATE。

**实现提示**（如果 agent 在能跑代码的环境）：
- placeholder / weasel / scope / consistency 4 通道适合**确定性正则**实现（参考 `D:/whb/github/MultiAgent/backend/spec_review.py`）
- anti-patterns 通道也是正则 + 章节切分
- 5 通道里 1/2/4/5 是 blocking，3 是 advisory
- 自检不调用 LLM，**确定性 + 快速**，可以反复跑（每次 PRD 修改后自动重审）

---

## Visual Companion（可选 · 借自 superpowers/brainstorming）

如果 PRD 涉及视觉内容（用户主流程 wireframe / 数据流 / sub-agent dispatch 顺序图 / 状态机），agent **可以** offer visual companion。

**协议**：
- offer 是**独立消息**，不和 Phase 5 PRD 输出混在一起
- 用户可以同意 / 拒绝 / 暂时不需要
- 同意后 ≠ 整个 PRD 都用 visual——**逐节判断**：
  - 概念性（hypothesis / risks / DoD）→ 文字
  - 视觉性（user flow / story 拆解树 / sub-agent 派单依赖图）→ visual
- 视觉产物是 PRD 的**附录**，不是替代

**offer 模板**（在 Phase 5.1 PM 骨架完成后、5.2 Jobs 检查前给出，独立消息）：

> "PRD 即将输出。其中 user flow 和 sub-agent 派单依赖图可能用 ASCII 图表说明更直观。需要我做几张图作为 PRD 附录吗？或者纯文字也 OK——你定。"

---

## HARD GATE · 进 sub-agent / E 段的入门检查

<HARD-GATE>
5 phase + self-review 4 项 + (可选) visual companion 都跑完后必须自检。**任何一项不通过，禁止输出 PRD**——必须退回相应 Phase 或重做扫描。

| 检查项 | 不通过怎么办 |
|--------|-----------|
| Self-review 4 项扫描全部通过 | 退回扫描 |
| Phase 1 Jobs 检查通过（1 周可证伪） | 退回 |
| Phase 2 Jobs 检查通过（一句话讲完每个 story） | 退回 |
| Phase 3 Jobs 检查通过（每片独立价值） | 退回 |
| Phase 4 Jobs 检查通过（Must 砍 50% 后仍可 ship） | 退回 |
| Phase 5 Jobs 检查通过（1.5 页内 + 三方 5 分钟读完） | 退回 |
| 振荡 log：相邻 Phase 来回 < 3 次 + 总跳转 < 5 次 | 超过 = 回 C 段 |
| 5 件套 Frame ↔ 5 Phase 输出连贯（hypothesis 和 problem 一致、stories 和 JTBD 一致 …） | 任一断开 → 退回 |

**不允许的事**：
- 不允许跳过 self-review 直接进 HARD GATE
- 不允许 PRD 里留 "TBD"
- 不允许 acceptance 写成不可验证的话（"系统正常运行" / "用户体验流畅"）
- 不允许超过 1.5 页（超过 = 不是精简 PRD）
- 不允许在 Frame Pack 不全时硬跑 D 段

全部通过 → **输出 PRD + sub-agent dispatch hints + 落盘**。
</HARD-GATE>

---

## Sub-agent dispatch（这是 D 段的核心特色）

按用户要求："其他可以交给子 agent 或其他 agent 去执行"。每个 MUST story 在 PRD 里标 dispatch hints：

```yaml
story_id: S001
title: 写 yml schema + markdown parser
sub_agent_hints:
  type: code-writer  # 适合的 agent 类型
  primary_skill: superpowers/test-driven-development
  reference_skills:
    - "everything-claude-code:python-patterns"
    - "everything-claude-code:python-testing"
  context_files:
    - "D:/AI/code2/superpowers/skills/brainstorming/SKILL.md"  # 解析参考
  acceptance_check: "pytest 全过 + 覆盖率 ≥ 80%"
  estimated_complexity: medium  # low / medium / high
  human_review_gate: "merge 前看一眼 schema 设计"  # 哪步必须人工
```

这部分在 `template.md` 里有详细模板。

---

## 角色切换协议

和 audit / frame 一致：

- **进入 Phase 1 第 1 次需要 Jobs 检查时**显式宣告
- **每个 Phase 内部**：PM 教练态填空 → Jobs 视角戳 → 切回 PM 教练态修改
- **Phase 之间**：保持 PM 教练态主线
- **HARD GATE 通过后**显式切回（"退出 Steve Jobs 视角，PRD 完成"）

---

## Anti-Patterns

| 反模式 | 现场特征 | 纠正 |
|--------|---------|------|
| **跳 Phase** | 不写 hypothesis 直接写 stories | hypothesis 缺则 stories 没有 anchor |
| **PM 检查通过即放行** | 不做 Jobs 检查直接进下一 Phase | 每 Phase 必须过 Jobs 二元门 |
| **无视振荡 log** | 来回 5 次还硬撑 | 振荡上限是诊断信号，不是惩罚 |
| **PRD 膨胀到 5+ 页** | 想写得"完整" | 1 页 PRD 是设计选择不是限制 |
| **MUST 列表 8+ 个** | 砍不动还过 Phase 4 | 退回逼砍 |
| **故事写技术实现** | "I want to click button"  | 退回，要求用户层面动作 |
| **跳过 sub-agent hints** | 输出传统 PRD 不标 dispatch | 缺 dispatch = 不是本 skill 输出 |
| **绕过 HARD GATE 直接 ship** | 想"快速过" | HARD GATE 是流程门，绕过 = 流程白做 |

---

## 与现有 Skill 的关系

| Skill | 关系 |
|-------|------|
| `product-frame`（C 段） | **唯一上游**——必须基于 Frame Pack |
| `Product-Manager-Skills/epic-hypothesis` | Phase 1 PM 骨架 |
| `Product-Manager-Skills/lean-ux-canvas` | Phase 1 hypothesis 句式 |
| `Product-Manager-Skills/user-story` | Phase 2 PM 骨架 |
| `Product-Manager-Skills/user-story-splitting` | Phase 3 PM 骨架（8 patterns） |
| `Product-Manager-Skills/epic-breakdown-advisor` | Phase 3 也可调用其交互式 9 patterns |
| `Product-Manager-Skills/prioritization-advisor` | Phase 4 推荐 MoSCoW 框架 |
| `Product-Manager-Skills/prd-development` | Phase 5 PM 骨架（精简版） |
| `steve-jobs-skill` | 每 Phase .2 检查段借用 |
| `superpowers/writing-plans` | Phase 5 PRD 输出可以衔接到 superpowers 的 plan 文档系统 |
| `product-verdict`（E 段，待建） | 上线后回流到 E 段 |

---

## 诚实边界

1. **D 段不写代码**：输出 PRD + sub-agent hints，**不实现**。实现是 sub-agent 的事
2. **振荡是常态不是 bug**：你最高频切换的痛点区，振荡保护机制就是承认它
3. **1 页 PRD 不能 cover 所有**：Edge case / 复杂集成细节 / 长期 roadmap 不在 1 页里——这些是 sub-agent 实施时再 brainstorm 的事
4. **不替代 Frame**：如果 Frame Pack 不全或不准，硬写 PRD 出来的是垃圾——退回 C 段
5. **MoSCoW 不是唯一**：超过 30 个 backlog 时建议改用 RICE/ICE（调用 prioritization-advisor 重选框架）

---

## 后续衔接

```
product-spec 输出 PRD + sub-agent hints
   ↓
HARD GATE 通过
   ↓
两条路径：
  (a) 交给 sub-agent 执行（Task tool / 子 agent）→ 完成 → E 段
  (b) 用户自己实施 → 完成 → E 段 product-verdict
```

---

## 调用示例（缩略）

> 用户："frame 完了，写需求"
> 
> agent 读取 `docs/frames/2026-05-03-skill-fragment-remix.md`，5 件套齐 → 入口检查通过
> 
> **Phase 1**：写 hypothesis "We believe Alex（独立 AI 工具开发者）needs 片段级 skill 编译能力 in order to 不被 superpowers 多步流程绑架。Leading: 1 周内自己编出 1 个能用的混合 skill。Lagging: 1 月内 dogfood 周 ≥ 3 次。"
> Jobs 检查："1 周可证伪？" → ✓
> 
> **Phase 2**：epic story "As Alex, I want to write skill.config.yml declaring fragments, so I can compile a working SKILL.md without copy-paste". Detail stories 4 个。
> Jobs 检查："一句话讲完？" → ✓
> 
> **Phase 3**：split 4 个 detail story 成 7 个 1-3 工作日小 story。
> Jobs 检查："每片独立价值？" 其中 2 个不独立 → 退回 Phase 2 重写 ↕（振荡 #1）
> 
> 改完再过 → ✓
> 
> **Phase 4**：MoSCoW。MUST 列了 5 个 → Jobs："砍 50%。" 砍到 3 个 → ✓
> 
> **Phase 5**：1.5 页 PRD + 每 MUST story 标 sub-agent dispatch hints
> Jobs："5 分钟读完？" → ✓
> 
> HARD GATE 振荡 log = 1 次 ↕（< 3 次 ✓） 总跳转 1 次（< 5 次 ✓）
> → 输出 PRD 到 `docs/prds/2026-05-03-skill-fragment-remix.md`
> → 提示用户：可派给 sub-agent 实施了

---

**Ready to generate. 给我 Frame Pack 路径，或粘 5 件套 + audit Verdict。**
