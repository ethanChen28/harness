# 编排器技能模板

编排器是协调整个团队的上层技能。根据执行模式提供3种模板:

- **模板 A: 智能体团队模式 (默认)** — 2人以上协作时首选
- **模板 B: 子智能体模式 (替代)** — 不需要团队通信的情况
- **模板 C: 混合模式** — 每个 Phase 混合使用不同模式

---

## 模板 A: 智能体团队模式 (默认 · 首选)

2人以上智能体协作时**首先考虑的默认模式**。通过 `TeamCreate` 组建团队，通过共享任务列表和 `SendMessage` 进行协调。

```markdown
---
name: {domain}-orchestrator
description: "{领域}协调智能体团队的编排器。{初始执行关键词}。后续操作: {领域}结果修改、部分重新执行、更新、补充、再次执行、改善之前结果的请求时也必须使用此技能。"
---

# {Domain} Orchestrator

协调{领域}的智能体团队以生成{最终产出物}的集成技能。

## 执行模式: 智能体团队

## 智能体配置

| 成员 | 智能体类型 | 角色 | 技能 | 输出 |
|------|-----------|------|------|------|
| {teammate-1} | {自定义或内置} | {角色} | {skill} | {output-file} |
| {teammate-2} | {自定义或内置} | {角色} | {skill} | {output-file} |
| ... | | | | |

## 工作流

### Phase 0: 上下文确认 (支持后续操作)

确认是否存在已有产出物，以决定执行模式:

1. 确认 `_workspace/` 目录是否存在
2. 决定执行模式:
   - **`_workspace/` 不存在** → 初次执行。进入 Phase 1
   - **`_workspace/` 存在 + 用户请求部分修改** → 部分重新执行。仅重新调用相应智能体，仅覆盖已有产出物中需要修改的部分
   - **`_workspace/` 存在 + 提供新输入** → 新执行。将已有 `_workspace/` 移动到 `_workspace_{YYYYMMDD_HHMMSS}/` 后进入 Phase 1
3. 部分重新执行时: 将之前的产出物路径包含在智能体提示词中，指示智能体读取已有结果并反映反馈

### Phase 1: 准备
1. 分析用户输入 — {分析什么}
2. 在工作目录中创建 `_workspace/`
   - **初次执行**: 创建新的 `_workspace/`
   - **新执行**: 将已有 `_workspace/` 移动到 `_workspace_{YYYYMMDD_HHMMSS}/` 后重新创建 `_workspace/`
3. 将输入数据保存到 `_workspace/00_input/`

### Phase 2: 团队组建

1. 创建团队:
   ```
   TeamCreate(
     team_name: "{domain}-team",
     members: [
       { name: "{teammate-1}", agent_type: "{type}", model: "opus", prompt: "{角色说明及工作指示}" },
       { name: "{teammate-2}", agent_type: "{type}", model: "opus", prompt: "{角色说明及工作指示}" },
       ...
     ]
   )
   ```

2. 注册任务:
   ```
   TaskCreate(tasks: [
     { title: "{任务1}", description: "{详情}", assignee: "{teammate-1}" },
     { title: "{任务2}", description: "{详情}", assignee: "{teammate-2}" },
     { title: "{任务3}", description: "{详情}", depends_on: ["{任务1}"] },
     ...
   ])
   ```

   > 每位成员5~6个任务为宜。有依赖关系的任务用 `depends_on` 明确标注。

### Phase 3: {主要工作 — 例如: 调查/生成/分析}

**执行方式:** 成员自主协调

成员从共享任务列表中认领 (claim) 任务并独立执行。
领导者监控进展，必要时介入。

**成员间通信规则:**
- {teammate-1} 通过 SendMessage 向 {teammate-2} 传递 {什么信息}
- {teammate-2} 完成任务后将结果保存为文件并通知领导者
- 成员需要其他成员的结果时通过 SendMessage 请求

**产出物保存:**

| 成员 | 输出路径 |
|------|----------|
| {teammate-1} | `_workspace/{phase}_{teammate-1}_{artifact}.md` |
| {teammate-2} | `_workspace/{phase}_{teammate-2}_{artifact}.md` |

**领导者监控:**
- 成员进入空闲状态时自动接收通知
- 特定成员卡住时通过 SendMessage 下达指示或重新分配任务
- 通过 TaskGet 查看整体进度

### Phase 4: {后续工作 — 例如: 验证/整合}
1. 等待所有成员完成任务 (通过 TaskGet 确认状态)
2. 通过 Read 收集各成员的产出物
3. {整合/验证逻辑}
4. 生成最终产出物: `{output-path}/{filename}`

### Phase 5: 清理
1. 向成员发送终止请求 (SendMessage)
2. 清理团队 (TeamDelete)
3. 保留 `_workspace/` 目录 (不删除中间产出物 — 用于事后验证·审计追踪)
4. 向用户报告结果摘要

> **需要重组团队时:** 如果不同 Phase 需要不同的专家组合，先用 TeamDelete 清理当前团队，再用新的 TeamCreate 组建下一 Phase 的团队。前一个团队的产出物保存在 `_workspace/` 中，新团队可通过 Read 访问。

## 数据流

```
[领导者] → TeamCreate → [teammate-1] ←SendMessage→ [teammate-2]
                          │                           │
                          ↓                           ↓
                    artifact-1.md              artifact-2.md
                          │                           │
                          └───────── Read ────────────┘
                                     ↓
                              [领导者: 整合]
                                     ↓
                              最终产出物
