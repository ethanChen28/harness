# 为 Harness 贡献

> 本文档是 [CONTRIBUTING.md](CONTRIBUTING.md) 的中文翻译版本。

感谢您考虑为 **Harness** 做出贡献——这是一个用于设计 Agent Teams 和生成技能的 Claude Code 元技能工厂。

本文档涵盖：响应 SLA、如何贡献、开发环境搭建、PR 规范、提交消息规则、行为准则以及维护者列表。

---

## 响应 SLA（服务承诺）

以下是本仓库维护者的响应目标。这些目标是**保守的**，以便小型维护者团队能够在扩展的同时切实遵守。

| 方面 | 目标 | 备注 |
|------|------|------|
| PR — 首次响应 | **< 72小时** | 工作日。"首次响应"至少包含一个标签 + 一条确认 PR 的评论。 |
| Issue 分类与标记 | **< 48小时** | 每个新 issue 将在 48 小时内移除 `needs-triage` 并获得类型标签（`bug` / `enhancement` / `question` / `discussion`）。 |
| Bug 修复（P0 / P1） | **< 14天** | P0 = 数据丢失 / 安全问题 / 安装失败。P1 = 常用路径故障。P2/P3 在路线图上跟踪，无硬性 SLA。 |
| 安全报告 | **< 7天** | 7 天内初步确认。补丁目标 30 天。请参阅下方**安全**部分了解私密渠道。 |
| 发布节奏 | **每 2 周** | 每两周打一次标签，除非没有可发布内容。P0 修复可能会安排计划外的补丁发布。 |

如果我们未能在 SLA 内响应，请随时在 issue/PR 中提醒——这并不粗鲁，这是我们约定的反馈机制。

---

## 如何贡献

不同类型的贡献通过不同的入口进行。请选择适合您的方式。

### Bug 报告

- 使用 **Bug report** 表单（`.github/ISSUE_TEMPLATE/bug_report.yml`）提交 issue。
- 必填项：Claude Code 版本、`CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` 标志状态、复现步骤、期望行为与实际行为、操作系统。
- 小型复现（< 30 行）最为理想。如果复现需要完整项目，请链接一个公开的 fork。

### 功能请求

- 使用 **Feature request** 表单提交 issue。
- 我们期望一段简短的"这解决了什么问题"说明。如果您有提案，请以 PR 就绪的格式提交（它扩展/替换了 6 种团队架构模式中的哪一种？）。

### 提问

