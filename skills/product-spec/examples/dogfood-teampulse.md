# [DOGFOOD EXAMPLE] PRD — TeamPulse

> 这是 product-flow 5 段流程的 dogfood reference。**虚构场景**，演示 product-spec 的典型用法。
> 真实落盘到 `docs/prds/<日期>-<产品名>.md`。
>
> 演示要点：**5 phase + 2 次振荡**（Phase 3↔Phase 2、Phase 4↔Phase 3）+ **完整 sub-agent dispatch**

---

## 元数据

- 日期：2026-05-03 ｜ 上游 frame：`docs/frames/2026-05-03-teampulse.md` ｜ Verdict：refactor ｜ Tagline："会后 30 分钟派工"
- 总用时：~50 min（含 2 次振荡）

---

## Oscillation Log（振荡记录）

```
14:08 · Phase 1 → Phase 2（hypothesis 通过）
14:18 · Phase 2 → Phase 3（stories 通过）
14:32 · Phase 3 → Phase 2（reason: split 后 2/5 不独立——退回重写 stories）
14:42 · Phase 2 → Phase 3（重写完成）
14:50 · Phase 3 → Phase 4（split 通过）
14:58 · Phase 4 → Phase 3（reason: MUST 砍到 3 个后发现 split 粒度不对）
15:05 · Phase 3 → Phase 4（重 split 完成）
15:10 · Phase 4 → Phase 5（scope 通过）
15:18 · Phase 5 → 完成
```

**总跨 Phase 跳转数**：8 / 5 ⚠️
**最大相邻 Phase 来回数**：Phase 2↔3 = 2 次 (≤3 ✓)；Phase 3↔4 = 1 次 (≤3 ✓)
**HARD GATE 通过状态**：⚠️ 总跳转 = 8 略超 5 阈值，但每对相邻 ≤ 3 → **黄灯通过**，记账下次 Frame 时多花时间。

> **注意**：本例总跳转超阈值，触发了"诊断信号"——意味着上游 Frame 还可以更准。但相邻对内来回 ≤ 3，所以不强制退回 C 段，黄灯通过 + 记录在案。

---

## 1 · One-pager

**Tagline**：会后 30 分钟派工

**Problem**：5-15 人小团队的 EM 同时承担"主持 + 整理 + 派发 + 跟进"4 个角色，会议刚结束时正是注意力最分散的时候（要回邮件、做面试、看代码），整理任务被推迟到下班后甚至遗忘。市场把这个空档误读为"AI 摘要不够好"，但真问题是"主持人没 30 分钟空档"。

**Solution**：把"整理 + 派发"从 EM 身上拿掉——主持依然是人，事后整理 + action 派发 = AI 30 分钟内交付 markdown 派工清单。专注会后 30 分钟空档，懂团队 context（学过去会议术语、项目代码、成员名）。

---

## 2 · Hypothesis

**主 hypothesis**：

> We believe **EM 李工（5-15 人小团队管理者）**
> needs **会后 30 分钟内一份 markdown 派工清单**
> in order to **不再丢失 action、不再花 1 小时整理**

**Leading indicator**（1 周内可观察）：
- Dogfood：自己用 1 周后，主动用次数 ≥ 3 次/周
- 5 个 EM 朋友试用 → ≥ 3 人主动说"想保留下周接着用"

**Lagging indicator**（1-3 月可衡量）：
- 月度 action 派工时间从 ~2h → ≤ 30min
- 团队成员对 action 完成率（自评） +20%

**1-week falsification plan**（来自 audit Pass 3 PoL probe）：
- Probe type: Vibe-Coded
- Day 1-2：Markdown transcript parser + LLM prompt v1
- Day 3-5：sqlite memory（团队术语）+ 跑 5 个真实会议
- Day 6-7：自己 dogfood 1 周

---

## 3 · Stories — MUST HAVE（MVP 范围）

### S001 · 粘 transcript → 出 action list

**Story**：
> As 李工（EM）, I want to 把飞书会议 transcript 粘进 CLI/Web，2 分钟内拿到结构化 action list（谁做什么、何时完成），so that 我不用自己整理。

