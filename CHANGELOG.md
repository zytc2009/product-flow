# Changelog

> 遵循 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/) 格式。
> 版本号遵循 [Semantic Versioning](https://semver.org/lang/zh-CN/)（major.minor.patch）。

---

## [v0.1.0] · 2026-05-03

首发版本。完整 5 段闭环 + 端到端 dogfood example + superpowers 借鉴升级。

### Added · 5 段 skill

- **A · `product-spark`** —— 灵感收敛器
  - 双输入源：obsidian / 口头 / 混合
  - Cluster 阶段（连点成线 from steve-jobs-skill）+ Converge 阶段（一次一问 from superpowers/brainstorming）
  - HARD GATE：3 项最小输入齐才进 audit
  - 单想法 escape hatch：跳过 Cluster 直接进 Converge

- **B · `product-audit`** —— 方向审计
  - 三段流程：Pass 1 Scaffold（PM 框架）/ Pass 2 Stress-test（Jobs 视角）/ Pass 3 Falsify（PM 教练态）
  - **磁盘优先调研**机制：磁盘代码 > 用户 URL > WebSearch
  - Verdict 三档强制：amazing / refactor / shit（无 hedging）
  - 角色切换协议：显式宣告进入 + 退出 Jobs 视角

- **C · `product-frame`** —— 5 件套展开
  - 5 步 mini 三段循环：persona / JTBD / problem / positioning / differentiation
  - 每步 PM 教练态填空 + Jobs 视角二元门
  - HARD GATE：5 件套连贯性自检 + Jobs 全过

- **D · `product-spec`** —— 需求生成
  - 5 phase 受控振荡：hypothesis / stories / split / scope / PRD
  - **振荡保护机制**：相邻 ≥ 3 次 / 总 ≥ 5 次 / 同 phase ≥ 3 次 → 红灯回上游
  - **Sub-agent dispatch hints**（首创设计）：每个 MUST story 标 type / primary_skill / context_files / acceptance_check / human_review_gate
  - 输出 1.5 页精简 PRD（不是传统 10 页）

- **E · `product-verdict`** —— 上线后迭代审计
  - 3 类触发：时间 / metric / 外部
  - 3 步流程：Compare（机械对比）/ Diagnose（Jobs 三问）/ Verdict（持守 / 调整 / 杀）
  - **Sunk cost 防御机制**：连续 2 次"再观察 / 持守"允许，第 3 次禁止
  - 历史 index 文件作为防御事实依据

### Added · superpowers/brainstorming 借鉴升级（2026-05-04）

读完 superpowers/brainstorming 后引入 3 处机制升级：

1. **`<HARD-GATE>` XML 标签**（A / C / D 段）
   - 把原 markdown 描述升级为 XML 标签，让 LLM 当系统约束执行
   - 加 3-5 条"不允许的事"明确禁止边界
2. **Self-review 4 项扫描**（C / D 段）
   - HARD GATE 之前 agent 自检：placeholder / 内部矛盾 / scope / 歧义
   - 就地修，无需 user 重新评审
3. **Visual Companion 协议**（C / D 段）
   - 涉及视觉内容（persona avatar / 2×2 差异化象限 / user flow / sub-agent 派单依赖图）时可选
   - offer 必须独立消息，逐题判断用文字 or 视觉

### Added · 工程化

- 单 Claude Code plugin 包（`.claude-plugin/plugin.json`）
- 5 段统一 `product-` 前缀（避免与其他 skill 冲突）
- `docs/` 落盘目录：sparks / audits / frames / prds / verdicts
- 5 个 dogfood example：TeamPulse · 会议→action 自动化（共 999 行）
- README.md 含 60 秒上手 + FAQ + Troubleshooting
- CHANGELOG / GLOSSARY / WALKTHROUGH / PATTERNS 4 份补充文档

### Borrowed · 跨仓库 pattern 致谢

| 借自 | 借用 |
|------|------|
| `superpowers/brainstorming` | 一次一问 / 多选优先 / `<HARD-GATE>` / 落盘 design doc / self-review 4 项 / visual companion |
| `superpowers/using-superpowers` | "1% 适用就 invoke" 心法 / process skill 先于 implementation skill |
| `Product-Manager-Skills` (47 skills) | Geoffrey Moore / JTBD / proto-persona / problem-framing-canvas / pol-probe / user-story / MoSCoW / SaaS metrics 等 |
| `steve-jobs-skill` | 角色扮演协议 / 6 心智模型 / 表达 DNA / Focus = Saying No |
| `obsidian` skill | A 段输入源（query / topic-scout 模式） |
| `hermes-agent` | sub-agent 编排 + memory pattern 参考 |

### Known trade-offs（已知设计妥协）

1. **不解决 Anthropic plugin loader 限制**：skill 只能整文件加载，无法运行时片段 import——这是 dogfood 产品 skill-fragment-remix 试图解决的问题，本套 product-flow 反而是它的"用户"
2. **振荡 / sunk cost 阈值是经验值**：3 / 5 / 3 不是科学数字，是基于"足够防 rationalize 又不至于过度严格"的判断。可能因人调整
3. **Visual companion 当前仅 ASCII 输出**：browser-based mockup 协议来自 superpowers，本套未实现 browser companion，仅文字描述图表
4. **5 段都偏轻量个人 / 小团队**：大公司多干系人 / 跨团队 alignment 场景未优化（需要 Product-Manager-Skills 的 workshop-facilitation 等 skill 补足）
5. **触发词中文为主**：英文触发不全（如"frame this product"暂未在 description 里——按需补）

### Unresolved（v0.1.0 未做但记账）

- [ ] cron 集成 example：让 product-verdict 真的月度自动跑
- [ ] 真实第二个产品方向跑通验证流程（目前只跑了 skill-fragment-remix 的 A→B 半段）
- [ ] CONTRIBUTING.md：如果未来开放贡献需要
- [ ] marketplace 发布：`marketplace.json` + namespace 注册

---

## [Unreleased]

下一个版本可能加：

- 跑 1 周 Vibe-Coded probe 验证 dogfood 真实有效性
- 实测 cron 集成路径
- 收集真实使用反馈，校准振荡阈值
