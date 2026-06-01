# 实验性标志依赖

> 本文档是 [experimental-dependency.md](experimental-dependency.md) 的中文翻译版本。

> **状态：** 活跃 · **负责人：** revfactory · **最后更新：** 2026-04-18 · **SLA：** 参见 [监控承诺](#监控承诺)

本文档解释了 `harness` 为什么需要 `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`、该标志的三种可能未来，以及本仓库在每种情况下将采取的措施——附带有时间限制的承诺，以便企业采用者可以据此进行规划。

---

## 当前状态

### 为什么需要该标志

`harness` 是一个构建在 Claude Code **Agent Teams API** 之上的元技能工厂。当用户运行 `claude "build a harness for <domain>"` 时，内部会调用三个 Claude Code 原语：

| 原语 | 用途 | 受标志门控？ |
|------|------|-------------|
| `TeamCreate` | 实例化具有共享上下文的多 Agent 团队 | **是** |
| `SendMessage` | 在团队成员之间路由消息（主管 ↔ 工作者） | **是** |
| `TaskCreate` | 在团队内生成长时间运行的子任务 | **是** |
| `Agent` 工具（invoke） | 单 Agent 调度 | 否（GA） |

所有三个受标志门控的原语都需要：

```bash
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
```

如果在启动 `claude` 的 shell 中未设置此变量，harness 生成的团队将回退到单 Agent 执行模式，这将静默地破坏 Pipeline / Fan-out-in / Supervisor / Hierarchical Delegation 模式。

### Anthropic 参考资料（提交 issue 前必读）

该标志的设计原理和路线图存在于三篇 Anthropic 工程博客文章中。评估 harness 的采用者应至少阅读第一篇：

1. [Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) — 定义了 Anthropic 认可的 "harness" 类别以及长时间运行 Agent 的契约。
2. [Harness design for long-running apps](https://www.anthropic.com/engineering/harness-design-long-running-apps) — `harness` 所编纂的模式（Pipeline、Producer-Reviewer、Supervisor 等）。
3. [Scaling Managed Agents](https://www.anthropic.com/engineering/managed-agents) — 可能取代实验性标志的前进路径（参见场景 B）。

---

## 依赖图

```
harness (v1.2.0)
  └── Agent Teams API (Claude Code)
        ├── TeamCreate            ← EXPERIMENTAL_AGENT_TEAMS=1
        ├── SendMessage           ← EXPERIMENTAL_AGENT_TEAMS=1
        ├── TaskCreate            ← EXPERIMENTAL_AGENT_TEAMS=1
        └── Agent (invoke)        ← GA (flag-independent)
              └── Anthropic Roadmap
                    ├── Scenario A: Flag removed (GA promotion)
                    ├── Scenario B: Managed Agents GA (parallel path)
                    └── Scenario C: Breaking signature change
```

**自上而下阅读此图：** harness 依赖于 Agent Teams API，后者依赖于单个实验性标志，而该标志又依赖于 Anthropic 自身的路线图。如果任何上游节点发生变化，本仓库有义务在下述 SLA 时间内进行适配。

---

## 3 种场景

每种场景都列出了**检测触发条件**（我们如何知道它发生了）、本仓库承诺的 **T+24h / T+48h / T+72h 行动**，以及每个检查点的**用户可见产物**。

### 场景 A — 标志移除（Agent Teams 升级为 GA）

**触发检测：** Anthropic Claude Code 更新日志发布 "Agent Teams is now GA"，**或者** `claude-code` 二进制文件不再需要 `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`（由 [P-13](#) 中的每夜 CI 检测到）。

**概率（主观评估）：** 高 — 这是上述三篇博客文章所预示的路径。

| 检查点 | 行动 | 产物 |
|--------|------|------|
| **T+24h** | 开启 `feat/drop-experimental-flag` 分支。从所有 README / 文档 / 快速入门中移除 `export` 行。在 `plugin.json` 中添加 `claude-code >= X.Y.Z` 下限约束。 | 分支 + PR（草稿） |
| **T+48h** | 发布 `docs/migrating-from-experimental.md`。更新 `docs/experimental-dependency.md`（本文件）标题为 "no flag required as of vX.Y"。置顶 GitHub issue："Action required: drop the export line"。 | 迁移指南 + 置顶 issue |
| **T+72h** | 发布 **v1.3.0** 版本，包含：(a) CHANGELOG 条目，(b) 带有迁移说明的 `gh release create`，(c) HN 跟进帖："We dropped the experimental flag"。 | `v1.3.0` git 标签 + GH Release |

**对采用者的影响：** 正面。企业审批摩擦降低 — 一个复选框（"无实验性标志"）即可满足要求。对 harness 用户代码无破坏性变更。

---

### 场景 B — Managed Agents 达到 GA（并行路径）

**触发检测：** Anthropic 发布 "[Managed Agents](https://www.anthropic.com/engineering/managed-agents) is generally available"，提供稳定的 `claude-agents` CLI 或 SDK 接口。

**概率（主观评估）：** 90 天内为中高。Managed Agents 是一种服务端执行模型；harness 的客户端团队编排**不能**自动转换。

| 检查点 | 行动 | 产物 |
|--------|------|------|
| **T+24h** | 开启 `feat/managed-agents-compat` PR。添加 `adapters/managed-agents/` 脚手架，将 harness 的 6 种团队模式映射到 Managed Agents 调用。识别不兼容的模式（可能是：Hierarchical Delegation）。 | 兼容性 PR（草稿） |
| **T+48h** | 在 Dev.to 和仓库中发布博客文章：**"Harness + Managed Agents: one layer up, not replaced"**。将 harness 重新定位为输出 Managed Agents 配置的**设计时**层，而非运行时竞争者。 | 共存定位博客 |
| **T+72h** | 发布 `docs/managed-agents-migration.md`，包含按模式的矩阵（6 种模式中哪些可以 1:1 映射，哪些需要重写）。更新 README 的相关仓库部分。 | 迁移指南 |

**战略说明：** harness 重新定位为 **Managed Agents 之上的上层** — "Managed Agents 运行团队，harness 设计团队。" 这是 GTM 计划第 4.2 节中的共存框架。

**对采用者的影响：** 中性偏正面。现有的 harness 用户可以继续在实验性标志路径上工作；新用户可以选择 Managed Agents 输出。

---

### 场景 C — 破坏性变更（API 签名变更）

**触发检测：** 每夜 CI（`.github/workflows/nightly-compat.yml`，作为路线图 P-13 追踪）在 Claude Code 最新每夜构建版本上失败，**或者**更新日志宣布环境变量重命名 / `TeamCreate` 签名变更。

**概率（主观评估）：** 中。实验性 API 可能在没有弃用窗口的情况下被重命名。

| 检查点 | 行动 | 产物 |
|--------|------|------|
| **T+0 至 T+24h** | 每夜 CI 告警在 Slack/Discord 中触发。作者开启 `hotfix/compat-<date>` 分支，修补受影响的调用点。单元测试在旧签名 + 新签名上均通过（尽最大努力）。 | 热修复分支 |
| **T+24h** | 合并热修复。推送 `v1.2.x` 补丁标签。更新 `docs/compatibility-matrix.md` 中受影响 Claude Code 版本对应的行。 | `v1.2.x` 补丁版本 |
| **T+72h** | 如果变更为非琐碎的（影响 harness 的公开契约），在仓库 Discussions 标签页 + X 上发布简短通知。否则，CHANGELOG 条目即可。 | Discussions 帖子（有条件） |

**对采用者的影响：** 固定使用先前 Claude Code 版本的现有用户不受影响。使用最新版本的用户将在同一周内获得补丁。

---

## 监控承诺

我们承诺以下**可观察的 SLA**。未能满足可作为使用 `sla-breach` 标签提交 issue 的依据。

| 事件 | SLA | 衡量标准 |
|------|-----|---------|
| Anthropic 在官方更新日志中发布 Agent Teams / Managed Agents 变更 | 本文档在 **72 小时内** 更新 | 比较更新日志发布时间戳与本文件的 `最后更新` 行 |
| 每夜 CI 检测到兼容性破坏 | 热修复分支在 **24 小时内** 开启 | GitHub Actions 运行时间戳与分支创建时间戳对比 |
| 新的 Claude Code 稳定版本（次要或主要） | 在 **7 天内** 添加 `docs/compatibility-matrix.md` 行 | 兼容性矩阵差异 |

**我们主动监控的来源：**

- Claude Code 发布说明 — 通过 [Anthropic Engineering 博客](https://www.anthropic.com/engineering) RSS 关注
- `anthropics/claude-code` GitHub Releases（每夜标签）
- Anthropic Discord `#claude-code` 频道（社区信号）

---

## 企业采用者常见问题

### Q1. 我们处于受监管行业（金融、医疗、公共部门），无法在生产环境中启用 `EXPERIMENTAL` 标志。我们如何采用 harness？

**原因：** 许多合规框架（SOC 2 Type II、ISO 27001、K-ISMS）禁止在生产环境中使用不稳定 / 预览功能。
**建议：** 仅在设计时使用 harness：在沙箱工作站中运行它来生成 `.claude/agents/` 和 `.claude/skills/` 文件，然后将生成的制品提交到您的生产仓库。生产环境的 Claude Code 永远不需要该标志 — 只有受标志门控的 `TeamCreate` 运行时才需要。生成的单 Agent 技能与 GA 路径兼容。

### Q2. 如果 Agent Teams 升级为 GA（场景 A），我现有的 harness 生成代码会出问题吗？

**原因：** 在 Anthropic 的 Claude Code 中，GA 升级在历史上对生成的制品是非破坏性的；该标志只是不再需要了。
**建议：** 最终用户无需采取任何行动。您的 `.claude/agents/*.md` 和 `.claude/skills/*` 文件是纯 Markdown，仍然有效。在 GA 发布当天，您将可以执行 `unset CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS`。我们将在 48 小时内发布迁移说明（参见场景 A）。

### Q3. 你们有书面 SLA 保证吗？如果未达到会怎样？

**原因：** 企业在批准前需要合同性的或至少可观察的承诺。
**建议：** 上方的 SLA 表格是我们的**公开承诺**，并通过以下方式执行：(a) 一个 GitHub Action，在检测到更新日志事件后，如果本文件的 `最后更新` 行超过 72 小时，会在此文件上添加评论，(b) 采用者可使用的 `sla-breach` issue 标签，(c) `CONTRIBUTING.md` 中对任何违约的事后分析义务。这不是付费 SLA — 而是社区承诺。如需付费 SLA，请联系维护者（参见仓库 README）。

---

**相关文档：**
- [`docs/quickstart.md`](./quickstart.md) — 5 分钟安装指南
- [`docs/show-hn-launch-kit.md`](./show-hn-launch-kit.md) — 公开发布包
- `docs/compatibility-matrix.md` *（待 P-13）* — Claude Code × harness 版本对照表
