# QA 智能体设计指南

在构建 Harness 中包含 QA 智能体时参考的指南。基于实际项目 (SatangSlide) 中发现的 Bug 模式及其根本原因分析，提供系统性地捕获 QA 容易遗漏缺陷的验证方法论。

---

## 目录

1. QA 智能体遗漏的缺陷模式
2. 集成一致性验证 (Integration Coherence Verification)
3. QA 智能体设计原则
4. 验证检查清单模板
5. QA 智能体定义模板

---

## 1. QA 智能体遗漏的缺陷模式

### 1-1. 边界不匹配 (Boundary Mismatch)

最常见的缺陷。两个组件各自"正确"实现，但连接点的契约不一致。

| 边界 | 不匹配示例 | 遗漏原因 |
|------|-----------|----------|
| API 响应 → 前端 Hook | API 返回 `{ projects: [...] }`，Hook 期望 `SlideProject[]` | 各自单独验证正常，不进行交叉比对 |
| API 响应字段名 → 类型定义 | API 使用 `thumbnailUrl`(camelCase)，类型使用 `thumbnail_url`(snake_case) | TypeScript 泛型类型转换后编译器无法检测 |
| 文件路径 → 链接 href | 页面在 `/dashboard/create`，链接却指向 `/create` | 不交叉比对文件结构与 href |
| 状态转换映射 → 实际 status 更新 | 映射定义了 `generating_template → template_approved`，代码中缺少转换 | 仅确认映射存在，不追踪所有更新代码 |
| API 端点 → 前端 Hook | API 存在但无对应 Hook (未被调用) | 不将 API 列表与 Hook 列表进行 1:1 映射 |
| 即时响应 → 异步结果 | API 即时返回 `{ status }`，前端访问 `data.failedIndices` | 不区分同步/异步响应，仅检查类型 |

### 1-2. 为什么静态代码审查无法捕获

- **TypeScript 泛型的局限**: `fetchJson<SlideProject[]>()` — 即使运行时响应是 `{ projects: [...] }` 也能通过编译
- **`npm run build` 通过 ≠ 正常运行**: 使用了类型转换、`any`、泛型时构建成功但运行时失败
- **存在验证 vs 连接验证的区别**: "API 是否存在？" 与 "API 的响应是否符合调用方的期望？" 是完全不同的验证

---

## 2. 集成一致性验证 (Integration Coherence Verification)

QA 智能体必须包含的**交叉比对验证**领域。

### 2-1. API 响应 ↔ 前端 Hook 类型交叉验证

**方法**: 比较每个 API route 的 `NextResponse.json()` 调用处与对应 Hook 的 `fetchJson<T>` 类型参数。

```
验证步骤:
1. 从 API route 的 NextResponse.json() 中提取传入对象的 shape
2. 确认对应 Hook 中 fetchJson<T> 的 T 类型
3. 比较 shape 与 T 是否一致
4. 确认包装处理 (如果 API 返回 { data: [...] }，Hook 是否提取了 .data)
```

**特别注意的模式:**
- 分页 API: `{ items: [], total, page }` vs 前端期望数组
- snake_case DB 字段 → camelCase API 响应 → 前端类型定义之间的不一致
- 即时响应 (202 Accepted) vs 最终结果的 shape 差异

### 2-2. 文件路径 ↔ 链接/路由路径映射

**方法**: 从 `src/app/` 下的 page 文件提取 URL 路径，与代码中所有 `href`、`router.push()`、`redirect()` 值进行对照。

```
验证步骤:
1. 从 src/app/ 下的 page.tsx 文件路径中提取 URL 模式
   - (group) → URL 中移除
   - [param] → 动态段
2. 收集代码中所有 href=, router.push(, redirect( 值
3. 确认每个链接是否匹配实际存在的 page 路径
4. 注意 route group 内页面的 URL 前缀 (如 dashboard/ 下)
```

### 2-3. 状态转换完整性追踪

**方法**: 从代码中提取所有 `status:` 更新，与状态转换映射进行对照。

```
验证步骤:
1. 从状态转换映射 (STATE_TRANSITIONS) 中提取允许的转换列表
2. 在所有 API route 中搜索 .update({ status: "..." }) 模式
3. 确认每个转换是否在映射中定义
4. 识别映射中定义但代码中未执行的转换 (死转换)
5. 特别注意: 中间状态 (如 generating_template) 到最终状态 (template_approved) 的转换是否遗漏
```

### 2-4. API 端点 ↔ 前端 Hook 1:1 映射

**方法**: 列出所有 API route 和前端 Hook，确认配对情况。

```
验证步骤:
1. 从 src/app/api/ 下的 route.ts 中按 HTTP 方法提取端点列表
2. 从 src/hooks/ 下的 use*.ts 中提取 fetch 调用 URL 列表
3. 识别 API 端点中 Hook 未调用的部分 → 标记为 "未使用"
4. 判断 "未使用" 是有意为之 (如管理 API) 还是调用遗漏
```

---

## 3. QA 智能体设计原则

### 3-1. 使用 general-purpose 类型而非 Explore 类型

QA 智能体使用 `Explore` 类型则只能读取。但有效的 QA 需要:
- 使用 Grep 搜索模式 (提取所有 `NextResponse.json()`)
- 执行脚本进行自动对照 (API shape vs Hook 类型)
- 必要时还能修改

