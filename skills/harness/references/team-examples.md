# 智能体团队示例

---

## 示例 1: 研究团队 (智能体团队模式)

### 团队架构: 扇出/扇入
### 执行模式: 智能体团队

```
[领导者/编排器]
    ├── TeamCreate(research-team)
    ├── TaskCreate(4个调查任务)
    ├── 成员自主协调 (SendMessage)
    ├── 结果收集 (Read)
    └── 生成综合报告
```

### 智能体配置

| 成员 | 智能体类型 | 角色 | 输出 |
|------|-----------|------|------|
| official-researcher | general-purpose | 官方文档/博客 | research_official.md |
| media-researcher | general-purpose | 媒体/投资 | research_media.md |
| community-researcher | general-purpose | 社区/SNS | research_community.md |
| background-researcher | general-purpose | 背景/竞争/学术 | research_background.md |
| (领导者 = 编排器) | — | 整合报告 | 综合报告.md |

> 研究智能体使用 `general-purpose` 内置类型，但必须通过 `.claude/agents/{name}.md` 文件定义。文件中明确角色·调查范围·团队通信协议以保证复用性和协作质量。

### 编排器工作流 (智能体团队)

```
Phase 1: 准备
  - 分析用户输入 (主题，调查模式)
  - 创建 _workspace/

Phase 2: 团队组建
  - TeamCreate(team_name: "research-team", members: [
      { name: "official", prompt: "调查官方渠道..." },
      { name: "media", prompt: "调查媒体/投资动向..." },
      { name: "community", prompt: "调查社区反应..." },
      { name: "background", prompt: "调查背景/竞争环境..." }
    ])
  - TaskCreate(tasks: [
      { title: "调查官方渠道", assignee: "official" },
      { title: "调查媒体动向", assignee: "media" },
      { title: "调查社区反应", assignee: "community" },
      { title: "调查背景环境", assignee: "background" }
    ])

Phase 3: 执行调查
  - 4名成员独立进行调查
  - 发现有趣内容时通过 SendMessage 在成员间共享
    (例如: media 发现的投资新闻传递给 background)
  - 发现冲突信息时成员之间直接讨论
  - 各成员完成时保存文件并通知领导者

Phase 4: 整合
  - 领导者通过 Read 读取4份产出物
  - 生成综合报告
  - 冲突信息标明来源并列记录

Phase 5: 清理
  - 请求成员终止
  - 清理团队
  - 保留 _workspace/ (用于事后验证·审计追踪)
```

### 团队通信模式

```
official ──SendMessage──→ background  (共享相关官方公告)
media ────SendMessage──→ background  (共享投资/收购信息)
community ─SendMessage──→ media      (社区反应中与媒体相关的信息)
所有成员 ──TaskUpdate──→ 共享任务列表  (更新进度)
领导者 ←───── 空闲通知 ──── 完成的成员   (自动)
```

---

## 示例 2: SF 小说写作团队 (智能体团队模式)

### 团队架构: 流水线 + 扇出
### 执行模式: 智能体团队

```
Phase 1 (并行 — 智能体团队): worldbuilder + character-designer + plot-architect
  → 通过 SendMessage 互相协调一致性
Phase 2 (顺序): prose-stylist (撰写)
Phase 3 (并行 — 智能体团队): science-consultant + continuity-manager (审阅)
  → 通过 SendMessage 互相共享发现
Phase 4 (顺序): prose-stylist (反映审阅修改)
```

### 智能体配置

| 成员 | 智能体类型 | 角色 | 技能 |
|------|-----------|------|------|
| worldbuilder | 自定义 | 世界观构建 | world-setting |
| character-designer | 自定义 | 角色设计 | character-profile |
| plot-architect | 自定义 | 情节结构 | outline |
| prose-stylist | 自定义 | 文体编辑 + 撰写 | write-scene, review-chapter |
| science-consultant | 自定义 | 科学验证 | science-check |
| continuity-manager | 自定义 | 一致性验证 | consistency-check |

### 智能体文件完整示例: `worldbuilder.md`

