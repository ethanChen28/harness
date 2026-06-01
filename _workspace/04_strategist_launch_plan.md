# GitHub Trending 综合发布/上线计划 — Harness

> **项目**: Harness — Agent Team & Skill Architect (Claude Code Plugin)
> **GitHub**: https://github.com/revfactory/harness
> **编写日期**: 2026-03-29
> **发布/上线目标日**: 2026-04-07 (周二)
> **编写依据**: repo-auditor, content-creator, community-scout 产出物综合

---

## 1. 战略摘要

### 目标
- **主要目标**: 进入 GitHub Trending (All Languages) Top 25
- **次要目标**: 登顶 GitHub Trending (Markdown / Misc) #1
- **冲刺目标**: Hacker News Front Page 保持 12 小时以上

### 目标类别
- GitHub Trending: `All Languages` + `Unknown languages` (由于基于 Markdown，语言分类很可能被归为 Unknown/Misc)
- Hacker News: `Show HN`
- Product Hunt: `AI Coding Agents` / `Developer Tools`

### 核心 KPI

| 指标 | D-Day +6h | D-Day +24h | D+3 | D+7 | D+14 |
|------|-----------|------------|-----|-----|------|
| GitHub Stars | 100+ | 300+ | 500+ | 800+ | 1,200+ |
| Star 增速 (stars/h) | 15-20 | 10-15 | 5-8 | 3-5 | 2-3 |
| Forks | 10+ | 30+ | 50+ | 80+ | 120+ |
| HN Points | 50+ | 100+ | — | — | — |
| Twitter 曝光 | 10k+ | 50k+ | 100k+ | — | — |

### 发布/上线日选定依据
- **周二 (2026-04-07)** 选定
  - 周二至周四为进入 GitHub Trending 最优日期 (在周末流量下降前获取动量)
  - 排除周一: 周初工作过载导致开发者注意力分散
  - HN 最佳发帖时段与周二最为匹配
  - D-7 = 3/31(周二)，准备期以工作日为主，工作效率较高

---

## 2. D-7 ~ D-1: 发布前准备

### D-7 (3/31 二) — 仓库核心优化

| # | 行动 | 依据 | 完成标准 | 时间 |
|---|------|------|----------|------|
| 1 | **创建 GitHub Releases** (v1.0.0, v1.0.1) | Audit R4 — 侧边栏信任信号，最高 ROI | Release 页面显示 2 个 Release，包含 CHANGELOG 内容 | 30分钟 |
| 2 | **设置 GitHub Topics** | Audit R9 — 搜索/Explore 曝光核心 | 完成 `claude-code`, `claude-code-plugin`, `agent-team`, `ai-agent`, `llm`, `skill-generation`, `orchestration`, `claude` 设置 | 10分钟 |
| 3 | **设置 Repo Description** | Audit R13 — 搜索结果曝光优化 | "Agent Team & Skill Architect — A Claude Code Plugin that designs domain-specific agent teams and generates skills" | 5分钟 |
| 4 | **上传 Social Preview 图片** | Audit R14 — SNS 分享时的视觉冲击 | 上传 1280x640px OG 图片完成 | 30分钟 |
| 5 | **部署 GitHub Pages** | Audit R12 — 落地页上线 + 信任信号 | `revfactory.github.io/harness` 可访问 | 20分钟 |
| 6 | **在 Repo About 中设置网站 URL** | Audit R15 | 关联 Pages URL | 5分钟 |

**D-7 耗时**: ~2小时
**D-7 效果**: Score 5.5 → 7.0 (可发现性大幅改善)

### D-6 (4/1 三) — 社区基础设施

