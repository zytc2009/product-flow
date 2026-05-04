# Spark Candidates — Skill 片段级合并与个性化裁剪

> 日期：2026-05-03 ｜ 收敛人：用户 ｜ 输入源：口头（单条） ｜ 候选数量：1
> 下一步：进入 product-audit

---

## 候选 1：Skill 片段级 Remix 工具（暂定名 skill-fragment-remix）

把 superpowers / everything-claude-code / 项目内 skills 等多源 skill 仓库，按**节 / step / 协议**级别（而非文件级）挑选合并，输出可程序化消费的 skill 对象，让用户既能给团队发定制包，也能把片段塞进非 Claude Code 环境（Cursor / Codex / 自建 agent SDK app）。

| 字段 | 内容 | HARD GATE |
|------|------|-----------|
| **target customer** | (a) Claude Code 团队 lead / 教练；(b) 跨 agent 框架的工具开发者。共性：**skill 中间商**——不直接消费 skill，而是把 skill 重新打包发给下游 | ⚠️ 待审：双 segment 合并，Audit Pass 1 该问"必须挑一个"还是"两类共用一个底层抽象就够" |
| **核心场景** | 三个并列：(1) 给团队/项目发一套定制 skill 包；(3) 把 skill 内容集成到非 Claude Code 环境的 agent app；(4) 上游仓库升级时同步定制版而不覆盖本地改动 | ⚠️ 待审：3 个场景同时 = 平台型项目，Audit Pass 2 Q2 必砍 |
| **最强对手** | 复制粘贴 SKILL.md 内容到 system prompt / agent app（不是某个具体工具，是**手工动作**本身） | ⚠️ 待审：对手是"手工"意味着市场可能根本没意识到这是问题 / 也可能意味着没人愿意付费解决 |
| **差异化苗头** | **片段级而非文件级** —— 挑某节、某 step、某协议（如"只要 brainstorming 的硬门那段"），而非整文件粘走 | — |

**来源**：单条口头想法（无 cluster），用户原话："superpowers 和 everything-claude-code, 项目skills 我想做个合并与个性化定制，有些 skill 太复杂了，我不想执行到底，我只想用其中一部分"

---

## Step 4 收敛轨迹

| Q | 选项 | 解读 |
|---|------|------|
| Q1 target | 3 + 4 | 团队 lead + 工具开发者（合并） |
| Q2 场景 | 1 + 3 + 4 | 团队包 / 代码集成 / 上游同步 |
| Q3 对手 | 2 | 复制粘贴 SKILL.md 到 system prompt |
| Q4 差异化 | 1 | 片段级而非文件级 |

**关键信号**（喂给 Audit 时记得说）：

- 场景选了 3 个但对手只选了 1 个（场景 3 的对手）→ 用户**心里实际重心在场景 3（代码集成）**，团队/同步是顺带的。Audit Pass 2 Q2 砍 80% 时该把场景 1 和 4 当成可砍候选
- target 是合并的两类——但有共同抽象（"skill 中间商"），不算硬冲突，但 Audit Pass 1 标红字段时会戳
- 差异化锚定在"片段级"——这是一个**架构选择**而非营销卖点，意味着产品的核心是**一个新的 skill 数据模型**，不是 UI 优化

---

## 待审切入语（喂给 product-audit 时复制此段）

> 我想做一个 **skill 片段级 remix 工具**：从 superpowers / everything-claude-code / 项目内 skills 等多源仓库里，按节/step/协议（不是文件级）挑选合并，输出可被代码消费的 skill 对象。
>
> **目标用户**：Claude Code 团队 lead/教练 + 跨 agent 框架的工具开发者，共性是"skill 中间商"——不直接用 skill，而是把 skill 重新打包发给下游。
>
> **核心场景**（3 个并列，但重心其实在第 2 个）：
> 1. 给团队发一套定制 skill 包，避免每人装 5 个仓库
> 2. 把 skill 内容塞进非 Claude Code 环境（Cursor/Codex/自建 agent SDK），现在只能复制粘贴 SKILL.md
> 3. 上游升级时同步而不覆盖本地改动
>
> **最强对手**：复制粘贴 SKILL.md 到 system prompt 这种手工动作（不是某个工具）。
>
> **差异化苗头**：片段级而非文件级——挑节/step/协议而不是整文件搬。
>
> 帮我审一下这个方向。

---

## HARD GATE 检查

| 检查项 | 状态 |
|--------|------|
| target 是具体角色（不是"中小企业"/"年轻人"） | ⚠️ 通过但合并了两类 |
| 核心场景是真实瞬间 | ⚠️ 通过但有 3 个 |
| 最强对手有真名 | ⚠️ 通过但是"手工动作"非工具名 |
| 候选数量 1-3 | ✓ |

**结论**：候选信息足以进 Audit，**但有 3 处待审信号**，已在切入语中标注，让 Audit 直接戳这些点而不是从零问起。

---

## 下一步

- [x] 候选生成完成
- [ ] **候选 1 → 进 `product-audit`** ← 强烈建议立刻
- [ ] 落单想法归档（无 —— 单条想法）
- [ ] 30 天后未进 Audit → 重跑 product-spark（市场和心境会变）

---

## 元数据

- 落盘路径：`D:/AI/skill/docs/sparks/2026-05-03-skill-fragment-remix.md`
- product-spark 跳过了 Cluster（单条想法），直接进 Converge
- 单条想法走完整流程是边界场景——下次类似情况可以考虑直接进 Audit 跳过 spark