**Acceptance Criteria**：
```gherkin
Given transcript 是中文 + 长度 500-5000 字
When  用户粘进输入框 + 点 "提取"
Then  返回 markdown，每条 action 包含：负责人、动作、deadline（若提到）、原文出处行号
And   action 数 ≥ 1（如果会议确实有 action 的话）
And   总耗时 ≤ 2 分钟（含 LLM 延迟）
```

**Sub-agent dispatch hints**：
```yaml
type: code-writer
primary_skill: superpowers/test-driven-development
reference_skills:
  - everything-claude-code:python-patterns
  - everything-claude-code:python-testing
context_files:
  - "D:/AI/code/agent-skills/skills/jobs-to-be-done/SKILL.md"  # JTBD 句式参考
acceptance_check: "pytest 全过 + 5 个 sample transcript 全部产生 ≥ 1 action"
estimated_complexity: medium
human_review_gate: "LLM prompt v1 写完后人工 review 一次"
```

---

### S002 · 团队术语学习（memory）

**Story**：
> As 李工, I want to 在 markdown 里看到"老王 → 王思远（PM）"等术语解析, so that 团队成员能直接看懂派工清单。

**Acceptance Criteria**：
```gherkin
Given 用户已经粘过 ≥ 5 个该团队的会议 transcript
When  新会议 transcript 提到"老王" / "PM" / "那个客户"
Then  markdown 中自动注解为"老王（王思远 PM）" / "客户 = ABC Corp"
And   不在术语库的人名/术语保持原样 + 标 [需补充]
```

**Sub-agent dispatch hints**：
```yaml
type: code-writer
primary_skill: superpowers/test-driven-development
reference_skills:
  - everything-claude-code:python-patterns
  - everything-claude-code:swift-actor-persistence  # memory pattern 参考
context_files:
  - "D:/AI/code/hermes-agent/memory/*.py"  # hermes 的 memory 系统
acceptance_check: "5 个团队会议 dogfood 后，术语注解准确率 ≥ 70%（人工抽样）"
estimated_complexity: high
human_review_gate: "memory schema 设计 + 隐私边界"
```

---

### S003 · 一键派工 markdown（飞书 / Notion 友好）

**Story**：
> As 李工, I want to 把 action list 一键复制成飞书机器人友好 / Notion table 友好的格式, so that 我可以直接 forward 给团队不用二次编辑。

**Acceptance Criteria**：
```gherkin
Given action list 已生成
When  用户点 "复制为飞书格式" 或 "复制为 Notion table"
Then  剪贴板内容粘到飞书群直接成 @ + checkbox 列表
And   粘到 Notion 直接成 database table，含 owner / due / status 列
```

**Sub-agent dispatch hints**：
```yaml
type: code-writer
primary_skill: superpowers/test-driven-development
reference_skills:
  - everything-claude-code:frontend-patterns  # 如做 web UI
context_files:
  - "[飞书 markdown 规范文档]"
acceptance_check: "复制到飞书 + Notion 两个目标都正确显示"
estimated_complexity: low
human_review_gate: "无（可全自动）"
```

> **MUST 上限 3-5**：本例砍到 3 个 ✓

---

## 4 · Stories — SHOULD HAVE（强化版有，可选实现）

- **S004 · CLI 版本**：让我能在 terminal 里跑（懒得开浏览器）
- **S005 · 历史搜索**：在过去 4 周会议里搜"老王说要做的那件事"
- **S006 · 多 EM 协作**：团队多个 EM 共享 memory（Phase 2）

---

## 5 · Stories — COULD HAVE（锦上添花）

- **S007 · weekly digest**：周日晚上自动汇总本周所有未完成 action
- **S008 · 个性化报告**：给每个团队成员发"和你相关的 action"

---

## 6 · OUT OF SCOPE — WON'T HAVE

写下来就是为了拒绝引诱：

- **会议平台 API 集成（Zoom/腾讯/飞书）** —— 为什么不做：audit D2 决策，避免外部依赖。让用户粘
- **ASR 转录** —— 为什么不做：audit Q2 砍掉，让会议平台做
- **会议摘要** —— 为什么不做：差异化在 action，不在摘要
- **跨团队学习（公司知识图谱）** —— 为什么不做：Phase 1 单团队聚焦，Phase 2 之后再说
- **移动 App** —— 为什么不做：Phase 1 CLI/Web 够用，移动不是 EM 的会后场景

---

## 7 · Risks & Open Questions