| # | 行动 | 依据 | 完成标准 | 时间 |
|---|------|------|----------|------|
| 7 | **添加 Issue 模板** (`bug_report.yml`, `feature_request.yml`) | Audit R5 | `.github/ISSUE_TEMPLATE/` 中 2 个文件 | 30分钟 |
| 8 | **添加 PR 模板** | Audit R6 | 创建 `.github/PULL_REQUEST_TEMPLATE.md` | 15分钟 |
| 9 | **编写 CONTRIBUTING.md** | Audit R7 — 引导新访客贡献的核心 | 包含 Bug 报告、PR 指南、开发环境部分 | 30分钟 |
| 10 | **添加 CODE_OF_CONDUCT.md** | Audit R8 | 应用 Contributor Covenant 模板 | 10分钟 |
| 11 | **README "Quick Start" 部分重命名** | Audit R2 | 安装部分 → "Quick Start" (≤5 步，首步为一行命令) | 15分钟 |
| 12 | **README 添加 "Contributing" 部分** | Audit R3 | 包含 CONTRIBUTING.md 链接 | 10分钟 |

**D-6 耗时**: ~2小时
**D-6 效果**: Score 7.0 → 7.5 (Repo Structure 大幅改善)

### D-5 (4/2 四) — 信任信号 & CI

| # | 行动 | 依据 | 完成标准 | 时间 |
|---|------|------|----------|------|
| 13 | **添加 GitHub Actions CI 工作流** | Audit R10 — 绿色 CI 徽章是强信任信号 | Markdown lint + plugin.json 验证 + 在 README 中添加徽章 | 1小时 |
| 14 | **添加基本有效性验证测试** | Audit R11 | plugin.json schema 验证，SKILL.md 存在确认，references 目录验证 | 1小时 |

**D-5 耗时**: ~2小时
**D-5 效果**: Score 7.5 → 8.0 (信任信号大幅改善)

### D-4 (4/3 五) — 内容准备 & 提前建立关系

| # | 行动 | 分类 | 完成标准 | 时间 |
|---|------|------|----------|------|
| 15 | **制作演示 GIF/录屏** | Audit R1 — 对 3 秒判断起决定性作用 | "build a harness" 执行 → 智能体团队生成过程 30-60 秒 GIF | 2小时 |
| 16 | **将演示 GIF 放置在 README 顶部** | | 紧跟在 tagline 下方 | 15分钟 |
| 17 | **准备 Twitter/X 线程用图片 3-4 张** | Content #3 | 工作流图、A/B 结果图表、架构模式卡片 | 1小时 |
| 18 | **最终审校所有内容文本** | | 确认 HN/Reddit/Twitter/Dev.to 草稿语法 + 链接 | 30分钟 |
| 19 | **开始联系 Product Hunt Hunter** | Scout — PH 发布必备 | 向 2-3 名 Hunter 发送 DM，分享预计发布日期 | 30分钟 |
| 20 | **提前 DM 影响者** | Scout Tier 1 KOL | 向 Nick Saraev、Boris Cherny 发送预介绍消息 | 30分钟 |

**D-4 耗时**: ~5小时 (工作量最大的一天)

### D-3 ~ D-2 (4/4~4/5 六~日) — 最终检查 & 支持者准备

| # | 行动 | 完成标准 | 时间 |
|---|------|----------|------|
| 21 | **确认 harness-100 仓库公开** | README 整理完毕，链接正常工作 | 30分钟 |
| 22 | **确认 claude-code-harness (研究仓库) 公开** | A/B 测试数据/论文可访问 | 30分钟 |
| 23 | **组建初始支持者小组** | 邀请同事/熟人 5-10 人协助在发布日 star/upvote。考虑时区，确保 KST 晚间~夜间时段有可用人员 | 1小时 |
| 24 | **预先准备 HN 预期问答** | 方法学局限、与其他工具比较、泛化可能性等 5-7 个 Q&A 草稿 | 1小时 |
| 25 | **确认 Anthropic Discord 账户活跃** | 确认可访问 #showcase 频道 | 15分钟 |

### D-1 (4/6 一) — 最终彩排

| # | 行动 | 完成标准 | 时间 |
|---|------|----------|------|
| 26 | **最终确认全部检查清单** (参见 Section 8) | 所有发布前项目已勾选 | 30分钟 |
| 27 | **最终确认 GitHub Pages 线上状态** | 落地页可访问 + 多语言切换正常工作 | 10分钟 |
| 28 | **将各平台发布文本复制到文本编辑器** | 粘贴准备完成 (防止格式混乱) | 20分钟 |
| 29 | **更新 Twitter 个人简介** | 添加 Harness 相关内容 | 5分钟 |
| 30 | **设置闹钟** | D-Day 起床闹钟 (KST 16:00 = UTC 07:00) | 5分钟 |

