# Post-Launch Verdict — [产品名] · [YYYY-MM 或触发时刻]

> 日期：YYYY-MM-DD ｜ 触发类型：[时间 / metric / 外部] ｜ 第 N 次 verdict
> 上游 audit：`docs/audits/<name>.md` ｜ 上游 PRD：`docs/prds/<name>.md`
> 上次 verdict：`docs/verdicts/<name>-YYYY-MM.md`（第 N-1 次：[Verdict]）

---

## Step 1 · Compare（kill-switch 对比）

### 1.1 audit 设的 kill-switch ↔ 现实 metric

| # | Metric | 阈值 | 现实值 | 色标 |
|---|--------|------|------|------|
| 1 | [audit Pass 3 设的 metric 1] | [阈值] | [现实值或 no_data] | 🟢/🟡/🔴 |
| 2 | [metric 2] | [...] | [...] | 🟢/🟡/🔴 |
| 3 | [metric 3] | [...] | [...] | 🟢/🟡/🔴 |

**色标规则**：
- 🟢 达标 +20% 以上
- 🟡 ±20% 震荡区
- 🔴 触及阈值或低于 20%

### 1.2 附加输入

**新出现的用户反馈**（1 条具体反馈优于"听起来 ok"）：
- [...]

**新出现的竞品/平台动作**：
- [...]

**团队 / 资源 / 战略变化**：
- [...]

---

## Step 2 · Diagnose（Jobs 视角三问）

### Q1 · 真增长还是 noise？

- 数据连续稳定 [X 周/月]：☐ 是 / ☐ 否
- 由单点事件驱动（博客 / 推荐 / 偶然）：☐ 是（noise 嫌疑） / ☐ 否
- 1 周后还会是这个值吗？：☐ 是 / ☐ 不一定 / ☐ 不会

**判断**：☐ 真信号 / ☐ noise / ☐ 数据不足

### Q2 · Sunk cost 检查（**关键**）

历史 verdict 类型回溯（从历史 index 拉）：

| 时间 | Verdict | 说"再观察"或"持守"? |
|------|---------|------------------|
| 第 N-2 次 | [...] | ☐ |
| 第 N-1 次 | [...] | ☐ |
| 本次本能想选 | [...] | ☐ |

**硬规则触发**：
- ☐ 第 1 次"再观察 / 持守" → 允许
- ☐ 第 2 次"再观察 / 持守" → 黄灯，记录警告
- ☐ **第 3 次想"再观察 / 持守" → 拒绝，强制变 "调整" 或 "杀"**

**用户回答**："是基于具体数据信号持守，还是因为已经投入舍不得？"
> [用户原话]

### Q3 · 外部环境检查

audit 时的关键假设：

| 假设 | 当前是否成立？ |
|------|------------|
| [audit 假设的竞品格局] | ☐ 成立 / ☐ 关键变化 |
| [audit 假设的技术拐点] | ☐ 成立 / ☐ 关键变化 |
| [audit 假设的用户群] | ☐ 成立 / ☐ 关键变化 |
| [audit 假设的平台 / 生态] | ☐ 成立 / ☐ 关键变化 |

**任一关键变化** → 跳过 Step 3，直接回 B 段重审 audit

---

## Step 3 · Verdict（强制三选一）

```
☐ 持守（Continue Investing）
☐ 调整（Refactor / Pivot Lite）
☐ 杀（Kill）
```

### 持守（如果选）

**触发条件**：≥ 2 🟢 + 0 🔴 + Step 2 三问全过 + sunk cost 防御未触发

**下个周期具体动作**（必须 1-2 个，不是"继续做"）：
1. [...]
2. [...]

**下次 verdict 时间**：[YYYY-MM-DD]

### 调整（如果选）

**触发条件**：1-2 🟡/🔴，其他 🟢；或 Q1 暴露 noise

**砍掉的 feature**：
- [feature X] —— 理由：[...]

**改的 positioning**：
- [from] → [to] —— 理由：[...]

**保留的核心动作**（来自 audit "What I'd cut" 段的留下项）：
- [...]

**调整后跳回**：☐ D 段 Phase 4（重 scope） / ☐ D 段 Phase 2（重写 stories）

**下次 verdict 时间**：[调整 1-2 周后]

### 杀（如果选）

**触发条件**：≥ 2 🔴 / 第 3 次想再观察 / 6 周 leading indicator 不动

**杀的具体动作**：
- ☐ 停止开发
- ☐ 关闭 production / 撤回发布
- ☐ 通知用户（如有）
- ☐ 归档代码 + 文档

**学到的 3 件事**（带走的 lessons，避免下次重蹈）：
1. [...]
2. [...]
3. [...]

**下一步**：☐ 回 A 段 product-spark 找新方向 / ☐ 暂停产品工作休整

---

## 4 · Risk register 更新

来自 audit Risks + 本次 verdict 新发现：

| # | 风险 | 状态变化 | 应对 |
|---|------|--------|------|
| R1 | [audit weakest link] | ☐ 已发生 / ☐ 仍潜在 / ☐ 已消除 | [...] |
| R2 | [audit differentiation 风险] | [...] | [...] |
| R3 | [本次新发现] | 新增 | [...] |

---

## 5 · 历史 index 更新

把这一行追加到 `docs/verdicts/<name>-history.md`：

```
[YYYY-MM-DD] | [触发类型] | [Verdict] | 下次 [YYYY-MM-DD]
```

---

## 元数据

- 角色切换：Step 2 进入 Steve Jobs 视角（基于公开言论推断，非本人观点）；Step 3 切回 PM 中性语气出 Verdict
- 本次用时：[X min]
- Sunk cost 防御触发？：☐ 否 / ☐ 是（说明：[...]）
- 数据完整度：[3/3 metric / 2/3 metric (1 项 no_data) / ...]

---

## 下一步

- [ ] **Verdict 已落盘**：`docs/verdicts/<name>-YYYY-MM.md`
- [ ] **历史 index 已更新**：`docs/verdicts/<name>-history.md`
- [ ] **如 Verdict = 持守** → 等下次触发
- [ ] **如 Verdict = 调整** → 跳回 D 段 / 实施调整
- [ ] **如 Verdict = 杀** → 落盘 lessons → 回 A 段 product-spark
- [ ] **如 Step 2 Q3 关键变化** → 回 B 段重审 audit
