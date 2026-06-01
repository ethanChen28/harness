<p align="center">
  <img src="harness_banner.png" alt="Harness Banner" width="600">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Version-1.2.0-brightgreen.svg" alt="Version">
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-Apache_2.0-blue.svg" alt="License"></a>
  <img src="https://img.shields.io/badge/Claude_Code-Plugin-purple.svg" alt="Claude Code Plugin">
  <img src="https://img.shields.io/badge/Patterns-6_Architectures-orange.svg" alt="6 Architecture Patterns">
  <img src="https://img.shields.io/badge/Mode-Agent_Teams-green.svg" alt="Agent Teams">
  <a href="https://github.com/revfactory/harness/stargazers"><img src="https://img.shields.io/github/stars/revfactory/harness?style=social" alt="GitHub Stars"></a>
</p>

<p align="center">
  <a href="#类别--harness-处于什么位置"><img src="https://img.shields.io/badge/Layer-L3%20Meta--Factory-orange" alt="Layer"></a>
  <a href="#类别--harness-处于什么位置"><img src="https://img.shields.io/badge/Sub--layer-Team--Architecture%20Factory-teal" alt="Sub-layer"></a>
  <a href="#"><img src="https://img.shields.io/badge/README-EN%20%7C%20ZH%20%7C%20JA-lightgrey" alt="i18n"></a>
</p>

# Harness — Claude Code 的团队架构工厂

[English](README.md) | **中文** | [日本語](README_JA.md)

> **Harness 是 Claude Code 的团队架构工厂。** 只需说 **"配置一下 Harness"** (中文) · **"build a harness for this project"** (English) · **"ハーネスを構成して"** (日本語)，插件就会将你的领域描述转化为智能体团队及其使用的技能 — 从 6 种预定义的团队架构模式中选择。

## 概述

Harness 利用 Claude Code 的智能体团队系统，将复杂任务分解为由专业智能体组成的协调团队。只需说"配置一下 Harness"，即可自动生成适合你领域的智能体定义（`.claude/agents/`）和技能（`.claude/skills/`）。

## 类别 — Harness 处于什么位置

Harness 位于 Claude Code 生态系统的 **L3 Meta-Factory** 层 — 不是其他 Harness，而是"生成其他 Harness 的层"。在该层中，我们选择了 **Team-Architecture Factory** 子层。

