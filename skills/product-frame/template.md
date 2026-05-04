# Product Frame Pack — [产品名]

> 日期：YYYY-MM-DD ｜ 上游 audit：`docs/audits/<name>.md` ｜ Verdict 引用：[amazing/refactor]
> 5 件套：persona + JTBD + problem + positioning + differentiation

---

## 1 · Persona Frame

**姓名/标签**：[一个具体的人或角色，例：Alex，36 岁独立 AI 工具开发者]

**一天里的 1 个真实瞬间**：
[他什么时候会想到你的产品？给一个具体场景]

**他现在的工具栈**：
[5-10 个工具，圈出和你产品相关的]

**他在乎什么**（按优先级 1→3）：
1. [...]
2. [...]
3. [...]

**他抗拒什么**（onboarding 时必须避免）：
- [...]
- [...]

**Jobs 检查记录**：
> 用户脱口而出的一句 persona：「[用户原话]」  
> ☐ 通过 / ☐ 退回过 N 次

---

## 2 · JTBD Frame

**主 JTBD**：

> When [触发情境]
> I want to [想做的事]
> So I can [更深层目标]

**相关 JTBD**（可选，1-2 个）：
> ...

**Jobs 检查记录**：
> 真实发生的瞬间数：[N 次]  
> 证据来源：[自己经历 / 朋友抱怨 / GitHub issue / 社区帖子]  
> ☐ 通过 / ☐ 退回过 N 次

---

## 3 · Problem Frame

**LOOK INWARD（往内看）**：
[问题在用户内部——工作方式 / 心智模型 / 限制]

**LOOK OUTWARD（往外看）**：
[问题被外部塑造——市场 / 技术 / 生态 / 组织约束]

**REFRAME（重新框定）**：
> 表面问题是：[A]  
> 真问题是：[B]  
> 啊瞬间：[这个 reframe 让你"啊"一下的点是什么]

**Jobs 检查记录**：
> 是问题不是方案：☐ 通过 / ☐ 退回过 N 次  
> Reframe 是真翻转不是改写：☐ 通过 / ☐ 退回过 N 次

---

## 4 · Positioning Frame

```
For    [target customer · 具体角色 + 1 句使用瞬间]
who    [JTBD 句式]
[X] is a [category · 已知产品类比]
that   [benefit · 可量化或可感知动作]
unlike [primary alternative · 真名 + URL]
       [our differentiator · 一个 · 用户能体感的瞬间]
```

**Tagline**（≤ 8 字）：
> 「[Jobs 检查的脱口而出版本]」

**Jobs 检查记录**：
> 1,000-songs 级：☐ 绿灯 / ☐ 黄灯（80 分待 3 个月内换）/ ☐ 红灯退回过 N 次  
> 如果黄灯，3 个月内目标版本：[暂定的更锋利候选]

---

## 5 · Differentiation Frame

### 真实对手坐标

**对手 1**：[真名 + URL]
- 他们对 JTBD 的解法：[...]
- 弱点（具体到 UX/价格/接入门槛）：[...]
- 你比他们强在：[...]

**对手 2**：[真名 + URL]
- 同上 3 项

**对手 3**（手工/现状/替代行为）：
- 用户不用产品时怎么解决：[...]
- 这个"不解决"为什么持续：[...]
- 你给他们的 switching cost：[...]

### 差异化定位

**两轴**：[X 轴定义] / [Y 轴定义]
**你占据**：[象限位置 + 一句话]

**Jobs 检查记录**：
> 对手 1 周内能否复制：☐ 不能（结构性壁垒）/ ⚠️ 6 个月内会复制 / ❌ 1 周能复制（功能差异化）  
> 如果非绿灯，写进 Risks

---

## Risks（5 件套之外的风险登记）

来自 audit 报告 + Frame 过程中发现的风险：

- **R1（来自 audit weakest link）**：[Anthropic / 平台 / 外部 API 等控制的关键环节]，应对：[...]
- **R2（来自 Step 5 黄/红灯）**：[差异化护城河风险]，应对：[...]
- **R3**：[Frame 过程中其他发现]

---

## 5 件套连贯性自检

| 链条 | 通过？ |
|------|--------|
| persona ↔ JTBD：是同一个人吗？ | ☐ |
| JTBD ↔ problem：JTBD 描述的痛点和 problem reframe 一致吗？ | ☐ |
| problem ↔ positioning：positioning 的 benefit 真的解决了 problem 吗？ | ☐ |
| positioning ↔ differentiation：differentiator 在对手坐标中真的是空白吗？ | ☐ |
| differentiation ↔ persona：你的 differentiator 是 persona 在乎的吗？ | ☐ |

任一条不通过 → 退回相应 Step 重做。

---

## 下一步

- [ ] **HARD GATE 通过** → 进入 D 段 `product-spec`
- [ ] **5 件套有断裂** → 退回相应 Step 重做
- [ ] **Frame 过程中推翻 Verdict** → 回 B 段 `product-audit` 重审
- [ ] **Frame 包路径**：`docs/frames/YYYY-MM-DD-<产品名>.md`

---

## 元数据

- 角色切换：本次 Frame 在每个 Step 的 .2 段进入 Steve Jobs 视角做检查（基于公开言论推断，非本人观点）；HARD GATE 通过后退出
- 退回次数（用于评估流程效率）：[Step 1: N / Step 2: N / Step 3: N / Step 4: N / Step 5: N]
- 总用时：[实际多少分钟]
- 重审建议：D 段写需求时若发现 Frame 不准，允许回 C 段重做相应 Step；Verdict 实质性变化时回 B 段