---

## 3. D-Day: 发布执行 (4/7 二)

### 时段战略

核心原则: **美国上午 (UTC 13:00-16:00 = PST 6-9am = EST 9am-12pm) HN/Reddit 流量达到峰值**。但 HN 需要**提前几小时发布，以便 Front Page 到达时间与峰值吻合**。以 KST 为基准是晚间~夜间时段。

| 时间 (UTC) | 时间 (KST) | 平台 | 行动 | 预期效果 | Plan B |
|-----------|-----------|------|------|----------|--------|
| **07:00** | **16:00** | GitHub | 最终确认 README，确认演示 GIF 正常工作 | 基本检查 | — |
| **08:00** | **17:00** | **Hacker News** | 发布 Show HN 帖子 (Content #1 文本) | HN New → Rising。初始 2-3 个 upvote 进入 Rising | 发布时机不佳则在 UTC 12:00 重新发布 (HN 允许重新发布) |
| **08:15** | **17:15** | **初始支持者** | 向支持者小组分享 HN 链接 + GitHub 链接 | 初始 5-10 stars + 2-3 HN upvotes (临界动量) | 支持者不足时额外动员个人网络 |
| **08:30** | **17:30** | **Twitter/X** | 发布推文线程 (Content #3, Tweet 1-9)。固定 Tweet 1 | 粉丝初始 engagement，转发扩散 | 线程 engagement 低则仅重发核心推文 |
| **09:00** | **18:00** | **r/ClaudeAI** | 发布 Reddit 帖子 (Content #2a) | 最 receptive 的目标社区。目标 50+ upvotes | 帖子被删时联系版主调整语气后重新发布 |
| **09:00** | **18:00** | **r/SideProject** | 发布 Reddit 帖子 (Scout Reddit 模板 D) | 允许自我推广的社区，安全的初始 traction | — |
| **09:30** | **18:30** | **r/opensource** | 发布 Reddit 帖子 | 开源社区曝光 | — |
| **10:00** | **19:00** | **r/programming** | 发布 Reddit 帖子 (Content #2c) | 广泛的开发者受众。间隔 1 小时以防止垃圾标记 | 被删时以强调技术价值的版本重写 |
| **10:00** | **19:00** | **Product Hunt** | PH 发布 (Hunter 或直接) — "AI Coding Agents" 类别 | 目标 PH Daily Top 5 | 未获得 Hunter 时直接发布/上线，立即撰写 Maker comment |
| **10:30** | **19:30** | **Anthropic Discord** | 在 #showcase 频道分享项目 | 直接触达 Claude 社区 | 备选频道 #community-projects |
| **12:00** | **21:00** | **Dev.to** | 发布技术博客文章 (Content #4) | 长期 SEO，搜索引流 | — |
| **12:00** | **21:00** | **HN 监控** | 确认是否进入 Front Page。开始回复评论 | 评论 engagement 是 HN 排名关键 | 未进入 Front Page 时 → 集中于 Reddit/Twitter |
| **13:00-18:00** | **22:00-03:00** | **全平台** | 实时监控 + 回复评论 (详见下方) | 美国峰值时段，最重要的 6 小时 | 疲劳时仅集中核心平台 (HN + r/ClaudeAI) |
| **14:00** | **23:00** | **Awesome Lists Tier 1** | 提交 awesome-claude-code (hesreallyhim) issue + jqueryscript PR + awesome-claude-skills 2 件 PR | 趁进入 Trending 同时提交 PR → 最大化通过率 | — |

### 实时监控指标 & 阈值

| 指标 | 检查周期 | 绿色 (正常) | 黄色 (注意) | 红色 (紧急) |
|------|----------|------------|------------|------------|
| Star 增速 | 30分钟 | >10 stars/h | 5-10 stars/h | <5 stars/h |
| HN 排名 | 30分钟 | Front Page (1-30) | 第 2 页 (31-60) | 第 3 页+ |
| HN 评论 | 1小时 | 5+ comments/h | 2-4 comments/h | <2 comments/h |
| Reddit upvotes (r/ClaudeAI) | 1小时 | 50+ | 20-50 | <20 |
| Twitter 线程曝光 | 2小时 | 5k+ | 1-5k | <1k |

### 紧急应对场景 (Plan B)

| 场景 | 触发条件 | 立即应对 |
|------|----------|----------|
| HN 发布后 3 小时内未进入 Front Page | UTC 11:00 时 Points < 10 | 集中于 Reddit/Twitter。考虑次日重新发布 HN |
| Reddit 帖子被删除 | 收到版主删除通知 | 重新确认该子版块规则 → 修改语气后通过 modmail 请求重新批准 |
| Star 增速 < 5/h (全渠道) | D-Day +6h 时点 | 紧急 DM 影响者 (Nick Saraev, Boris Cherny) + 支持者第二轮动员 |
| 负面评论较多 | 批评评论 3+ | 立即礼貌回复。承认技术局限 + 分享路线图。绝对禁止防御性态度 |

---

## 4. D+1 ~ D+3: 扩大传播

### D+1 (4/8 三)

| 时间 (UTC) | 行动 | 平台 | 触发条件 |
|-----------|------|------|----------|
| 08:00 | **r/MachineLearning 帖子** (Content #2b — 研究为主) | Reddit | 无条件执行 |
| 10:00 | **Twitter 引用推文**: "研究结果中最令人惊讶的部分..." — 强调 scaling insight | Twitter/X | 无条件执行 |
| 10:00 | **Ben's Bites 社区投票提交** | Newsletter | 无条件执行 |
| 12:00 | **Anthropic Discord 后续帖子** — 分享社区反响 | Discord | Stars 达到 100+ 时 |
| 14:00 | **HN 评论深度回复** — 对技术问题详细回答 | HN | HN Front Page 持续保持中 |
| Ongoing | **所有 GitHub Issues 在 24 小时内回复** | GitHub | 收到 Issue 时 |

### D+2 (4/9 四)

| 时间 (UTC) | 行动 | 平台 | 触发条件 |
|-----------|------|------|----------|
| 08:00 | **联系 TLDR AI / TLDR Open Source 编辑团队** | Email | Stars 200+ 时加强推介 |
| 10:00 | **联系 The Rundown AI 编辑团队** | Email | 无条件执行 |
| 10:00 | **提交 Console.dev** | Web | 无条件执行 |
| 12:00 | **r/LocalLLaMA 跨版发帖** (技术深度版) | Reddit | r/ClaudeAI 反应正面时 |
| 14:00 | **r/artificial 帖子** | Reddit | 无条件执行 |

### D+3 (4/10 五)

| 时间 (UTC) | 行动 | 平台 | 触发条件 |
|-----------|------|------|----------|
| 08:00 | **Twitter: "100 harnesses" 独立帖子** — 介绍 companion repo | Twitter/X | 无条件执行 |
| 10:00 | **提交 Changelog Weekly** | Newsletter | 无条件执行 |
| 12:00 | **提交 AI Tool Report** | Newsletter | 无条件执行 |
| 14:00 | **在 Star History 注册项目** + Star 图表嵌入 README | GitHub/Web | Stars 300+ 时图表更有冲击力 |
| Ongoing | **里程碑推文**: "Launched 3 days ago → X stars" | Twitter/X | Stars 达到 500+ 时 |

---

## 5. D+4 ~ D+14: 持续维护

### D+4 ~ D+7 (4/11~4/14)

| 日期 | 行动 | 平台 |
|------|------|------|
| D+4 (六) | **提交 Awesome Lists Tier 2 PR**: awesome-llm-agents, awesome-agents, awesome-ai-agents, awesome-ai-agents-2026, Awesome-Prompt-Engineering | GitHub |
| D+5 (日) | **Dev.to 后续文章**: "5 Architecture Patterns for AI Agent Teams" (常青内容) | Dev.to |
| D+5 | **联系 YouTube 创作者**: Nick Saraev (最优先)，Sabrina Ramonov，freeCodeCamp | Email/DM |
| D+6 (一) | **确认 Trendshift 徽章并嵌入 README** | GitHub |
| D+7 (二) | **每周里程碑推文**: "Week 1: X stars, Y forks, Z countries" + Star History 图表 | Twitter/X |
| D+7 | **注册 awesomeclaude.ai** | Web |

### D+8 ~ D+14 (4/15~4/21)

| 日期 | 行动 | 平台 |
|------|------|------|
| D+8~10 | **提交 Awesome Lists Tier 3 PR**: jim-schwoebel, webfuse-com, BehiSecc, alvinunreal | GitHub |
| D+10 | **联系 Superhuman AI、The Neuron newsletter** | Email |
| D+10 | **r/MachineLearning 项目展示后续** (实际使用案例 + 反映社区反馈) | Reddit |
| D+12 | **Twitter: 用户反馈亮点线程** — "Here's what people are building with Harness" | Twitter/X |
| D+14 | **2 周里程碑推文** + 分享路线图 (下一版本计划) | Twitter/X |
| Ongoing | **所有 GitHub Issues/PR 在 48 小时内回复** | GitHub |
| Ongoing | **回复所有 Twitter 提及/引用** | Twitter/X |

### 长期社区维护原则

1. **Issue 响应 24-48 小时规则**: Trending 后新访客提交第一个 Issue 时的快速回复是社区形成的关键
2. **"Good First Issue" 标签**: 刻意创建简单贡献机会以吸引贡献者
3. **持续更新 CHANGELOG**: 每个发布/上线附详细说明以营造活跃项目印象
4. **月度更新帖子**: 在 r/ClaudeAI + Twitter 分享进展

---

## 6. 成功指标

### Star 增速目标 (按时段)

| 时点 | 累计 Stars | 增速 (stars/h) | Trending 门槛标准 |
|------|-----------|----------------|-------------------|
| D-Day +2h | 20-30 | 10-15 | 开始进入 |
| D-Day +6h | 80-120 | 15-20 | Daily Trending 候选 |
| D-Day +12h | 150-200 | 10-15 | 进入 Daily Trending |
| D-Day +24h | 250-350 | 8-12 | Daily Trending 稳定 |
| D+2 | 400-500 | 5-8 | Weekly Trending 候选 |
| D+3 | 500-600 | 4-6 | 进入 Weekly Trending |
| D+7 | 700-900 | 2-4 | 保持 Weekly Trending |
| D+14 | 1,000-1,500 | 1-3 | Monthly Trending |

### 综合成功指标

| 指标 | D-Day | D+3 | D+7 | D+14 | 备注 |
|------|-------|-----|-----|------|------|
| **GitHub Stars** | 300+ | 500+ | 800+ | 1,200+ | 核心指标 |
| **GitHub Forks** | 30+ | 50+ | 80+ | 120+ | 实际使用 proxy |
| **GitHub Trending 排名** | Top 50 | Top 25 | 保持 Top 25 或重新进入 | — | 主要目标 |
| **HN Points** | 100+ | — | — | — | Front Page = ~50+ points |
| **HN Comments** | 30+ | — | — | — | Engagement 深度 |
| **Product Hunt 排名** | Top 10 | — | — | — | Daily 排名 |
| **Reddit 总 Upvotes** | 200+ | 400+ | — | — | 所有子版块合计 |
| **Twitter 曝光** | 50k+ | 100k+ | 150k+ | — | 线程 + 提及 |
| **Awesome List PRs 合并数** | 2+ | 4+ | 6+ | 8+ | 长期可发现性 |
| **GitHub Issues 开启数** | 5+ | 10+ | 20+ | 30+ | 社区健康指标 |
| **Contributors** | 1 | 2+ | 3+ | 5+ | 社区形成 |

---

## 7. 风险 & 缓解

| # | 风险 | 概率 | 影响 | 缓解战略 |
|---|------|------|------|----------|
| 1 | **HN 冷遇** — Show HN 未能进入 Front Page | 中 | 高 | HN 有赌注成分。失败时集中于 Reddit/Twitter。考虑 D+2~3 重新发布。也参与 "Ask HN: What are you working on?" 月度讨论帖 |
| 2 | **Reddit 自我推广被删** — 版主删除帖子 | 中 | 中 | 对每个子版块使用差异化语气 (r/ClaudeAI 用使用案例，r/programming 用技术价值，r/MachineLearning 用研究)。发帖历史中有该子版块参与记录更有利 → 发布前 1 周在该子版块正常参与 |
| 3 | **Star 增速不足** — 未达 Trending 门槛 (D-Day +12h 时 < 100 stars) | 中 | 高 | Plan B: 紧急 DM 影响者 + 支持者第二轮动员 + 额外利用亚洲时区 (韩国/日本) 社区。已有日语 README，可向日本开发者社区 (Zenn, Qiita) 发日语帖子 |
| 4 | **负面技术反馈** — "不就是 prompt 吗？"、"A/B 测试方法存疑" | 中 | 中 | 用预准备的 Q&A 礼貌且技术性地回复。坦诚承认局限，但用实际结果数据支撑。"You're right that X is a limitation — here's what we're working on for v2" |
| 5 | **Product Hunt 表现平淡** — 未进入 Daily Top 10 | 中 | 低 | PH 为辅助渠道。表现平淡对 GitHub Trending 无直接影响。将精力集中于 HN/Reddit 而非 PH |
| 6 | **竞争项目同期发布** — 同一天类似 AI agent 工具进入 Trending | 低 | 中 | 强调 Harness 差异化 (meta-skill，A/B 研究结果，100 个预构建)。准备清晰说明与竞争项目差异的评论 |
| 7 | **作者时区不利** — KST 与美国峰值时段 12-14 小时差导致实时应对延迟 | 高 | 中 | D-Day 晚间~凌晨集中监控计划 (KST 17:00 ~ 03:00 = UTC 08:00 ~ 18:00)。D+1 上午开始追赶评论回复。睡前留一条 "I'll respond to all questions in the morning" 评论 |
| 8 | **GitHub 服务故障** — 发布当天 GitHub 宕机/缓慢 | 低 | 高 | 提前监控 githubstatus.com。故障时将发布推迟到次日。内容已准备就绪可复用 |

---

## 8. 最终检查清单

### 发布前 (D-1 前完成)

**仓库优化**
- [ ] 创建 GitHub Releases v1.0.0, v1.0.1
- [ ] 设置 GitHub Topics 8 个
- [ ] 设置 Repo Description
- [ ] 上传 Social Preview 图片 (1280x640)
- [ ] 部署 GitHub Pages + 在 About 中设置 URL
- [ ] 添加 Issue 模板 (bug_report.yml, feature_request.yml)
- [ ] 添加 PR 模板
- [ ] 编写 CONTRIBUTING.md
- [ ] 添加 CODE_OF_CONDUCT.md
- [ ] 添加 CI 工作流 + 绿色徽章
- [ ] 添加有效性验证测试
- [ ] README "Quick Start" 部分重命名
- [ ] README 添加 "Contributing" 部分
- [ ] 制作演示 GIF + 放置在 README 顶部

**内容**
- [ ] 准备 HN Show HN 文本最终版
- [ ] 准备 Reddit 帖子 4 种 (r/ClaudeAI, r/programming, r/SideProject, r/opensource) 最终版
- [ ] 准备 Twitter/X 线程 9 条推文 + 图片 3-4 张
- [ ] 准备 Dev.to 文章最终版
- [ ] 准备 Product Hunt 发布/上线页面草稿 (tagline, description, images)
- [ ] 确认所有内容链接正常工作

**外联**
- [ ] 确定 Product Hunt Hunter (或决定直接发布/上线)
- [ ] 向 Nick Saraev、Boris Cherny 发送预 DM
- [ ] 确保初始支持者 5-10 人 + 确认 D-Day 参与
- [ ] 完成 HN 预期 Q&A 5-7 个草稿
- [ ] 确认 Anthropic Discord 访问

**仓库关联**
- [ ] 公开 harness-100 仓库 + 整理 README
- [ ] 公开 claude-code-harness (研究仓库)
- [ ] 确认 3 个仓库间交叉链接正常
- [ ] 更新 Twitter 个人简介

### D-Day (4/7 二)

**发布顺序**
- [ ] UTC 08:00 — 发布 HN Show HN
- [ ] UTC 08:15 — 向支持者小组分享链接
- [ ] UTC 08:30 — 发布 Twitter/X 线程 + 置顶
- [ ] UTC 09:00 — 发布 r/ClaudeAI + r/SideProject
- [ ] UTC 09:30 — 发布 r/opensource
- [ ] UTC 10:00 — 发布 r/programming + Product Hunt
- [ ] UTC 10:30 — 发布 Anthropic Discord #showcase
- [ ] UTC 12:00 — 发布 Dev.to 文章
- [ ] UTC 14:00 — 提交 Awesome Lists Tier 1 PR/Issue (4 件)

**监控**
- [ ] 每 30 分钟检查 Star 增速
- [ ] 每 30 分钟检查 HN 排名
- [ ] 每 1 小时检查 Reddit upvotes
- [ ] 所有评论在 1 小时内回复
- [ ] 立即回复 GitHub Issues

### 发布后 (D+1 ~ D+14)

- [ ] D+1: r/MachineLearning 帖子 + Twitter QT + 提交 Ben's Bites
- [ ] D+2: 联系 TLDR/Rundown/Console.dev
- [ ] D+3: "100 harnesses" 独立帖子 + 提交 Changelog + 注册 Star History
- [ ] D+4: Awesome Lists Tier 2 PR 5 件
- [ ] D+5: Dev.to 后续文章 + 联系 YouTube 创作者
- [ ] D+7: 每周里程碑推文
- [ ] D+8~10: Awesome Lists Tier 3 PR 4 件 + 联系 Newsletter Tier 3
- [ ] D+14: 2 周里程碑 + 分享路线图

---

## 连锁效应 (Cascade) 设计

```
D-Day 08:00  HN Show HN ──────────────┐
D-Day 08:30  Twitter Thread ────────────┤
D-Day 09:00  Reddit (r/ClaudeAI) ───────┤
D-Day 10:00  Product Hunt ──────────────┤
                                        ▼
                              初始 Star Velocity
                              (30-50 stars in 3h)
                                        │
                                        ▼
                              GitHub Trending 算法检测
                              (Daily Trending 候选)
                                        │
                          ┌─────────────┼─────────────┐
                          ▼             ▼             ▼
                    Trendshift       Alpha Signal   GitNews
                    自动收集          自动跟踪      自动收集
                          │             │             │
                          ▼             ▼             ▼
                    二次流量涌入 → Star Velocity 加速
                                        │
                                        ▼
                              GitHub Trending 稳定进入
                              (Top 25)
                                        │
                          ┌─────────────┼─────────────┐
                          ▼             ▼             ▼
                    TLDR AI 收录    Newsletter     Awesome List
                    (D+2~3)        报道 (D+3~7)   PR 通过促进
                          │             │             │
                          ▼             ▼             ▼
                    三次流量 → 确保长期可发现性 → 1,000+ Stars
```

**核心**: 每个阶段是下一阶段的触发器。最初 24 小时的 Star velocity 是整个连锁反应的点火条件。

---

## 执行摘要

| 阶段 | 期间 | 核心目标 | 成功标准 |
|------|------|----------|----------|
| **发布前** | D-7 ~ D-1 | Repo Score 5.5 → 8.0 | 检查清单 100% 完成 |
| **发布** | D-Day | Star velocity > 10/h | 24h 内 300+ stars |
| **扩大传播** | D+1 ~ D+3 | 进入 Trending + 二次扩散 | 进入 Top 25 + 500+ stars |
| **持续维护** | D+4 ~ D+14 | 确保长期可发现性 | 1,200+ stars + Awesome List 8+ merged |

> **最终目标**: 将 Harness 定位为 "Claude Code Plugin 的代表性案例"，通过 GitHub Trending 在全球 AI 开发者社区中建立知名度。