```markdown
---
name: worldbuilder
description: "构建 SF 小说世界观的专家。设计物理法则、社会结构、技术水平、历史。"
---

# Worldbuilder — SF 世界观设计专家

你是 SF 小说世界观设计专家。基于科学事实发挥想象力，构建故事展开世界的物理·社会·技术基础。

## 核心角色
1. 定义世界的物理法则和技术水平
2. 设计社会结构、政治体系、经济系统
3. 确立历史脉络和当前冲突结构
4. 描述各地点的环境和氛围

## 工作原则
- 内部一致性最优先 — 设定之间不能有矛盾
- 通过 "如果存在这种技术？" 的连锁问题推演世界的影响
- 服务于故事的世界观 — 避免阻碍情节的过度设定

## 输入/输出协议
- 输入: 用户的世界观概念，体裁要求
- 输出: `_workspace/01_worldbuilder_setting.md`
- 格式: Markdown。按章节划分 (物理/社会/技术/历史/地点)

## 团队通信协议
- 向 character-designer: 通过 SendMessage 发送社会结构、阶级系统、职业群信息
- 向 plot-architect: 通过 SendMessage 发送世界的主要冲突结构、危机要素
- 从 science-consultant: 接收科学错误反馈 → 修改设定
- 世界观变更时向所有相关成员广播

## 错误处理
- 概念模糊时提出3个方向并请求选择
- 发现科学错误时同时提供替代方案

## 协作
- 向 character-designer 提供社会结构信息
- 向 plot-architect 提供冲突结构信息
- 反映 science-consultant 的反馈修改设定
```

### 团队工作流详情

```
Phase 1: TeamCreate(team_name: "novel-team", members: [worldbuilder, character-designer, plot-architect])
         TaskCreate([世界观构建, 角色设计, 情节结构])
         → 成员自主协调并行工作
         → worldbuilder 完成社会结构时 SendMessage 给 character-designer
         → character-designer 设定主角时 SendMessage 给 plot-architect

Phase 2: 清理 Phase 1 团队 → 以子智能体调用 prose-stylist (独立撰写无需团队)
         prose-stylist 通过 Read 读取 _workspace/ 的3份产出物并撰写
         → 结果保存到 _workspace/02_prose_draft.md

Phase 3: 创建新团队 — TeamCreate(team_name: "review-team", members: [science-consultant, continuity-manager])
         (每会话只能激活一个团队，但 Phase 1 团队已清理，可以创建新团队)
         → 两位审阅者审阅 draft，互相共享发现
         → science-consultant 发现物理错误时也通知 continuity-manager
         → 审阅完成后清理团队

Phase 4: 以子智能体调用 prose-stylist，反映审阅结果进行最终修改
```

---

## 示例 3: 网络漫画制作团队 (子智能体模式)

### 团队架构: 生成-验证
### 执行模式: 子智能体

> 生成-验证模式中只有2个智能体，且核心是结果传递而非通信，因此子智能体更适合。

```
Phase 1: Agent(webtoon-artist) → 生成面板
Phase 2: Agent(webtoon-reviewer) → 审核
Phase 3: Agent(webtoon-artist) → 问题面板重新生成 (最多2次)
```

### 智能体配置

| 智能体 | subagent_type | 角色 | 技能 |
|--------|--------------|------|------|
| webtoon-artist | 自定义 | 面板图片生成 | generate-webtoon |
| webtoon-reviewer | 自定义 | 质量审核 | review-webtoon, fix-webtoon-panel |

### 智能体文件完整示例: `webtoon-reviewer.md`

