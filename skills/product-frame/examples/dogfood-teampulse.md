# [DOGFOOD EXAMPLE] Product Frame Pack — TeamPulse

> 这是 product-flow 5 段流程的 dogfood reference。**虚构场景**，演示 product-frame 的典型用法。
> 真实落盘到 `docs/frames/<日期>-<产品名>.md`。
>
> 演示要点：**5 件套 + Step 3 Problem 退回 1 次**（用户写成方案被退回）

---

## 元数据

- 日期：2026-05-03 ｜ 上游 audit：`docs/audits/2026-05-03-teampulse.md` ｜ Verdict：refactor
- 退回次数：Step 3 退回 1 次（Problem 写成方案）
- 总用时：~25 min

---

## 1 · Persona Frame

**姓名/标签**：李工 · 36 岁产品技术 EM · 7 人团队 · Tencent 出来 3 年 · 现独立创业团队

**一天里的 1 个真实瞬间**：
> 周一 10:00 站会刚开完，他打开飞书会议录像，开始记笔记。但马上有客户邮件要回、有面试要做、有 PR 要 review——笔记写了一半就放下了。下午 4:00 想起来"今天会上王哥说要做啥来着？"，翻笔记没翻到。

**他现在的工具栈**：
- 飞书（会议 + 文档 + IM）✓ 相关
- 自己写的 weekly note Notion 模板 ✓ 相关
- Linear（任务追踪）✓ 相关
- Cursor（写代码）
- ChatGPT（偶尔做 brainstorm）

**他在乎什么**（按优先级）：
1. **团队不被会议拖死**——开会时间已经够多了，不要再花 1 小时整理
2. **action 真的执行**——开完会就忘 = 等于没开
3. **保持节奏感**——周一应该清晰看到这周要干嘛

**他抗拒什么**：
- 又一个要登录的 SaaS 工具（已经 8 个 SaaS 账号了）
- 看不懂数据怎么"被学习"的（隐私焦虑）

**Jobs 检查记录**：
> 用户脱口而出：「李工，36 岁 EM，周一开完会下午就忘了王哥说要做啥」 → ✅ 通过

---

## 2 · JTBD Frame

**主 JTBD**：

> **When** 周一晨会刚结束（11:00 之前），
> **I want to** 拿到一份"谁要做什么"的 markdown，
> **So I can** 把 action 派给团队成员、不用一个个去问 / 自己写笔记。

**相关 JTBD**：
> When 周中突然想起"上次会议老王说要做什么"，
> I want to 在 5 秒内查到，
> So I can 不打断他、自己确认进度。

**Jobs 检查记录**：
> 真实发生瞬间数：**自己 12 周里 11 次都中招**（自己 EM 角色）+ Twitter EM 群里 3 个朋友说"对对对" 
> 证据来源：自己日记 + 朋友抱怨
> ☑ 通过

---

## 3 · Problem Frame（**演示退回**）

### 第 1 次尝试（被 Jobs 退回）

用户写：

> **LOOK INWARD**：用户没有自动化工具帮他从 transcript 提取 action  
> **LOOK OUTWARD**：现有工具（Otter）是英文的、不懂中文 + 团队 context  
> **REFRAME**：问题是缺一个懂团队 context 的中文 action 提取工具

**Jobs 视角戳**：
> 「Stop. 你写的 LOOK INWARD = '用户没有自动化工具'——**这是方案描述，不是问题描述**。'缺工具' 不是问题，'丢失 action' 才是问题。退回。」

### 第 2 次尝试（通过）

**LOOK INWARD（往内看）**：
> EM 同时承担"主持会议 + 整理会议 + 派发 action + 跟进执行"4 个角色——会议刚结束时正是他注意力最分散的时候（要回邮件、做面试、看代码），整理任务被持续推迟，最终丢失。

**LOOK OUTWARD（往外看）**：
> 现有 SaaS 工具（Otter / Fireflies）是为英文 + 大团队设计的——它们追求"全套会议生命周期"，但中小团队 EM 的真痛点是更窄的"会后 30 分钟的注意力空档"。市场把这个空档误读为"摘要需求"，但其实是"action 派发"需求。

**REFRAME**：

> 表面问题：**"AI 会议工具还不够好"**  
> 真问题：**"主持会议的人就是会后整理的人，但他没有 30 分钟空档"**  
> **啊瞬间**：解决方案不是"做更好的 AI 总结"，是"把整理的人换成 AI"——主持依然是人，但整理 + 派发 = AI 立刻出结果。

**Jobs 视角戳**（第 2 次）：
> 「Now you're talking. 这是问题，不是方案。Reframe 也是真翻转——从'工具不够好'翻到'人没空档'。✅ 通过。」

**Jobs 检查记录**：
> 是问题不是方案：☑ 通过（退回 1 次）  
> Reframe 是真翻转：☑ 通过

---

## 4 · Positioning Frame

```
For    5-15 人小团队的 EM / Tech Lead，每周一晨会后注意力被多线程拉扯
who    when 周一晨会刚结束（11:00 之前）,
       I want to 拿到一份"谁要做什么"的 markdown,
       so I can 把 action 派给团队成员、不用挨个问

TeamPulse is a **会后 30 分钟的 AI 整理人**
       —— 类比：你的会议秘书，但只在会后 30 分钟里高强度工作

that   把"整理 + 派发 action" 这一段从你身上拿掉
       （主持会议依然是你，事后整理 = AI 30 分钟内交付）
       
unlike Otter / Fireflies / 飞书自带 AI
       它们是"全周期工具"——录音、转录、摘要、搜索都做
       
       differentiator: **专注会后 30 分钟空档** + **懂团队 context**
       —— 用户体感：每周一 10:30 收到一份"派工清单"，直接 forward 给团队
```