```

## 错误处理

| 情况 | 策略 |
|------|------|
| 1名成员失败/停止 | 领导者检测 → 通过 SendMessage 确认状态 → 重启或创建替代成员 |
| 过半成员失败 | 通知用户并确认是否继续 |
| 超时 | 使用已收集的部分结果，终止未完成的成员 |
| 成员间数据冲突 | 标明来源后并列记录，不删除 |
| 任务状态延迟 | 领导者通过 TaskGet 确认后手动 TaskUpdate |

## 测试场景

### 正常流程
1. 用户提供{输入}
2. Phase 1 得出{分析结果}
3. Phase 2 组建团队 ({N}名成员 + {M}个任务)
4. Phase 3 成员自主协调执行任务
5. Phase 4 整合产出物生成最终结果
6. Phase 5 清理团队
7. 预期结果: 生成 `{output-path}/{filename}`

### 错误流程
1. Phase 3 中 {teammate-2} 因错误停止
2. 领导者接收空闲通知
3. 通过 SendMessage 确认状态 → 尝试重启
4. 重启失败时将 {teammate-2} 的任务重新分配给 {teammate-1}
5. 用剩余结果进入 Phase 4
6. 最终报告中注明 "{teammate-2} 区域部分未收集"
```

---

## 模板 B: 子智能体模式 (替代)

不需要团队通信开销的情况。通过 `Agent` 工具直接调用，用返回值收集结果。

```markdown
---
name: {domain}-orchestrator
description: "{领域}协调智能体的编排器。{初始执行关键词}。包含后续操作关键词。"
---

## 执行模式: 子智能体

## 智能体配置

| 智能体 | subagent_type | 角色 | 技能 | 输出 |
|--------|--------------|------|------|------|
| {agent-1} | {内置或自定义} | {角色} | {skill} | {output-file} |
| {agent-2} | ... | ... | ... | ... |

## 工作流

### Phase 0: 上下文确认
(与模板 A 相同 — 根据 `_workspace/` 是否存在进行分支)

### Phase 1: 准备
1. 分析输入
2. 创建 `_workspace/` (初次执行时，或新执行中将已有 `_workspace/` 移动到归档目录后)

### Phase 2: 并行执行
在单条消息中同时调用 N 个 Agent 工具:

| 智能体 | 输入 | 输出 | model | run_in_background |
|--------|------|------|-------|-------------------|
| {agent-1} | {来源} | `_workspace/{phase}_{agent}_{artifact}.md` | opus | true |
| {agent-2} | {来源} | `_workspace/{phase}_{agent}_{artifact}.md` | opus | true |

### Phase 3: 整合
1. 收集各智能体的返回值
2. 基于文件的产出物通过 Read 收集
3. 应用整合逻辑 → 最终产出物

### Phase 4: 清理
1. 保留 `_workspace/`
2. 报告结果摘要

## 错误处理
- 1个智能体失败: 重试1次。再次失败则注明缺失并继续
- 过半失败: 通知用户并确认是否继续
- 超时: 使用已收集的部分结果
```

