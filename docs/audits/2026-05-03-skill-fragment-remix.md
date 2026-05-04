# Product Direction Audit — Skill 片段级 Remix（暂称 skill-compiler）

> 日期：2026-05-03 ｜ 审计人：用户 ｜ 调研时间窗：2026-05-03（基于磁盘上 4 套 skill 系统的真实状态）
> 上游 spark：`docs/sparks/2026-05-03-skill-fragment-remix.md`

## Verdict

**REFACTOR** —— 骨架对，3 颗螺丝必须拧

需求真实（多源 skill 重组在你磁盘 4 套系统里都缺）+ 赛道空白（0 个竞品）+ 技术拐点齐（skill 与 sub-agent 编排在 2026 同时成熟）。但产品形态、tagline、护城河都需 reframe。

---

## Positioning（来自 Pass 1，需依 Decision 1 重写）

For    重度 Claude Code 使用者 + sub-agent 编排者
       —— "skill 中间商"次要受益群体（团队 lead、跨框架开发者）

who    when 写自己的 agent 工作流时
       I want to 调用某 skill 的某几节，
       so I can 不被原 skill 的固定流程绑架，
              把执行交给我的 sub-agent

[skill-compiler] is a **Skill 编译器 / build step**
       —— 类比：webpack for skills / Tailwind 的 @apply for prompts

that   读你的 `skill.config.yml`，从多源 skill 仓库挑节合并，
       输出可被 Claude Code 加载的 SKILL.md
       —— 让 1 个 SKILL.md 不再是"剧本"，是"积木拼出来的成品"

unlike 复制粘贴 SKILL.md 到 system prompt
       （以及 git submodule、plugin marketplace 全装、fork 整仓库）

       differentiator: **声明式编译**（不是手工合并）+ **片段级**（不是文件级）

---

## What I'd cut（来自 Pass 2 · Q2，spark reframe 时已做）

- **「团队 skill 包发布」场景** —— 砍。这是包管理器的事，不是编译器
- **「上游同步」场景** —— 砍。这是 git submodule / git subtree 的事
- **target = 团队 lead/教练** —— 降级为次级用例，产品不为他们设计
- **target = 跨 agent 框架工具开发者** —— **暂砍 Phase 1**，但 Decision 3 要求 **Phase 2 拉回作为护城河**
- 留下的核心动作：**写一个声明 → 编出一个能跑的混合 SKILL.md**（一件事）

---

## Weakest link in the chain（来自 Pass 2 · Q3）

体验链拆解：

1. 听说工具 —— 社区
2. 安装 —— 用户自己
3. 写声明 —— **我**（产品自己控制）
4. 读取多源 skill —— Anthropic 生态
5. 解析 SKILL.md —— Anthropic spec
6. 合并输出 —— Anthropic spec
7. **Claude Code 加载并触发合并产物** —— **Anthropic** ❗

🔴 **被外部控制的关键环节**：Step 7 ——Claude Code 怎么加载、怎么决定哪个 skill 触发、frontmatter 怎么解析，**全在 Anthropic 手上**。这是体验天花板。

**应对方案**（Decision 3）：
- Phase 1 专心做 Claude Code Skill Compiler，吃赛道空白窗口
- **Phase 2 加跨框架输出**（Cursor / Codex / hermes-agent），即使 Anthropic 出原生组合功能也不死
- 监控 Anthropic 6 个月内是否发布原生 skill 组合（kill-switch metric 3）

---

## 3 个必须换的 decision（这一段是 Audit 的核心）

### D1 · 产品形态 = **Skill Compiler / Build Step**（不是 runtime import）
- 用户写 `skill.config.yml` → 跑 `skill-compile` → 输出 SKILL.md → Claude Code 正常加载
- 集成点：CLI / GitHub Action / `claude-code init` hook
- **不是** library，**不是** SDK，**不是**运行时 plugin

### D2 · Tagline 换到「积木 vs 剧本」象限
- 当前的「skill 量体裁衣」借形不借义、且与编译器形态不匹配
- 候选：**「skill 不再整套跑，按需编出来」** / **「积木式 skill」** / **「编 skill，不抄 skill」**
- 3 个月内换。早期暂用「skill 量体裁衣」OK，但不要锁死

### D3 · 护城河 = **跨框架输出**（Phase 2 拉回 spark 砍掉的 target #4）
- Anthropic 出原生 skill 组合 = 你的 Phase 1 价值归零
- Phase 1：专心做 Claude Code 编译器（吃空白）
- Phase 2：增加 Cursor / Codex / hermes-agent / 自建 agent 的输出格式 —— 即使 Anthropic 入场你还有跨框架价值
- 这是把 Decision 3 转成产品 roadmap 的具体动作

---

## Cheapest falsification（来自 Pass 3）

**Probe type**：**Vibe-Coded** —— 1 周搓一个能跑的最小 dogfood 版本

**1-week plan**：
- Day 1-2：写 `skill.config.yml` schema + markdown parser（按 H2 节切分 SKILL.md）
- Day 3-5：写合并逻辑（concat 节 + frontmatter merge + 触发词冲突检测）
- Day 6-7：自己用它合 3 个 skill 验证（product-spark Step 1-2 + brainstorming HARD GATE + audit Pass 2）；扔进 `~/.claude/skills/` 看 Claude Code 真能加载触发吗

预期成本：~7 工时/周（晚上+周末）；零金钱投入；只用你已有的 Claude Code 环境

---

## Kill-switches（达不到就停）

| # | Metric | 阈值 | 测量方式 |
|---|--------|------|---------|
| 1 | **Dogfood 可用度**：自己用合成 skill 真实工作的次数 | ≥ 3 次/周（连续 4 周） | 自己日记 + git log 中 SKILL.md 编辑频次 |
| 2 | **第三方真实意愿**：有类似痛点的 Claude Code 用户中 git clone 试过的 | ≥ 3/5 人 | 中文 Claude 社群 / Twitter 发 demo + 收 git clone log |
| 3 | **Anthropic 入场窗口**：原生 skill 组合功能未发布 | ≤ 6 个月内未发 | 监控 Anthropic blog / Claude Code release notes |

**任何一个达不到 → 杀。** Metric 3 尤其——发了立即重新审 Verdict，转 Phase 2 跨框架或全新方向。**不要硬撑。**

---

## Next step

- [x] Verdict = REFACTOR
- [ ] **应用 D1**：把 positioning 重写成"Skill Compiler"形态，不是"runtime 调用"
- [ ] **应用 D2**：3 个月内换 tagline 到「积木式 skill」或类似象限
- [ ] **应用 D3**：把跨框架输出明确为 Phase 2 roadmap
- [ ] 跑 Vibe-Coded probe（1 周）→ 4 周后看 kill-switch metric 1 是否达标
- [ ] 进入 **C 段 product-frame**（之前我们 5 段地图里的下一步）展开 user/problem/positioning 4 步
- [ ] 或者跳到 **D 段 product-spec**（你最高频切换的痛点区）写需求

---

## 元数据

- 调研基于：D:/AI/code2/superpowers, D:/AI/code/agent-skills, D:/AI/code/hermes-agent, D:/AI/skill/Product-Manager-Skills, D:/AI/skill/steve-jobs-skill, D:/AI/skill/product-flow/skills/product-spark, D:/AI/skill/product-flow/skills/product-audit
- 角色切换：Pass 2 进入 Steve Jobs 视角（基于公开言论推断，非本人观点），Pass 3 已退出
- 重审建议：3 个月后或 Anthropic 发布原生 skill 组合时