```markdown
---
name: webtoon-reviewer
description: "审核网络漫画面板质量的专家。评估构图、角色一致性、文字可读性、演出效果。"
---

# Webtoon Reviewer — 网络漫画质量审核专家

你是审核网络漫画面板质量的专家。以视觉完成度、故事传达力、角色一致性为标准评估面板。

## 核心角色
1. 评估每个面板的构图和视觉完成度
2. 验证角色外形在面板间的一致性
3. 评估气泡文字的可读性和布局
4. 审视整集的演出节奏和步调

## 工作原则
- 以 PASS/FIX/REDO 三级明确判定
- FIX 是可通过部分修改解决的情况，REDO 是需要全面重新生成
- 以客观标准 (一致性、可读性、构图) 判断而非主观喜好

## 输入/输出协议
- 输入: `_workspace/panels/` 目录中的面板图片
- 输出: `_workspace/review_report.md`
- 格式:
  ```
  ## Panel {N}
  - 判定: PASS | FIX | REDO
  - 原因: [具体理由]
  - 修改指示: [FIX/REDO 时给出具体修改方向]
  ```

## 错误处理
- 图片加载失败时判定该面板为 REDO
- 2次重新生成后仍为 REDO 的面板附上警告后按 PASS 处理

## 协作
- 向 webtoon-artist 传递修改指示书 (基于结果文件)
- 再次审核重新生成的面板 (最多2次循环)
```

### 错误处理

```
重试策略:
- REDO 判定面板 → 向 artist 请求重新生成 (包含具体修改指示)
- 最多2次循环后强制 PASS
- 超过50%的面板为 REDO 时向用户建议修改提示词
```

---

## 示例 4: 代码审查团队 (智能体团队模式)

### 团队架构: 扇出/扇入 + 讨论
### 执行模式: 智能体团队

> 代码审查是智能体团队大放异彩的典型案例。不同视角的审阅者共享发现并互相质疑，实现更深入的审查。

```
[领导者] → TeamCreate(review-team)
    ├── security-reviewer: 安全漏洞检查
    ├── performance-reviewer: 性能影响分析
    └── test-reviewer: 测试覆盖率验证
    → 审阅者互相共享发现 (SendMessage)
    → 领导者综合结果
```

### 团队通信模式

```
security ──SendMessage──→ performance  ("此 SQL 查询可能存在注入，请从性能角度也确认一下")
performance ──SendMessage──→ test      ("发现 N+1 查询，请确认是否有相关测试")
test ────SendMessage──→ security      ("认证模块无测试，从安全角度看优先级如何？")
```

核心: 审阅者**不经过领导者**直接沟通，快速捕获跨领域问题。

---

## 示例 5: 监督者模式 — 代码迁移团队 (智能体团队模式)

### 团队架构: 监督者
### 执行模式: 智能体团队

```
[supervisor/领导者] → 分析文件列表 → 分配批次
    ├→ [migrator-1] (批次 A)
    ├→ [migrator-2] (批次 B)
    └→ [migrator-3] (批次 C)
    ← 接收 TaskUpdate → 额外批次分配或重新分配
```

### 智能体配置

| 成员 | 角色 |
|------|------|
| (领导者 = migration-supervisor) | 文件分析，批次分配，进度管理 |
| migrator-1~3 | 迁移分配的文件批次 |

### 监督者的动态分配逻辑 (利用智能体团队)

```
1. 收集全部目标文件列表
2. 估算复杂度 (文件大小，import 数量，依赖关系)
3. 通过 TaskCreate 将文件批次注册为任务 (包含依赖关系)
4. 成员自行认领 (claim) 任务
5. 成员通过 TaskUpdate 报告完成时:
   - 成功 → 自动认领下一个任务
   - 失败 → 领导者通过 SendMessage 确认原因 → 重新分配或分配给其他成员
6. 所有任务完成 → 领导者执行集成测试
```

与扇出的区别: 任务不是预先固定的，而是在**运行时动态分配**。共享任务列表的自行认领 (claim) 功能与监督者模式自然匹配。

---

## 产出物模式总结

### 智能体定义文件
位置: `项目/.claude/agents/{agent-name}.md`
必需部分: 核心角色，工作原则，输入/输出协议，错误处理，协作
团队模式追加部分: **团队通信协议** (消息接收/发送，任务请求范围)

### 技能文件结构
位置: `项目/.claude/skills/{skill-name}/SKILL.md` (项目级别)
或: `~/.claude/skills/{skill-name}/SKILL.md` (全局级别)

### 集成技能 (编排器)
协调整个团队的上层技能。定义各场景的智能体配置和工作流。
模板: 参考 `references/orchestrator-template.md`。
**必须注明执行模式** — 智能体团队 (默认) 或子智能体。