**推荐**: 设置为 `general-purpose` 类型，在智能体定义中明确 "验证 → 报告 → 修改请求" 协议。

### 3-2. 检查清单应优先 "交叉比对" 而非 "存在确认"

| 弱检查清单 | 强检查清单 |
|-----------|-----------|
| API 端点是否存在？ | API 端点的响应 shape 与对应 Hook 的类型是否一致？ |
| 状态转换映射是否已定义？ | 所有 status 更新代码是否与映射中的转换一致？ |
| 页面文件是否存在？ | 代码中所有链接是否指向实际存在的页面？ |
| 是否使用 TypeScript strict mode？ | 是否存在通过泛型转换绕过的类型安全？ |

### 3-3. "同时读取两边" 原则

QA 要捕获边界 Bug，不能只读一边。必须:
- 同时读取 API route **和** 对应 Hook
- 同时读取状态转换映射 **和** 实际更新代码
- 同时读取文件结构 **和** 链接路径

在智能体定义中明确记载此原则。

### 3-4. QA 应在每个模块完成后立即执行，而非构建完成后

如果编排器将 QA 仅放在 "Phase 4: 全部完成后":
- Bug 累积导致修复成本升高
- 初期边界不一致传播到后续模块

**推荐模式**: 每个后端 API 完成时立即进行该 API + 对应 Hook 的交叉验证 (增量 QA)。

---

## 4. 验证检查清单模板

包含在 QA 智能体定义中的 Web 应用集成一致性检查清单。

```markdown
### 集成一致性验证 (Web 应用)

#### API ↔ 前端连接
- [ ] 所有 API route 的响应 shape 与对应 Hook 的泛型类型一致
- [ ] 包装响应 ({ items: [...] }) 确认 Hook 中进行了 unwrap
- [ ] snake_case ↔ camelCase 转换一致应用
- [ ] 即时响应 (202) 与最终结果的 shape 在前端有区分
- [ ] 所有 API 端点都有对应的前端 Hook 且实际被调用

#### 路由一致性
- [ ] 代码中所有 href/router.push 值匹配实际 page 文件路径
- [ ] route group ((group)) 在 URL 中被移除已在路径验证中考虑
- [ ] 动态段 ([id]) 使用正确的参数填充

#### 状态机一致性
- [ ] 定义的所有状态转换在代码中都有执行 (无死转换)
- [ ] 代码中所有 status 更新都在转换映射中定义 (无未授权转换)
- [ ] 中间状态到最终状态的转换没有遗漏
- [ ] 前端基于状态的分支 (if status === "X") 中的 X 实际可达

#### 数据流一致性
- [ ] DB Schema 字段名与 API 响应字段名的映射一致
- [ ] 前端类型定义与 API 响应的字段名一致
- [ ] 可选字段的 null/undefined 处理在两边一致
```

---

## 5. QA 智能体定义模板

构建 Harness 的 QA 智能体中应包含的核心部分。

```markdown
---
name: qa-inspector
description: "QA 验证专家。验证规格合规性、集成一致性、设计质量。"
---

# QA Inspector

## 核心角色
验证规格对比实现质量和**模块间集成一致性**。

## 验证优先级

1. **集成一致性** (最高) — 边界不一致是运行时错误的主要原因
2. **功能规格合规性** — API/状态机/数据模型
3. **设计质量** — 颜色/排版/响应式
4. **代码质量** — 未使用代码，命名规范

## 验证方法: "两边同时读取"

边界验证必须**同时打开两边代码**进行比较:

| 验证对象 | 左侧 (生产者) | 右侧 (消费者) |
|---------|-------------|--------------|
| API 响应 shape | route.ts 的 NextResponse.json() | hooks/ 的 fetchJson<T> |
| 路由 | src/app/ page 文件路径 | href, router.push 值 |
| 状态转换 | STATE_TRANSITIONS 映射 | .update({ status }) 代码 |
| DB → API → UI | 表格列名 | API 响应字段 → 类型定义 |

## 团队通信协议

- 发现后立即向相应智能体发送具体修改请求 (文件:行号 + 修改方法)
- 边界问题通知**双方**智能体
- 向领导者: 验证报告 (区分通过/失败/未验证项)
```

---

## 实际案例: SatangSlide 中发现的 Bug

本指南的所有内容均来自以下实际 Bug 中提取的教训:

| Bug | 边界 | 原因 |
|-----|------|------|
| `projects?.filter is not a function` | API→Hook | API 返回 `{projects:[]}`，Hook 期望数组 |
| 仪表盘所有链接 404 | 文件路径→href | 缺少 `/dashboard/` 前缀 |
| 主题图片不显示 | API→组件 | `thumbnailUrl` vs `thumbnail_url` |
| 主题选择保存无效 | API→Hook | select-theme API 存在，但无 Hook |
| 生成页面永远等待 | 状态转换→代码 | `template_approved` 转换代码缺失 |
| `data.failedIndices` 崩溃 | 即时响应→前端 | 在即时响应中访问后台结果 |
| 完成后查看幻灯片 404 | 文件路径→href | `/projects/` → `/dashboard/projects/` |
