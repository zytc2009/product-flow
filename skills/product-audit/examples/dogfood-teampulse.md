# [DOGFOOD EXAMPLE] Product Direction Audit — TeamPulse · 会议→action 自动化

> 这是 product-flow 5 段流程的 dogfood reference。**虚构场景**，演示 product-audit 的典型用法。
> 真实落盘到 `docs/audits/<日期>-<产品名>.md`。
>
> 演示要点：**Verdict = refactor + 3 颗螺丝**（关键环节 ASR / 会议平台 API 都在外人手上）

---

## 元数据

- 日期：2026-05-03 ｜ 审计人：用户 ｜ 调研时间窗：2026-05-03
- 上游 spark：`docs/sparks/2026-05-03-teampulse.md`（候选 1）

## Verdict

**REFACTOR** —— 骨架对，3 颗螺丝必须拧

需求真实（5 个想法 cluster 后还能稳定收敛到一个主线）+ 差异化锚定（懂中文 + 懂团队 context）+ 调研显示有真实空白（中文+中小团队场景）。但**关键环节 ASR / 会议平台 API / 用户 task 系统全部在外人手上**——产品形态必须 reframe。

---

## Pass 1 · Scaffold

```
For    5-15 人小团队的 EM / Tech Lead，每周开 3-5 个会议、需要派 action 给团队
who    when 周一晨会结束后 30 分钟内,
       I want to 拿到一份谁要做什么的 markdown,
       so I can 把 action 派给团队不用挨个问
TeamPulse is a **会议→action 自动化管道**
       —— 类比："你的会议秘书，但只关心 action 和 follow-up"
that   让 EM 30 分钟内从会议 transcript 拿到可派工的 action list
       （不要 80% 摘要，只要 20% 可执行项）
unlike Otter (otter.ai) / Fireflies (fireflies.ai) —— 英文为主、不懂团队 context
       和"不做总结忍着"
       differentiator: 懂中文 + 懂团队 context（学过去项目代码 + 历史会议术语）
```

---

## Pass 2 · Stress-test（Steve Jobs 视角）

> 进入 Pass 2 时显式宣告：「现在切到 Steve Jobs 视角，会比较直接，不 hedging。基于公开言论推断，非本人观点。」

### Step 2.1 · 调研内部摘要（**磁盘优先**）

调研源优先级：磁盘 > URL > Web。

**磁盘调研**：
- `D:/AI/code/agent-skills` 中 PM-Skills 没有专门会议工具
- `D:/AI/code/hermes-agent` 有 memory + cron 但没有会议总结模块
- 用户磁盘上确认空白

**Web 调研**（Otter / Fireflies / tldv）：
- Otter：英文为主，中文准确率仅 70%
- Fireflies：集成多但 action 提取依赖关键词触发（"action item:"），不够智能
- tldv：摘要为主，action 提取弱
- 国内：腾讯会议 / 飞书自带 AI 总结但**通用化、不懂团队**

**品类拥挤度**：英文+通用赛道**拥挤**（5+ 同质），中文+中小团队赛道**有空白**

**技术拐点**（最近 12 个月）：
- 中文 ASR 准确率因新模型显著提升（≥ 95%）
- 长 context LLM 让"学过去 1 年会议"成为可能
- Agent + memory 让团队 context 学习从天 → 周 缩短

### Step 2.2 · 三问三砍

**Q1 · 一句话定义**

用户答："**会议→action 30 分钟**"（8 字）→ ✅ 通过

Jobs 评：「这是个能脱口而出的句子，但不性感。'30 分钟'够具体，'会议→action'够直白。 amazing 级会更短，比如'你团队的会议秘书' —— 但 80 分够 ship。**3 个月后换。**」

**Q2 · 砍 80%**

用户列了 7 个想做的功能：
1. 自动转录 ASR
2. 摘要生成
3. action 提取
4. action 派给团队成员
5. 跨会议搜索
6. weekly report 自动生成
7. 团队术语学习

砍 80% 留 1 个 → 用户选 **#3 action 提取**（从 transcript 出发，不做 ASR、不做摘要）

Jobs 评：「砍得果断。**ASR 不是你的事**——这是 Otter / 腾讯会议 / 飞书的事，你做不过他们。摘要也不是你的事——LLM 直接生成的摘要满地都是。'action 提取'是你能赢的细分，因为它需要懂 context。」

**Q3 · 体验链谁控制**（**最尖锐的一题**）

| # | 步骤 | 谁控制 |
|---|------|------|
| 1 | 用户**得知**有这工具 | 社区 |
| 2 | 用户**安装** | 用户自己 |
| 3 | 用户**导入会议 transcript** | **会议平台 API** ❗（Zoom/腾讯/飞书/Google Meet）|
| 4 | 转 transcript（如未转） | **ASR 服务**（OpenAI Whisper / Azure / 阿里）❗ |
| 5 | LLM 提取 action | **OpenAI / Anthropic / 国内大模型** ❗ |
| 6 | 派工到 task 系统 | **task 系统**（Notion / Lark / Jira / Trello）❗ |
| 7 | 团队成员看 + 执行 | 用户自己 + 团队 |