| # | 风险 / 疑问 | 应对 / 决策 |
|---|----------|-----------|
| R1（audit weakest link） | LLM 提供方变 pricing / 限流 | 抽象 LLM 接口，支持 OpenAI / Claude / 国内大模型 3 选 1 |
| R2（Frame Step 5 黄灯） | Otter 推中文版 = 功能层被复制 | Phase 1 重投团队 context 学习；Phase 2 加跨会议历史搜索 + 多人协作 |
| R3（Frame 发现） | 用户抗拒"又一个 SaaS" | Phase 1 形态 = CLI + 简单 web UI（无登录）+ 飞书机器人 |
| Q1（Phase 1 振荡发现） | 用户的 transcript 隐私？memory 存在哪？ | Phase 1 默认本地 sqlite；Phase 2 视用户量再做云同步 |
| Q2 | LLM action 提取准确率多少算 ship？ | Dogfood 4 周后看自己接受率 ≥ 70% 即 ship |

---

## 8 · Definition of Done（DoD）

- [ ] **MUST** S001-S003 全部 acceptance 通过
- [ ] Leading indicator："Dogfood 周用次数 ≥ 3 次，连续 4 周"达标
- [ ] Sub-agent dispatch 全部完成且 human review gate 通过
- [ ] R1-R3 风险都有应对落地（不只是登记，要有具体代码 / 配置）
- [ ] kill-switch metric 1（自己用次数）已开始测量

---

## 9 · Sub-agent Dispatch Plan（聚合视图）

| Story | type | 适合派给 | 复杂度 | human gate |
|-------|------|--------|------|-----------|
| S001 | code-writer | python + LLM prompt 子 agent | medium | LLM prompt v1 review |
| S002 | code-writer | python + memory 设计子 agent | high | memory schema + 隐私边界 |
| S003 | code-writer | python + UI 集成子 agent | low | 无 |

**派单建议**：
1. **并行可行**：S001 + S003（不依赖 memory）
2. **必须串行**：S002 在 S001 完成后（依赖 LLM 接口已封装）
3. **人工守门**：S001 prompt v1 / S002 memory schema 设计

---

## 10 · 上游一致性自检

| 链条 | 通过？ |
|------|--------|
| Phase 1 hypothesis ↔ Frame Step 3 problem：解决"主持人没空档"问题 | ☑ |
| Phase 2 stories ↔ Frame Step 2 JTBD：会后 30 分钟拿到 markdown ↔ S001 | ☑ |
| Phase 4 MUST scope ↔ audit "留下的核心动作"：核心 = action 提取 = S001 ☑ | ☑ |
| Phase 5 PRD ↔ audit Verdict refactor 的 3 颗螺丝：D1（不做 ASR）✓ D2（让用户粘）✓ D3（团队 context = S002）✓ | ☑ |
| Sub-agent dispatch ↔ 用户工作模式：每个 MUST 都标了 sub-agent type | ☑ |

---

## 下一步

- [x] **HARD GATE 黄灯通过**（总跳转超阈值但相邻对内 ≤3，记账）
- [ ] **派单**：用 Task tool 把 S001 + S003 并行派给 code-writer，S002 等 S001 完成
- [ ] **PRD 路径**：`docs/prds/2026-05-03-teampulse.md`
- [ ] **完成实施后** → 进入 E 段 `product-verdict`（建议 4 周后跑第一次）

---

## 演示说明（看完这个 example 你应该 take away 什么）

1. **振荡是常态，但要记日志**——本例 8 次跳转看着多，但每对相邻 ≤ 3 是健康的
2. **超总阈值（5）但相邻健康 = 黄灯通过**——不强制退回，但记账。下次 Frame 多花时间能减少 D 段振荡
3. **Phase 3 退回 Phase 2 是经典场景**——split 后发现某些 story 不独立 = 上游 stories 没写好（不是 split 错）
4. **MUST 砍到 3 个不是教条**——本例真的从 5 个砍到 3 个，砍掉的 S004-S008 移到 SHOULD/COULD
5. **Sub-agent dispatch hints 是 D 段的核心交付**——不是传统 PRD，是给 Task tool / 子 agent 的派单清单
6. **OUT OF SCOPE 段保护你不被引诱**——写下来"WON'T 做会议平台 API"，写代码时就有底气说不