---

## 模板 C: 混合模式

每个 Phase 使用不同的执行模式。在每个 Phase 开头注明 `**执行模式:** {团队 | 子智能体}`。

```markdown
---
name: {domain}-orchestrator
description: "{领域}编排器 (混合模式)。{关键词}。包含后续操作关键词。"
---

## 执行模式: 混合

| Phase | 模式 | 原因 |
|-------|------|------|
| Phase 2 (并行收集) | 子智能体 | 独立资料收集，不需要团队通信 |
| Phase 3 (共识整合) | 智能体团队 | 需要讨论冲突数据·达成共识 |
| Phase 4 (独立验证) | 子智能体 | 1名 QA 智能体进行客观验证 |

## 工作流

### Phase 2: 并行资料收集
**执行模式:** 子智能体

在单条消息中通过 Agent 工具并行调用 N 个智能体 (`run_in_background: true`)。
各结果保存到 `_workspace/02_{agent}_raw.md`。

### Phase 3: 共识整合
**执行模式:** 智能体团队

1. 通过 `TeamCreate` 组建整合团队 (editor + fact-checker + synthesizer)
2. 通过 `TaskCreate` 分配任务 — 全部读取 Phase 2 的 `_workspace/02_*` 文件
3. 成员通过 `SendMessage` 讨论冲突数据，基于文件达成共识
4. 生成最终整合版本 `_workspace/03_integrated.md`
5. 通过 `TeamDelete` 清理团队

### Phase 4: 独立验证
**执行模式:** 子智能体

单个 QA 子智能体以 `_workspace/03_integrated.md` 为输入生成验证报告。
```

**混合模式切换规则:**
- 团队 → 子智能体: 必须先用 `TeamDelete` 清理团队，再调用 Agent 工具
- 子智能体 → 团队: 将子智能体的文件产出物通过 Read 路径传递给团队成员
- 团队 → 团队: 清理前一个团队后进行新的 `TeamCreate` (每会话只能激活1个团队)

---

## 编写原则

1. **首先注明执行模式** — 编排器顶部注明 "智能体团队" / "子智能体" / "混合" 之一。混合模式必须有 Phase 模式表
2. **团队模式要具体说明 TeamCreate/SendMessage/TaskCreate 的用法** — 团队组建、任务注册、通信规则
3. **子智能体模式要完整指定 Agent 工具参数** — name, subagent_type, prompt, run_in_background, model
4. **文件路径使用绝对路径** — 禁止相对路径，以 `_workspace/` 为基准明确路径
5. **注明 Phase 之间的依赖关系** — 哪个 Phase 依赖哪个 Phase 的结果。混合模式要特别强调模式切换点
6. **错误处理要切合实际** — 不要假设 "一切都会成功"
7. **测试场景必不可少** — 至少1个正常流程 + 1个错误流程

## description 编写时的后续操作关键词

编排器 description 仅有初始执行关键词是不够的。必须包含以下后续操作表述:

- 重新执行/再次执行/更新/修改/补充
- "只重新执行{领域}的{部分}"
- "基于之前的结果"，"改善结果"
- 领域相关的日常请求 (例如: 午餐策略 Harness 则包含 "午餐"、"宣传"、"热门" 等)

缺少后续关键词的话，首次执行后 Harness 实际上就成了死代码。

## 实际编排器参考

扇出/扇入模式的编排器基本结构:
准备 → Phase 0(上下文确认) → TeamCreate + TaskCreate → N名成员并行执行 → Read + 整合 → 清理。
参考 `references/team-examples.md` 中的研究团队示例。