| 层级 | 职责 | 共存的邻居 |
|------|------|-----------|
| **L3 — Meta-Factory / Team-Architecture Factory** (本项目) | 领域描述 → 智能体团队 + 技能，6 种预定义团队模式 | — |
| L3 — Meta-Factory / Runtime-Configuration Factory | 确定性的、可重复的运行时配置生成 | [coleam00/Archon](https://github.com/coleam00/Archon) |
| L3 — Meta-Factory / Codex Runtime Port | 同一概念的 Codex 运行时版本 | [SaehwanPark/meta-harness](https://github.com/SaehwanPark/meta-harness) |
| L2 — Cross-Harness Workflow | 跨多个 Harness 的技能、规则、钩子标准化 | [affaan-m/ECC](https://github.com/affaan-m/everything-claude-code) |

> Archon 生成确定性的运行时配置。Harness 生成团队架构（流水线、扇出/扇入、专家池、生成-验证、监督者、层级委派）和智能体使用的技能。两者是同一 L3 的不同子层。需要运行时确定性就用 Archon，需要团队架构就用 Harness，或者组合使用。

## 核心功能

- **智能体团队设计** — 支持流水线、扇出/扇入、专家池、生成-验证、监督者、层级委派等 6 种架构模式
- **技能生成** — 采用 Progressive Disclosure 模式高效管理上下文的技能自动生成
- **编排** — 包含智能体间数据传递、错误处理、团队协调协议
- **验证体系** — 触发验证、空运行测试、With-skill vs Without-skill 对比测试

## Harness 进化机制

Harness 进化机制将"什么有效、什么无效"的增量反馈回工厂，使下一代可衡量地更好。当生成的 Harness 在实际项目中被使用时，`/harness:evolve` 技能捕获初始架构与最终发布架构之间的变化量并反馈给工厂。下次针对相同领域的生成将反映这些反馈，从"更接近发布状态的草稿"开始。

```
初始 Harness ──▶ 实际项目使用 ──▶ 发布版 Harness
                                      │
                                      ▼ (/harness:evolve 捕获增量)
                                ┌───────────────┐
                                │    工厂       │◀── 更好的下一代草稿
                                └───────────────┘
```

这就是 **Harness 进化机制 (Harness Evolution Mechanism; JA: ハーネス進化メカニズム)**。

## 工作流

```
Phase 1: 领域分析
    ↓
Phase 2: 团队架构设计 (智能体团队 vs 子智能体)
    ↓
Phase 3: 智能体定义生成 (.claude/agents/)
    ↓
Phase 4: 技能生成 (.claude/skills/)
    ↓
Phase 5: 集成与编排
    ↓
Phase 6: 验证与测试
```

## 安装

### 通过市场安装

#### 添加市场
```shell
/plugin marketplace add revfactory/harness
```

#### 安装插件
```shell
/plugin install harness-marketplace
```

### 作为全局技能直接安装

```shell
# 将 skills 目录复制到 ~/.claude/skills/harness/
cp -r skills/harness ~/.claude/skills/harness
```

## 插件结构

```
harness/
├── .claude-plugin/
│   └── plugin.json                 # 插件清单
├── skills/
│   └── harness/
│       ├── SKILL.md                # 主技能定义 (6 Phase 工作流)
│       └── references/
│           ├── agent-design-patterns.md   # 6 种架构模式
│           ├── orchestrator-template.md   # 团队/子智能体编排器模板
│           ├── team-examples.md           # 5 种实战团队配置示例
│           ├── skill-writing-guide.md     # 技能编写指南
│           ├── skill-testing-guide.md     # 测试/评估方法论
│           └── qa-agent-guide.md          # QA 智能体集成指南
└── README.md
```

## 使用方法

在 Claude Code 中按以下方式触发：

```
配置一下 Harness
设计一下 Harness
为这个项目搭建智能体团队
```

### 执行模式

| 模式 | 说明 | 推荐场景 |
|------|------|----------|
| **智能体团队** (默认) | TeamCreate + SendMessage + TaskCreate | 2 个以上智能体，需要协作 |
| **子智能体** | Agent 工具直接调用 | 单次任务，无需通信 |

<p align="center">
  <img src="harness_team.png" alt="Harness Agent Team" width="500">
</p>

### 架构模式

| 模式 | 说明 |
|------|------|
| 流水线 | 顺序依赖任务 |
| 扇出/扇入 | 并行独立任务 |
| 专家池 | 按场景选择调用 |
| 生成-验证 | 生成后质量审查 |
| 监督者 | 中央智能体动态分配 |
| 层级委派 | 上级→下级递归委派 |

## 产出物

Harness 生成的文件：

```
项目/
├── .claude/
│   ├── agents/          # 智能体定义文件
│   │   ├── analyst.md
│   │   ├── builder.md
│   │   └── qa.md
│   └── skills/          # 技能文件
│       ├── analyze/
│       │   └── SKILL.md
│       └── build/
│           ├── SKILL.md
│           └── references/
```

## 使用案例 — 直接使用以下提示词

安装 Harness 后，将以下提示词复制到 Claude Code 中使用：

**深度研究**
```
配置一个研究 Harness。需要一个能从多个角度调查任何主题的智能体团队
— 网络搜索、学术资料、社区反应 — 交叉验证后生成综合报告。
```

**网站开发**
```
配置一个全栈网站开发 Harness。团队从线框图到部署，以流水线方式协调
设计、前端(React/Next.js)、后端(API)和 QA 测试。
```

**漫画制作**
```
配置一个漫画集制作 Harness。需要故事编写、角色设计提示词、分镜布局
策划、对白编辑智能体，并从风格一致性角度互相审查作品。
```

**YouTube 内容策划**
```
配置一个 YouTube 内容制作 Harness。由监督者智能体协调趋势调研、
脚本编写、标题/标签 SEO 优化、缩略图概念策划的团队。
```

**代码审查**
```
配置一个综合代码审查 Harness。并行审计架构、安全漏洞、性能瓶颈、
代码风格的智能体，将结果汇总为一份报告。
```

**技术文档编写**
```
配置一个从这个代码库自动生成 API 文档的 Harness。团队以流水线方式
处理端点分析、描述编写、使用示例生成、完成度审查。
```

**数据流水线设计**
```
配置一个数据流水线设计 Harness。智能体团队以层级方式委派模式设计、
ETL 逻辑、数据验证规则、监控设置。
```

**营销活动**
```
配置一个营销活动制作 Harness。团队在迭代质量审查中完成目标市场调研、
广告文案编写、视觉概念设计、A/B 测试计划。
```

## 共存 — Harness 与邻居仓库

Harness 在 Claude Code / 智能体框架生态系统中并非孤立存在。以下仓库位于相邻层级，均以"X 是 ···，Harness 是 ···"的并列结构描述，可根据用途选择或组合使用。

| 仓库 | 仓库定位 | 与 Harness 的关系 |
|------|---------|-----------------|
| [coleam00/Archon](https://github.com/coleam00/Archon) | "harness builder" — 确定性、可重复的运行时配置 | **同 L3，相邻子层。** Archon 是 Runtime-Configuration Factory，Harness 是 Team-Architecture Factory。运行时确定性用 Archon，团队架构用 Harness，或组合使用。 |
| [SaehwanPark/meta-harness](https://github.com/SaehwanPark/meta-harness) | 同一概念的 Codex 移植 | **同 L3，不同运行时。** Claude Code 用 Harness，Codex 用 meta-harness。 |
| [affaan-m/ECC](https://github.com/affaan-m/everything-claude-code) | "Agent harness performance & workflow layer" — 坐在现有 Harness 之上的标准化层 | **不同层级。** ECC 是跨多个 Harness 的标准化层，Harness 是生成 Harness 的工厂。可串联组合。 |
| [wshobson/agents](https://github.com/wshobson/agents) | 子智能体 / 技能目录 (182 agents, 149 skills) | **工厂 ↔ 零件供应。** wshobson 是"选购目录"，Harness 是"团队设计"。Harness 创建的团队可吸收 wshobson 的条目作为零件。 |
| [LangGraph](https://langchain-ai.github.io/langgraph/) | 状态图编排，LLM 无关 | **不同轨道。** 需要长时间运行/状态恢复用 LangGraph，需要 Claude Code 原生快速团队设计用 Harness。 |

## 用 Harness 构建的项目

### Harness 100

**[revfactory/harness-100](https://github.com/revfactory/harness-100)** — 10 个领域、100 个生产就绪的智能体团队 Harness（中英 200 个包）。每个 Harness 包含 4-5 名专业智能体、编排器技能、领域专用技能，全部由本插件生成。涵盖内容创作、软件开发、数据/AI、商业战略、教育、法律、医疗等领域，共 1,808 个 Markdown 文件。

### 研究：Harness 应用前后 A/B 测试

**[revfactory/claude-code-harness](https://github.com/revfactory/claude-code-harness)** — 对 15 个软件工程任务进行的对照实验，测量了结构化预配置对 LLM 代码智能体输出质量的影响。

| 指标 | 未应用 Harness | 应用 Harness | 改善 |
|------|:-:|:-:|:-:|
| 平均质量分数 | 49.5 | 79.3 | **+60%** |
| 胜率 | — | — | **100%** (15/15) |
| 输出方差 | — | — | **-32%** |

核心发现：任务难度越高，改善效果越大 (Basic +23.8, Advanced +29.6, Expert +36.2)。

**请在任何场合直接使用这句话：** 平均质量 +60% (49.5 → 79.3)，15/15 胜率，输出方差 −32% (n=15，作者自测 A/B，第三方复现实验进行中)。

> 论文全文：*Hwang, M. (2026). Harness: Structured Pre-Configuration for Enhancing LLM Code Agent Output Quality.*

## 系统要求

- [启用智能体团队功能](https://code.claude.com/docs/en/agent-teams)：`CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`

## FAQ

<details>
<summary><b>Q1. "+60%" 是否夸大？</b></summary>

**A.** +60% 是**作者自测 A/B (n=15, 15 个任务，在姊妹仓库 `claude-code-harness` 中测量)** 的结果。本仓库在引用此数据时，始终在同行句子中注明"n=15，作者自测，第三方复现实验进行中"。建议组织在引入时通过 2-4 周的内部试点测量自己的数据。

**Evidence:**
- 作者 A/B: [revfactory/claude-code-harness](https://github.com/revfactory/claude-code-harness)
- 论文: *Hwang, M. (2026). Harness: Structured Pre-Configuration for Enhancing LLM Code Agent Output Quality*
</details>

<details>
<summary><b>Q2. 为什么是 "harness factory" 而不是 "harness builder"？与 Archon 竞争吗？</b></summary>

**A.** Archon 是生成确定性运行时配置的 **Runtime-Configuration Factory**，Harness 是生成智能体团队架构（团队结构、消息协议、审查门控）的 **Team-Architecture Factory**。两者是**同 L3 Meta-Factory 层的相邻子层**，用途不同。需要确定性运行时用 Archon，需要 6 种团队架构模式预定义用 Harness。组合使用（架构设计 → 运行时部署）也行。

**Evidence:**
- Archon 自定义位: [clawfit docs/reference-levels.md](https://github.com/hongsw/clawfit/blob/main/docs/reference-levels.md)
- 子层声明：本 README **类别 — Harness 处于什么位置** 章节
- Archon 仓库: [github.com/coleam00/Archon](https://github.com/coleam00/Archon)
</details>

<details>
<summary><b>Q3. "Claude Code 专用" 是否太窄？Gemini、Codex 呢？</b></summary>

**A.** 目前官方运行时仅 Claude Code 单一。同一概念的 Codex 移植 [SaehwanPark/meta-harness](https://github.com/SaehwanPark/meta-harness) 已公开发布，现有 Codex 团队可直接从那边开始。Harness 选择了"Claude Code 原生、深入"的路线，跨运行时需求已将共存仓库 (meta-harness, harness-init, OpenRig) 的联动计划纳入路线图。

**Evidence:**
- Codex 移植: [github.com/SaehwanPark/meta-harness](https://github.com/SaehwanPark/meta-harness)
- 跨运行时脚手架: [github.com/Gizele1/harness-init](https://github.com/Gizele1/harness-init)
</details>

## 许可证

Apache 2.0
