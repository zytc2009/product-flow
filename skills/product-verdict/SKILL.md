---
name: product-verdict
description: |
  上线后周期性迭代审计：把现实 metric 与 audit 时设的 kill-switch 阈值对比，3 步出 Verdict（持守/调整/杀）。
  核心机制是 sunk cost 防御——同一方向连续 2 次 "再观察" → 第 3 次禁止再观察，强制变"杀"或"调整"。
  当用户说「上线后审一下」「这个产品该继续投吗」「kill-switch 触发了」「月度/季度回顾」「post-launch verdict」「该杀这个方向吗」时触发。
intent: >-
  五段产品流程的 E 段。在产品/功能上线后，周期性（月度/季度）或被 metric 触发地跑一次轻量迭代审计。
  比 B 段 audit 轻得多——不重审整个方向，只对比 kill-switch、诊断信号 vs noise、给三档 Verdict。
  解决"再观察一段时间"循环造成的沉没成本累积。
type: interactive
theme: post-launch-iteration
estimated_time: "10-15 min"
best_for:
  - "月度/季度对已上线产品做轻量审计"
  - "kill-switch metric 触及阈值时立即决策"
  - "对抗'舍不得杀方向'的 sunk cost bias"
scenarios:
  - "PRD 上线 1 个月了，跑一次回顾"
  - "metric 1 已经连续 2 周低于阈值，要不要杀"
  - "Anthropic 刚发布原生 skill 组合功能（外部触发），重审 verdict"
---

# Post-Launch Verdict · 周期性迭代审计

> 这是产品流程的 E 段。在 D 段 PRD 实施完、产品上线后跑。
> 不是再做一遍 audit——是对 audit 时设的 kill-switch 做**周期性比对**，逼出"持守 / 调整 / 杀"三选一。

## 触发条件（3 类）

| 触发类型 | 何时触发 | 执行频率 |
|---------|--------|--------|
| **时间触发** | 月度首日 / 季度首日 | 自动周期 |
| **Metric 触发** | audit 设的 kill-switch 任一项**首次触及阈值** | 立即跑 |
| **外部触发** | 竞品大动作 / 平台方发布原生功能 / 团队战略变化 | 立即跑 |

可以用 `cron` skill 设定时触发：
```
每月 1 日 09:00 → 自动启动 product-verdict
```

---

## 入口检查

读取 3 类输入：

| 输入 | 路径 | 缺了怎么办 |
|------|------|-----------|
| 上次 audit 报告（含 kill-switch） | `docs/audits/<name>.md` | 缺 = 没跑过 audit，回 B 段 |
| 上次 PRD（含 hypothesis + DoD） | `docs/prds/<name>.md` | 缺 = 没跑过 D 段，可以放过但标记"无 PRD baseline" |
| 现实 metrics + 用户反馈 | 用户提供（数据 csv / dashboard 截图 / 文字描述） | 缺 = 没法跑，停止并问用户 |
| **上次 product-verdict 报告**（如有） | `docs/verdicts/<name>-YYYY-MM.md` | 缺 = 第一次跑，无历史 |

---

## 总流程（3 步）

```
触发 → 入口检查
   ↓
Step 1 · Compare        现实 metric ↔ kill-switch 阈值（机械对比）
   ↓
Step 2 · Diagnose       Jobs 视角：真增长 vs noise / sunk cost 干扰检查
   ↓
Step 3 · Verdict        持守 / 调整 / 杀 三选一 + 下一步动作
   ↓
落盘 docs/verdicts/<name>-YYYY-MM.md
   ↓
（可选）如 Verdict = 调整 → 跳回 D 段 Phase 4 重 scope
       如 Verdict = 杀 → 回 A 段 product-spark 找新方向
```

---

## Step 1 · Compare（机械对比，**不评判**）

### 1.1 拉出 audit 时的 kill-switch

从上次 audit 报告读取 3 个 kill-switch metric 和阈值。例：

| # | Metric | 阈值 | 实际值 | 状态 |
|---|--------|------|------|------|
| 1 | Dogfood 周次数 | ≥ 3 次/周 | [用户填] | 🟢 / 🟡 / 🔴 |
| 2 | 第三方 try 人数 | ≥ 3/5 | [用户填] | 🟢 / 🟡 / 🔴 |
| 3 | Anthropic 入场窗口 | ≤ 6 个月内未发 | [用户填] | 🟢 / 🟡 / 🔴 |

**色标规则**：
- 🟢 绿：达标 +20%（明显超）
- 🟡 黄：±20% 范围（震荡区，noise 嫌疑）
- 🔴 红：触及阈值或低于 20%（明确未达）

### 1.2 让用户填实际值

```
"按你 audit 时设的 3 个 kill-switch，给我现实数据：

1. [Metric 1 描述]：当前实际是？
2. [Metric 2 描述]：当前实际是？
3. [Metric 3 描述]：当前实际是？

如果某个 metric 还没法测（数据不到位）→ 标 'no_data'，我会列入 Open Questions。"
```