- 使用 **Question** 表单提交 issue，**或者**在 [GitHub Discussions](https://github.com/revfactory/harness/discussions) 中发帖讨论（适用于开放式问题）。

### 讨论（RFC 级别的想法）

- 优先使用 GitHub Discussions。只有在方向上达成初步共识后才升级为 issue。

### Pull Request

- 请参阅下方 **Pull Request 指南**。
- 小型 PR 合并更快。任何超过 400 行变更的内容应先发起 Discussion。

### 安全

- 对于可能被利用的安全问题，**不要**公开提交 issue。
- 发送邮件至：`robin.hwang@kakaocorp.com`，主题前缀为 `[harness-security]`。
- 我们力争在 7 天内确认（见 SLA 表格）。

---

## 开发环境搭建

### 前置条件

- Claude Code `v2.x`（需要 Agent Teams API）
- Node.js `>= 18`（用于 CI 中的本地工具）
- Git

### 环境标志

Harness 目前需要 Claude Code 的实验性 Agent Teams 功能。请在您的 shell 配置文件中或每次会话中设置此标志：

```bash
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
```

我们在 `docs/experimental-dependency.md` 中跟踪此依赖（如果 Anthropic 将此标志提升为稳定版，我们将按照上述 SLA 在 72 小时内更新 README）。

### 本地插件链接

要在本地 Claude Code 会话中测试您的更改而无需发布到 marketplace：

```bash
# 从您的检出目录
claude plugin link ./harness

# 验证
claude plugin list | grep harness
```

完成后使用 `claude plugin unlink harness` 取消链接。

### 运行元技能

```bash
claude "build a harness for a fintech risk-assessment team"
```

脚手架生成的 agents 和 skills 将放置在目标项目的 `.claude/agents/` 和 `.claude/skills/` 目录下。

### 测试与代码检查

- Markdown 检查：`npx markdownlint '**/*.md'`
- YAML 检查（issue 模板与工作流）：`npx yaml-lint .github/`
- 技能元数据验证：`python scripts/validate_skills.py`（如果存在）

CI 会在每个 PR 上运行这些检查。鼓励本地执行但不是必需的——我们不会因为 CI 发现的简单问题阻塞合并。

---

## Pull Request 指南

### 分支命名

使用 `type/short-description` 格式：

| 前缀 | 用途 | 示例 |
|------|------|------|
| `feat/` | 新的用户可见功能 | `feat/expert-pool-variance-mode` |
| `fix/` | Bug 修复 | `fix/agent-teams-flag-detection` |
| `docs/` | 仅文档更改 | `docs/quickstart-gemini-section` |
| `refactor/` | 内部结构重构，无行为变更 | `refactor/skill-loader-split` |
| `chore/` | 构建、依赖、维护工作 | `chore/upgrade-markdownlint` |
| `test/` | 仅测试 | `test/fan-out-fan-in-e2e` |

### 提交消息语言

- **中文和英文均可接受。** 请使用您能更精确表达的语言。
- 如果更改将出现在 CHANGELOG 或发布说明中，请在 PR 描述中也提供英文标题，以便下游读者理解。

### PR 模板

每个 PR 正文会从 `.github/PULL_REQUEST_TEMPLATE.md` 预填充。请填写以下内容：

- **摘要**（做什么和为什么，2-4 句话）
- **动机**（链接 issue、引用研究或一行理由）
- **变更范围**（涉及方面的清单）
- **测试**（您运行/添加了什么测试）
- **CHANGELOG**（是否更新了 `CHANGELOG.md`？Y/N/NA）
- **SemVer 影响**（patch / minor / major——见下一节）

### 审查期望

- 需要一位维护者的批准审查。
- 我们力争在 72 小时内响应 PR（见 SLA）。如果您被阻塞了，请提醒我们。

---

## 提交消息规范

我们遵循 **Conventional Commits** 的轻量变体，直接映射到 SemVer。

```
<type>(<scope>)!: <short summary>

<body — 可选>

<footer — 可选>
```

### 类型与 SemVer 映射

| 提交类型 | SemVer 影响 | 示例 |
|----------|-------------|------|
| `feat!:` 或 footer 中的 `BREAKING CHANGE:` | **major**（如 1.x → 2.0） | `feat!: rename primary pattern "Supervisor" → "Orchestrator"` |
| `feat:` | **minor**（如 1.2 → 1.3） | `feat: add Producer-Reviewer variance metric` |
| `fix:` | **patch**（如 1.2.3 → 1.2.4） | `fix: correct flag detection on zsh` |
| `docs:` / `chore:` / `refactor:` / `test:` | 不触发版本发布 | `docs: clarify Gemini roadmap` |

- 中文摘要也可以：`feat: 为专家池模式添加分散度指标`。
- `!` 后缀（或 `BREAKING CHANGE:` footer）是**唯一**的主版本触发器。请不要轻易使用。

### 发布标签

- 每 2 周进行一次发布（见 SLA）。
- CI 通过且 CHANGELOG 更新后从 `main` 分支打标签。
- 标签格式为 `vMAJOR.MINOR.PATCH`（如 `v1.3.0`）。

---

## 行为准则

本项目遵循 **Contributor Covenant v1.4**——简而言之：

- 友好包容，善意推定。
- 禁止骚扰、人身攻击和歧视性语言。
- 批评观点，而非人身。尽可能用参考资料支持论点。
- 维护者有权审核、编辑或删除违反这些原则的评论/提交/issue/PR，并可以封禁违规者。

完整文本：<https://www.contributor-covenant.org/version/1/4/code-of-conduct/>

请私下向 `robin.hwang@kakaocorp.com` 报告行为准则违规，主题前缀为 `[harness-coc]`。

---

## 维护者

| 角色 | 账号 | 负责领域 |
|------|------|----------|
| 主要维护者 | [@revfactory](https://github.com/revfactory) | 项目方向、发布、最终审查 |
| 贡献者 | [@hnts03](https://github.com/hnts03) | 技能模板、中文文档 |
| 贡献者 | [@JunghwanNA](https://github.com/JunghwanNA) | Agent 模式、集成测试 |
| 贡献者 | [@shaun0927](https://github.com/shaun0927) | 工具链、CI、基础设施 |

新贡献者在持续贡献（而非单次 PR）后将被列在此处。如果您想讨论成为维护者的路径，请在 Discussion 中留言。

---

## 许可证

通过贡献代码，您同意您的贡献将按照与本仓库相同的许可证进行授权（见 [`LICENSE`](./LICENSE)）。