**Tagline**（≤ 8 字）：

> 「**会后 30 分钟派工**」（8 字，可朗读）

**Jobs 视角戳**：
> 「OK. 8 字。'会后 30 分钟派工' 比 audit 时的 '会议→action 30 分钟' 进了一档——加了'派工'这个动词，明确了产品价值。✅ 绿灯通过。」

---

## 5 · Differentiation Frame

### 真实对手坐标

**对手 1**：Otter (otter.ai)
- 解法：录音 → 全套转录 + 摘要 + action（关键词触发）
- 弱点：英文 only、中文准确率 70%、action 提取靠"action item:" 关键词识别（实际会议很少这么说）
- 你比他们强：中文优先 + 团队术语学习 + 不依赖关键词触发

**对手 2**：飞书 / 腾讯会议自带 AI
- 解法：会议结束后自动出摘要 + 关键决议
- 弱点：通用化、不懂特定团队（"老王" / "那件事" / 项目术语全部失败）、action 提取浅
- 你比他们强：你为这个团队学习 6 个月后效果指数级好

**对手 3**：自己手工做总结 / 不做总结忍着
- 现在：开完会暂时不写、晚上回家累得不写、第二天忘了
- 持续原因：从来没省下过 30 分钟空档
- switching cost：你给的不是"另一个工具"，是"30 分钟空档归还"

### 差异化定位

**两轴**：
- X 轴：通用 ←→ 团队定制
- Y 轴：全周期 ←→ 30 分钟空档

**你占据**：右下象限（团队定制 + 30 分钟空档）—— Otter / 飞书都在左上（通用 + 全周期）。**右下空白**。

**Jobs 视角戳**：

> 「1 周内能复制吗？
>  - 功能层（中文 ASR + LLM action 提取）→ 可复制（Otter 只要愿意做中文版 1 个月就上线）  
>  - 团队 context 学习层 → **6 个月学习曲线，无法直接复制**  
> 
>  ⚠️ **黄灯**——功能复制风险存在。**写进 Risks，必须计划 Phase 2 加深护城河。**」

**Jobs 检查记录**：
> 1 周复制：⚠️ 黄灯（功能层可复制，团队 context 层不可）→ 写进 Risks

---

## Risks（5 件套之外的风险登记）

- **R1（来自 audit weakest link）**：4 个外部接口（会议平台 / ASR / LLM / task 系统）—— 应对：D1+D2 降低到 1 个（只依赖 LLM）
- **R2（来自 Step 5 黄灯）**：功能层 1 周复制风险 —— 应对：Phase 1 投团队 context 学习机制（**这是 audit 的 D3 决策**），Phase 2 加跨会议历史搜索 + 团队成员个性化报告 = 提高 switching cost
- **R3（Frame 过程发现）**：用户抗拒"又一个 SaaS 账号"—— 应对：Phase 1 优先 CLI / VSCode 插件 / 飞书机器人，避免独立网页登录

---

## 5 件套连贯性自检

| 链条 | 通过？ |
|------|--------|
| persona ↔ JTBD：李工 EM ↔ 周一晨会瞬间 | ☑ |
| JTBD ↔ problem：30 分钟空档 ↔ 主持人没空档 | ☑ |
| problem ↔ positioning：把整理人换成 AI ↔ 会后 30 分钟派工 | ☑ |
| positioning ↔ differentiation：团队定制+空档 ↔ 团队 context 学习 | ☑ |
| differentiation ↔ persona：李工抗拒"又一个 SaaS" ↔ 飞书机器人/CLI 形态 | ☑ |

---

## 下一步

- [x] **HARD GATE 通过** → 进入 D 段 `product-spec`
- [ ] R1-R3 风险已登记，D 段写需求时必须考虑
- [ ] **Frame 包路径**：`docs/frames/2026-05-03-teampulse.md`

---

## 元数据

- 角色切换：Step 3 / Step 4 / Step 5 进入 Steve Jobs 视角检查；HARD GATE 通过后退出
- 退回次数：Step 1: 0 / Step 2: 0 / Step 3: **1**（Problem 第一次写成方案）/ Step 4: 0 / Step 5: 0（黄灯但通过）
- 总用时：~25 min（含退回时间）
- 重审建议：D 段写需求时若发现 Frame 不准，允许回 C 段重做相应 Step

---

## 演示说明（看完这个 example 你应该 take away 什么）

1. **Step 3 退回是常态**——大多数人第一次写 Problem 都会写成方案（"缺工具" vs "失 action"）。Jobs 视角的二元门就是用来逼真问题
2. **REFRAME "啊瞬间"是 Frame 的核心价值**——"工具不够好"→"主持人没空档"是真翻转，不是改写
3. **Tagline 在 Frame 阶段升级了**——audit 时是"会议→action 30 分钟"（动作 + 时间），Frame 时升级为"会后 30 分钟派工"（动词更明确）
4. **黄灯是允许通过的**——Step 5 differentiation 黄灯不强制退回，但写进 Risks，D 段必须有应对
5. **5 件套连贯性自检**容易被忽略——本例最后一行（differentiation ↔ persona = 飞书机器人不要 SaaS 登录）是从 persona "抗拒 SaaS 账号"反推出来的产品形态约束