### 1.3 输出对比表 + 1 段附加输入

附加输入：
- **新出现的用户反馈**（哪怕只 1 条具体反馈也比不写好）
- **新出现的竞品/平台动作**（外部触发的源信息）
- **团队/资源变化**（人员、预算、战略变了？）

**这一步只做对比和数据收集，不出判断**。

---

## Step 2 · Diagnose（Jobs 视角检查）

### 2.1 角色切换显式宣告

> 切到 Jobs 视角（首次进入显式宣告）：
> 「现在切到 Steve Jobs 视角做诊断。基于公开言论推断，非本人观点。」

### 2.2 三问诊断

**Q1 · 真增长还是 noise？**

> "看你 1.3 的数据——黄灯/绿灯是真增长还是 noise？给我证据：
> - 数据连续 2 周/月稳定吗？
> - 增长由 1 件具体事件驱动还是自然扩散？
> - 同样的 metric，1 周后还会是这个值吗？"

判断：
- **稳定 + 自然扩散** → 真信号 ✓
- **由单点事件驱动**（一篇博客 / 一次推荐） → noise，等下次 verdict 重看
- **数据波动 > ±30%** → 显然 noise，不能基于此 verdict

**Q2 · Sunk cost 检查（**E 段最关键问题**）**

> "查上次 verdict 是什么时候、是什么 Verdict？
> 如果连续 2 次 verdict = '再观察' / '持守' 但 metric 没改善 ——
> 你是基于数据持守，还是因为'已经投入这么多舍不得'？"

判断（**硬规则**）：
- **第 1 次"再观察"** → 允许，但记录在案
- **第 2 次"再观察"** → 黄灯，agent 明确警告："这是第 2 次说再观察，下次禁止"
- **第 3 次想说"再观察"** → **拒绝**，强制选 "调整" 或 "杀"

**Jobs 戳法**：「'Let's wait and see' is the most expensive sentence in product history. Either there's a signal that justifies waiting (and you can name it), or you're rationalizing sunk cost. Which one?」

**Q3 · 外部环境变没变？**

> "audit 时的关键假设——竞品 / 平台 / 技术拐点 / 用户群——还成立吗？
> 任何一个被颠覆 = audit Verdict 自动作废，必须重做（回 B 段）。"

判断：
- **环境稳定** → 继续 Step 3
- **环境关键变化**（如 Anthropic 发了原生 skill 组合）→ **跳过 Step 3，直接回 B 段重审**

---

## Step 3 · Verdict（三选一）

基于 Step 1 数据 + Step 2 诊断，agent **强制三选一**：

### 持守（Continue Investing）

**触发条件**：
- ≥ 2 个 kill-switch 🟢
- 0 个 🔴
- Step 2 三问全过

**意味着**：方向对、信号真、可继续投。**进入下一阶段计划**——比如 audit 时设的 Phase 2 roadmap 项（在 skill-fragment-remix 例子里就是"加跨框架输出"）。

**输出**：
- 下个月/季度的 1-2 个具体动作
- 下次 verdict 时间

### 调整（Refactor / Pivot Lite）

**触发条件**：
- 1-2 个 kill-switch 🟡 / 🔴，但其他 🟢
- 或 Step 2 Q1 暴露 noise 问题

**意味着**：骨架对，但某 feature/positioning 要换。不杀方向，只换零件。

**输出**：
- **明确指出哪个 feature 砍 / 改**
- **明确指出哪个 positioning 调**（参考 audit 的"必须换的 decision"）
- 调整后下次 verdict 时间

### 杀（Kill）

**触发条件**（满足任一即触发）：
- ≥ 2 个 kill-switch 🔴
- Step 2 Q2 第 3 次"再观察" → 强制变杀
- Step 2 Q3 外部环境关键变化（除非用户选回 B 段重审）
- core hypothesis 在 leading indicator 上 6 周内不动

**意味着**：方向死了。停止投入。**杀不丢人**——是给资源去做下一个方向。

**输出**：
- 明确 kill 的具体动作（停止开发 / 关掉 production / 通知用户）
- 学到的 3 件事（这次方向死了，但要带走 lessons）
- 推荐回 A 段 product-spark 找新方向

**Jobs 戳法**：「Killing a project is the highest-status decision a leader can make. Anyone can keep something alive by inertia. It takes courage to call time of death and free up resources for the next thing.」

---

## 角色切换协议

和前面 B/C/D 段一致：

- **进入 Step 2 第 1 次需要 Jobs 视角时**显式宣告
- **Step 1 / Step 3 是 PM 教练态**（Step 3 给 Verdict 时回到 PM 中性语气，避免 Jobs 戏剧化）
- **本 skill 通常用时短（10-15 min）**，角色切换比 audit 简单

---

## 落盘

输出到：

```
docs/verdicts/<name>-YYYY-MM.md
```

按月份/触发时间命名。**保留所有历史 verdict**——这是 sunk cost 防御机制的依据（agent 下次跑时要看上 N 次的 Verdict 类型）。