🔴 **被外部控制的关键环节**：步骤 3, 4, 5, 6——**4 个外部接口**！

Jobs 评：「Whoa. 你的产品在 4 个外人的肩膀上跳舞。任何一家断你 API、改 pricing、出原生竞品，你都 dead。」

### Step 2.3 · Verdict

**REFACTOR** —— 骨架对，但 3 颗螺丝必须换：

#### D1 · 产品形态：**不做 ASR，专注 transcript 后处理**
- 输入：用户粘 transcript（让 Otter / 腾讯 / 飞书帮你做 ASR，免费 + 高准确率）
- 输出：action list（你专注做这一段）
- **去掉**：录音上传、实时转录、会议平台直接集成
- **集成模式**：CLI / VSCode 插件 / 简单 web UI

#### D2 · 集成点：**让用户粘，不做 API 集成**
- Phase 1 不做 Zoom/腾讯/飞书 API（节省 6 个月开发，避免被断 API）
- Phase 2 视用户量再决定做哪个平台
- **代价**：用户多 1 步粘贴动作 → 但去掉了 4 个外部依赖

#### D3 · 护城河：**团队 context 学习曲线 = 时间壁垒**
- 功能层（action 提取）任何人 1 周做出来
- **真护城河**：你这个团队连续用了 6 个月，AI 学会了"老王" "那件事" "上次说的方案" 都指什么——**新工具没法直接复制 6 个月的团队记忆**
- Phase 1 优先做记忆 + 学习机制，**不优先**做漂亮 UI

---

## Pass 3 · Falsify

> 退出 Steve Jobs 视角，切回结构化模式。

### 推荐 1 个 PoL probe：**Vibe-Coded**

**1-week plan**：
- Day 1-2：写 Markdown transcript parser + LLM action 提取 prompt
- Day 3-5：加 memory（用 obsidian 或 sqlite 存团队术语）+ 跑 5 个真实会议 transcript
- Day 6-7：自己用 1 周（dogfood）：每周开 3 个会议都用它，看是不是真用得起来

预期成本：~12 工时，纯个人；零金钱投入

### 3 个 kill-switch metric

| # | Metric | 阈值 | 测量方式 |
|---|--------|------|---------|
| 1 | **Dogfood 周用次数**（自己 EM 角色） | ≥ 3 次/周（连续 4 周） | 自己日记 + git log |
| 2 | **第三方真实意愿**：5 个 EM 同行试用 → ≥ 3 实际用过 | ≥ 3/5 | 在 PM 社群 / Twitter 发 demo + 收 feedback |
| 3 | **Otter 中文版风险**：Otter 推出中文 + team context | ≤ 6 个月内未发 | 监控 Otter blog / changelog |

---

## What I'd cut（来自 Pass 2 Q2）

砍掉的功能：
- **ASR 转录** —— 让会议平台做，不和 Otter / 腾讯 / 飞书竞争
- **摘要生成** —— LLM 直接做的事，没差异化
- **跨会议搜索** —— 候选 2 的范畴（线 2），Phase 1 不做
- **weekly report 自动生成** —— Phase 2 之后

留下的核心动作：**1 个**——从粘进来的 transcript 提取 action items（带团队 context）

---

## Weakest link in the chain

🔴 步骤 3, 4, 5, 6 都是外部依赖（4 个外部接口）——这是体验天花板

**应对方案（D1+D2）**：
- 把 ASR / 会议平台 API 整体砍掉（Decision 1+2）
- 输入降级为"用户手工粘 transcript"——失去无缝体验，换来零外部依赖
- LLM 这一层接受外部依赖（市场都依赖，无法回避）
- task 系统这一层 Phase 1 不集成（只输出 markdown，让用户自己复制到 Notion / Lark / Jira）

---

## Next step

- [x] Verdict = REFACTOR
- [ ] **应用 D1**：产品形态 reframe 为"transcript 后处理工具"
- [ ] **应用 D2**：Phase 1 不做平台 API，让用户粘
- [ ] **应用 D3**：Phase 1 投入团队 context 学习机制（不投漂亮 UI）
- [ ] 跑 Vibe-Coded probe（1 周）→ 4 周后看 kill-switch metric 1
- [ ] **进入 C 段 product-frame** 展开 5 件套

---

## 演示说明（看完这个 example 你应该 take away 什么）

1. **磁盘优先调研**很关键——本例先看了 hermes-agent / agent-skills 确认磁盘没同类，然后才查 web 竞品
2. **Verdict 三档强制**——本例不允许说"good"或"有潜力"，refactor 就明确指出 3 颗螺丝
3. **D2 决定（让用户粘 vs 做 API）**是经典 tradeoff——失去无缝体验换来零外部依赖。Audit 强迫你做这个 tradeoff
4. **kill-switch 必须可证伪**——本例 3 个 metric 全部带阈值 + 测量方式
5. **Vibe-Coded probe 是 dogfood**——你自己每周用，比"5 个朋友 try"更早给出信号
