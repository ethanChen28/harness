# 快速入门 — 5 分钟完成你的第一个 Harness

> 本文档是 [quickstart.md](quickstart.md) 的中文翻译版本。

> **时间预算：5 分钟（严格）。** 如果你在 5 分钟内没有到达第 5 步，请停下来提交一个 issue — 这是文档的 bug，不是你的问题。

<!-- TODO: Loom embed — 60s screen recording showing Steps 1→5 end-to-end. Replace this comment with the `<iframe>` once recorded. -->

**完成后你将拥有：** 一个可用的 `.claude/agents/` 目录，包含 3–5 个领域专业化 agent，通过一句话提示词生成，可直接在示例任务上运行。

**前置条件（开始前请确认）：**
- Claude Code **v2.x 或更高版本**（`claude --version` 应返回 `2.x` 或更高）
- 能在命令间保持 `export` 的 shell（bash、zsh 或 fish）
- 能访问 `github.com` 和 `api.anthropic.com` 的网络

---

## 第 1 步 — 添加 marketplace（60 秒）

```bash
claude plugin marketplace add revfactory/harness
```

**作用：** 注册 `harness` marketplace，使 Claude Code 能够发现 `revfactory` 发布的插件。

**预期输出：** `Added marketplace: revfactory/harness`

---

## 第 2 步 — 安装插件并启用实验性标志（40 秒）

```bash
claude plugin install harness@harness
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
```

*（要使该标志在 shell 会话间持久生效，请将 `export` 行追加到 `~/.zshrc` 或 `~/.bashrc` 中。）*

**作用：** 从 `harness` marketplace 安装 `harness` 插件，然后启用 Agent Teams — Claude Code 用于编排多 agent 工作流的 API harness。关于为什么需要此标志，请参阅 [`docs/experimental-dependency.md`](./experimental-dependency.md)。

**故障排查 #1 — `AGENT_TEAMS not found` / 团队未实例化**
**原因：** Claude Code 版本低于 v2.x（Agent Teams 在 v2.0 中引入）。
**解决方法：** 运行 `claude --version`。如果低于 2.0，通过 `npm i -g @anthropic-ai/claude-code`（或你所用发行版的安装器）升级，然后重复第 2 步。

---

## 第 3 步 — 用一句话生成一个 harness（2 分钟）

```bash
claude "build a harness for a fintech risk-assessment team"
```

**作用：** 调用 `/harness:harness` 元技能（meta-skill），该技能会分析你的领域描述，并在当前目录的 `.claude/agents/` 和 `.claude/skills/` 中搭建一个专业化 agent 团队及其技能。

**试试这些替代提示词** — 任何一个都可以：
- `claude "配置一下 Harness — 金融科技风险评估团队"`（中文也可以）
- `claude "build a harness for an e-commerce fraud-detection workflow"`
- `claude "design an agent team for technical due diligence on open-source repos"`

**预期输出：** 一个流式计划，然后确认已写入 3–5 个 agent `.md` 文件及其技能。

**故障排查 #2 — 中文提示词无返回结果 / 英文提示词成功但中文不行**
**原因：** 区域设置或分词器路由问题；harness 的编排器通过中文触发词（"配置一下 Harness"）进行匹配，这些触发词内置于技能定义中。
**解决方法：** 如果中文失败，请使用上面的英文提示词重新运行 — 底层技能是完全相同的。如果两者都失败，请跳到故障排查 #3。

---

## 第 4 步 — 验证生成的文件（30 秒）

```bash
ls -la .claude/agents/
ls -la .claude/skills/
```

**作用：** 确认元技能已将文件写入预期位置。

**预期输出：** 每个目录 3–5 个文件，文件名反映你的领域（例如，金融科技示例中的 `risk-analyst.md`、`compliance-reviewer.md`、`portfolio-monitor.md`）。

**故障排查 #3 — "未生成任何内容" / 目录为空**
**原因：** 插件未实际安装或在当前项目中未激活。
**解决方法：** 运行 `claude plugin list`。如果 `harness@harness` 不存在，重复第 2 步。如果存在但未激活，运行 `claude plugin enable harness@harness`，然后重复第 3 步。

---

## 第 5 步 — 用新团队运行一个示例任务（90 秒）

复制一个真实的 Jira 工单风格的提示词，交给你的新团队：

```bash
claude "Ticket FIN-427: A new corporate customer (mid-cap manufacturer, \$80M revenue, South Korea) has applied for a \$5M working-capital line. Produce a risk assessment covering (1) credit-history red flags, (2) sector concentration vs. our existing book, (3) regulatory exposure (KFTC, FSC). Output: a 1-page memo with a go/no-go recommendation."
```

**作用：** Claude Code 检测到 `.claude/agents/` 中的新 agent，通过 harness 生成的团队模式（风险评估工作通常为 Producer-Reviewer 或 Expert-Pool 模式）路由任务，并返回结构化的备忘录。

**故障排查 #4 — "团队未执行 / 只有一个 agent 响应"**
**原因：** `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` 在运行第 3 步的 shell 中设置了，但没有在运行第 5 步的 shell 中设置（打开新终端时会发生这种情况）。
**解决方法：** 在当前 shell 中重新导出：`export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`，然后重新运行第 5 步。要永久生效，将该行添加到你的 shell rc 文件中。

**故障排查 #5 — "API 调用太多 / 担心成本"**
**原因：** 多 agent 团队可以扩展到每个任务 5 个以上的并行 Claude 调用。一个复杂的工单可能消耗 50K–200K token。
**解决方法：** 每次运行限制为单个任务（不要用 `&&` 链式调用多个 harness），如果你的 Claude Code 版本支持，可以使用 `--max-turns` 标志。对于生产环境，请通过成本感知包装器来控制 harness 调用 — 参见 `docs/cost-controls.md` *（即将推出）*。

---

## 完成

此时你应该拥有：

- [x] 一个包含领域专业化 agent 的 `.claude/agents/` 目录
- [x] 一个包含配套技能的 `.claude/skills/` 目录
- [x] 一次成功的示例任务执行
- [x] 一个正常工作的 `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` 环境

**推荐阅读：**
- [`docs/experimental-dependency.md`](./experimental-dependency.md) — 为什么需要这个标志，以及当它变化时我们会怎么做
- [`revfactory/harness-100`](https://github.com/revfactory/harness-100) — 100+ 预构建领域 harness 目录，如果你更想克隆而非生成
- [`revfactory/claude-code-harness`](https://github.com/revfactory/claude-code-harness) — 我们用于在 15 个任务上测量 +60% 质量提升的 A/B 测试 harness

**如果你遇到了本指南未涵盖的问题：** 请使用 `quickstart-gap` 标签提交一个 issue，并附上：(a) 哪一步失败了，(b) `claude --version` 的输出，(c) 确切的错误信息。quickstart-gap issue 的首次响应 SLA 为 **48 小时**（参见 `CONTRIBUTING.md`）。