历史索引文件：
```
docs/verdicts/<name>-history.md
```

每行一条：
```
2026-05-03 | 月度 | 持守 | 下次 2026-06-01
2026-06-01 | 月度 | 持守 | 下次 2026-07-01
2026-07-01 | metric | 调整 | 砍掉 feature X，下次 2026-08-01
2026-08-01 | 月度 | 持守 | 下次 2026-09-01
2026-09-01 | 月度 | 持守 | 下次 2026-10-01  ← 第 N 次再观察 / 持守，agent 标黄
```

---

## Anti-Patterns

| 反模式 | 现场特征 | 纠正 |
|--------|---------|------|
| **跳过 Compare 直接给 Verdict** | "感觉还行，持守" | 必须有数据，没数据不出 Verdict |
| **没看 audit kill-switch** | 用新拍脑袋的 metric 评估 | 强制对照 audit 时设的阈值 |
| **第 3 次"再观察"** | "再给它一个月" | 硬规则禁止，强制选调整/杀 |
| **杀的时候说"暂停"** | 用模糊词回避决策 | 杀就说杀。"暂停"会变成永久占坑 |
| **持守却不计划下一步** | "继续做"——做什么不说 | 持守必须配 1-2 个具体动作 |
| **Verdict 后不落盘** | "知道了就行" | 不落盘 = sunk cost 防御失效 |
| **环境变了还跑 Step 3** | Anthropic 出了原生功能但还在算 dogfood 次数 | Step 2 Q3 检查环境，关键变化直接回 B 段 |

---

## 与现有 Skill 的关系

| Skill | 关系 |
|-------|------|
| `product-audit`（B 段） | **唯一上游**——E 段对比的就是 B 段设的 kill-switch；环境关键变化时回 B 段重审 |
| `product-spec`（D 段） | 提供 PRD baseline，包含 hypothesis + DoD |
| `Product-Manager-Skills/business-health-diagnostic` | 如果是 SaaS 产品，Step 1 可调用其健康度框架补充 metrics |
| `Product-Manager-Skills/saas-revenue-growth-metrics` | SaaS 产品的 metric 选择参考 |
| `Product-Manager-Skills/saas-economics-efficiency-metrics` | 同上 |
| `Product-Manager-Skills/feature-investment-advisor` | Verdict = 调整时，决定砍哪个 feature 可调用 |
| `steve-jobs-skill` | Step 2 三问借用其表达 DNA + 二元判断 |
| `cron` 系统 skill | 时间触发的自动化 |
| `product-spark`（A 段） | Verdict = 杀时回到这里找新方向 |

---

## 诚实边界

1. **E 段不替代 audit**：定期跑的是轻量比对，方向/竞品/技术拐点的根本性变化必须回 B 段
2. **Sunk cost 防御不能完美**：硬规则只能堵住"明显的舍不得"，无法防止用户用其他借口（"换个 metric 再观察"）。需要用户自己有诚实
3. **数据不到位时**：诚实标 `no_data` 比凭感觉填数字强。下次 verdict 时优先确认数据采集
4. **杀方向不等于失败**：把"杀"和"失败"绑定 = 永远不杀。本 skill 把"杀"重新定义为"释放资源去做下一个"

---

## 后续衔接

```
product-verdict 输出
   ├─ 持守 → 落盘，等下次触发
   ├─ 调整 → 落盘，回 D 段 Phase 4 重 scope（具体砍/换）
   └─ 杀   → 落盘，回 A 段 product-spark 找新方向
                + 学到的 3 件事写进 docs/lessons/<name>.md
```

---

## 调用示例（缩略）

> 触发：月度 cron 触发，2026-06-01

> agent 读取：
>   - audit kill-switch（3 个）
>   - 上次 PRD（hypothesis + DoD）
>   - 上次 verdict（5 月 → 持守）→ 这是第 1 次"持守"
> 
> **Step 1 Compare**：让用户填 3 个 metric 现实值
>   - Metric 1 (dogfood 周 3 次)：实际 4 次/周 🟢
>   - Metric 2 (3rd party try)：2/5 🟡
>   - Metric 3 (Anthropic)：未发布 🟢
>   - 附加：用户反馈 1 条"yml 写起来不直观，能不能交互式生成"
> 
> **Step 2 Diagnose**（Jobs 视角）：
>   - Q1 真增长：dogfood 4 次稳定 2 周 → 真信号 ✓
>   - Q2 sunk cost：上次持守 1 次，本次允许持守
>   - Q3 环境：Anthropic 没大动作 ✓
> 
> **Step 3 Verdict**：**持守**
>   - 下个月动作：增加交互式 yml 生成器 (响应 Metric 2 黄灯 + 用户反馈)
>   - 下次 verdict：2026-07-01
> 
> 落盘 `docs/verdicts/skill-fragment-remix-2026-06.md`
> 历史 index 加一行

---

**Ready to verdict. 给我 audit 报告路径 + 现实 metrics（按 audit 设的 3 个 kill-switch 一项一项报）。**
