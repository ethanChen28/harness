# Changelog

本项目遵循 [Semantic Versioning](https://semver.org/)。

## [Unreleased]

### Added
- 新建智能体/技能前的重复审查步骤 (Phase 3-0, Phase 4-0)
- `references/agent-design-patterns.md` "智能体复用设计" 部分
- `references/skill-writing-guide.md` §9 "技能复用设计"

### Changed
- Phase 选择矩阵中标注 3-0/4-0
- Phase 2-3 添加复用审查步骤指针
- 产出物检查清单中添加 2 项复用审查条目

---

## [1.2.1] - 2026-04-18

### Fixed

- **版本一致性同步** — README.md / README_KO.md / README_JA.md 徽章为 `v1.0.1`，`.claude-plugin/marketplace.json` 为 `1.1.0`，`.claude-plugin/plugin.json` 为 `1.2.0`，三处不一致 → 全部统一为 **v1.2.0** (以 plugin.json 为准)
- **标签化发布 0 项状态解除准备** — v1.0.0 / v1.0.1 / v1.1.0 / v1.2.0 追溯标签计划编写 (`_workspace/release/audit-2026-04-18.md` §4 参照)

### Added

- **定位声明: "harness factory"** — README 顶部引入类别自我定义语句。以"按领域生成智能体+技能的 Harness 工厂"抢占类别定位（区别于单一智能体/提示词框架）
- **CONTRIBUTING.md** — 贡献指南及 SLA 说明 (PR 首次响应 72h，Issue 分类 48h)。降低社区入门门槛
- **docs/ 目录** — 长期文档（架构、迁移、模式目录）迁移空间新建。防止 README 膨胀并提升可搜索性
- **Issue #3 响应策略** — 社区 Issue 官方响应模板及分类流程添加

### Changed

- `.claude-plugin/marketplace.json` version: `1.1.0` → `1.2.0`
- README 徽章 (EN/KO/JA 3 种): `Version-1.0.1` → `Version-1.2.0`
- **`.claude-plugin/plugin.json` description 重写** — `"Agent Team & Skill Architect — Meta-skill that designs..."` → `"The team-architecture factory for Claude Code — a meta-skill that turns a domain description into an agent team and the skills they use, with six pre-defined team-architecture patterns..."` (EN+ZH 并记，反映 L3 Meta-Factory 定位)
- **`.claude-plugin/plugin.json` keywords 扩展** — 5 个 → 17 个 (`harness-factory`, `team-architecture-factory`, `claude-code-plugin`, `agent-scaffolding`, `multi-agent`，6 种模式关键词 6 个追加)

## [1.2.0] - 2026-04-08

### Changed

- **CLAUDE.md 注册策略简化（去除重复）** — Phase 5-4 从"上下文注册"转为"指针注册"。从 CLAUDE.md 中移除智能体列表、技能列表、目录结构、执行规则详情，仅保留**触发规则 + 变更历史**。智能体/技能列表由 `.claude/agents/`、`.claude/skills/` 及编排器技能作为单一来源管理
- **Phase 3/4 临时同步步骤删除** — 为减轻 CLAUDE.md 同步负担，移除 Phase 3/4 的临时同步指示。最终指针注册仅在 Phase 5-4 执行一次
- **核心原则第 3 条重新定义** — "在 CLAUDE.md 中注册 Harness 上下文" → "在 CLAUDE.md 中注册 Harness 指针"
- **CLAUDE.md vs 编排器角色分工表删除** — 指针策略简化后表格不再需要

### Added

- **Phase 2-1: 混合执行模式** — 在智能体团队/子智能体之外，增加按 Phase 混合模式的混合模式。列出常用组合（并行采集→共识整合、团队创建→验证、Phase 间团队重组）
- **Phase 2-1 执行模式对比表** — 提供团队/子智能体/混合 3 种特性及决策顺序 3 步骤
- **Phase 5-0 混合编排器模式** — 混合配置时在每个 Phase 顶部标注执行模式的规则
- **Phase 5-1 返回值数据传递** — 子智能体模式专用数据传递策略追加（在现有消息/任务/文件基础上增加返回值）
- **Phase 5-1 推荐组合 (子智能体/混合)** — 除团队模式外，明确子智能体和混合模式的数据传递推荐组合

## [1.1.0] - 2026-04-05

### Added

- **Phase 0: 现状审计** — 触发时先确认现有 Harness 状态，路由到新建/扩展现有/运维维护三个分支
- **现有扩展 Phase 选择矩阵** — 按添加智能体/添加技能/更改架构区分所需 Phase 的决策表
- **Phase 3/4 CLAUDE.md 临时同步** — 智能体/技能生成后立即反映到 CLAUDE.md（会话中断容错）
- **Phase 5-4: CLAUDE.md Harness 上下文注册** — 记录智能体团队结构、技能列表、执行规则、目录结构、变更历史。包含 CLAUDE.md vs 编排器角色分工表
- **Phase 5-5: 后续工作支持** — 编排器 description 必须包含后续关键词，Phase 0 上下文确认步骤自动判断初次/部分重新执行/新执行
- **Phase 5 编排器修改路径** — 扩展现有时不创建新编排器而是修改现有编排器的指南
- **Phase 7: Harness 进化机制** — 执行后反馈收集 → 按反馈类型映射修改目标 → 变更历史记录 → 自动进化触发
- **Phase 7-5: 运维维护工作流** — 现状审计→渐进修改→CLAUDE.md 同步→变更验证 4 步骤
- **description 添加运维维护触发** — "Harness 检查"、"Harness 审计"、"Harness 现状"、"智能体/技能同步" 关键词
- **产出物检查清单强化** — CLAUDE.md 同步完成、变更历史记录、Phase 0 上下文确认项目追加
- 编排器模板添加 Phase 0（上下文确认）— 适用于智能体团队/子智能体两种模式
- 编排器 description 模板包含后续工作关键词模式

### Changed

- 核心原则从 2 条扩展为 4 条（新增 CLAUDE.md 注册、进化系统）
- **"进化日志" → "变更历史"统一** — 名称和模式（4 列：日期/变更内容/对象/原因）在所有章节中统一
- **Phase 1 Step 3** — 改为基于 Phase 0 审计结果进行冲突分析（去除重复）
- **5-4 CLAUDE.md 模板代码块** — 修复嵌套渲染问题（3 反引号→4 反引号）
- **角色分工表扩展** — 追加技能列表、目录结构、变更历史行
- **编排器模板** — 追加 Phase 0 上下文确认步骤和后续工作关键词指南

## [1.0.1] - 2026-03-28

### Changed

- SKILL.md ↔ references 间重复内容删除 (330 行 → 285 行)
  - Phase 2-1: 执行模式对比表/项目符号 → 核心原则 + agent-design-patterns.md 指针
  - Phase 2-3: 智能体分离标准项目符号 → 4 轴摘要 + agent-design-patterns.md 指针
  - Phase 3: 智能体定义模板代码块 → 必要章节列举 + references 指针
  - Phase 5-2: 错误处理 5 行表格 → 核心原则 + orchestrator-template.md 指针

## [1.0.0] - 2026-03-27

### Added

- 6 Phase 工作流驱动的 Harness 配置元技能
- 6 种智能体架构模式（流水线、扇出/扇入、专家池、生成-验证、监督者、层级委派）
- 智能体团队 / 子智能体执行模式支持
- Progressive Disclosure 技能生成指南
- 编排器模板（智能体团队模式 + 子智能体模式）
- QA 智能体集成指南（基于实际项目 7 个缺陷案例）
- 技能测试/评估方法论（With-skill vs Without-skill 对比）
- 5 种实战团队配置示例（研究、小说、漫画、代码审查、迁移）
