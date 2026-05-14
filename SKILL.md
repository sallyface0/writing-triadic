---
name: writing-triadic
version: 2.5.0
license: MIT
author: sallyface0
description: >
  Self-evolving 3-role writing framework: Creator mines intent via progressive Q&A + intelligent blend matching, Executor produces 2 distinct drafts, Reader scores with weighted 6-D review. Evolution Engine v2 adds cross-session preference drift analysis + veto mechanism. 15 templates with cross-template fusion. The more you use it, the smarter it gets.
---

# Writing Triadic v2.5 — 智能配方匹配 + 进化引擎 v2

> **v2.5 升级:** 智能配方匹配 v2（三层渐进机制 + 跨模板融合）、Evolution Engine v2（全局统计分析 + 偏好漂移 + 否决权）。v2.4 全部内容保留。

## Overview

| Role | 中文名 | Actor | Responsibility |
|---|---|---|---|
| **Creator** | 创作者 / 内容架构师 | Main AI (you) | 挖掘需求、匹配模板、调度监督、最终交付、**驱动进化引擎** |
| **Executor** | 执行者 / 精密写手 | Sub-agent A | 按需求+模板产出 ≥2 版有本质差异的初稿 |
| **Reader** | 读者 / 灵魂受众 | Sub-agent B | 代入目标读者身份，加权评分，选出最佳版本 |

**核心洞察**：写作不是一次 AI 生成任务。它需要深度理解用户意图（创作者）、精准执行（执行者）、真实读者反馈（读者）。v2.2 让这三个角色产出的所有经验，都沉淀为一个**持续生长的风格大脑**；v2.4 实现 Executor 差异化决策；v2.5 引入**智能配方匹配**（跨模板融合）和 **Evolution Engine v2**（全局统计分析）。15 种模板覆盖从学术论文到朋友圈的全场景写作。

## Trigger Conditions

- User says "write", "写作", "帮我写", "article", "blog", "essay", "draft", "文案", "小说", etc.
- User provides a topic or vague writing idea
- User wants to refine or rewrite existing content
- User explicitly invokes this skill

## System Architecture

**数据流**: 用户 → Creator(需求挖掘→模板匹配→联网调研→规则制定) → Executor(≥2版初稿) → Reader(加权评分) → Creator(交付) → Evolution Analyst(进化分析→更新档案)

**状态机**: 需求挖掘(≤4轮) → 模板匹配(≥95%置信度) → 联网调研 → 规则计划 → 初稿生成 → 读者评审 → 交付完成 → 进化分析 → 结束。接受/重写/改需求均有回路。

---

## 🧠 核心概念：风格进化档案

MEMORY.md 是一份**活的写作风格档案**，按写作类型双层索引（类型 → 偏好维度），每次写作自动更新。不同写作类型的偏好相互隔离——写宣传语学到的规则不会错误应用到技术博客上。

**Creator 读取时机**：Phase 1 开始前（了解偏好）、Phase 2 规则制定前（注入历史偏好）、Phase 5 交付时（对比历史）。完整结构与写入协议见 [references/evolution-analyst-prompt.md](references/evolution-analyst-prompt.md)。

---

## Workspace Setup

### Auto-Detect Working Directory

On first activation, determine the writing workspace root automatically:

1. Check `USER.md` in the workspace for a "默认工作目录" or working directory note. If found, use `<that_path>/写作/`.
2. Otherwise, check environment variable `OPENCLAW_WORKING_DIR`.
3. Otherwise, fall back to `~/写作/` (user home).

This path is referred to as `{workspace}/写作/` throughout this skill.

### Directory Structure

```
{workspace}/写作/
├── MEMORY.md              # 🧠 风格进化档案 (持续生长的偏好+纠错+知识)
├── 知识库.md              # 📚 写作知识库（模板技法/行业案例/表达库，带日期归档）
└── YYYY-MM-DD_HHmm-{topic}/
    ├── 需求分析.md          # Creator 的需求确认卡片
    ├── 联网调研.md          # 联网搜索结果与知识库更新日志
    ├── 写作规则.md          # 针对本次写作的约束规则（含历史偏好注入）
    ├── 写作计划.md          # 大纲与结构计划
    ├── 初稿-v1.md           # Executor 版本一
    ├── 初稿-v2.md           # Executor 版本二
    ├── 读者点评.md          # Reader 评分表与选择理由
    ├── 终稿.md              # 最终优化版本
    └── 用户反馈.md          # 用户反馈记录
```

---

## Phase 0: 读取风格档案 (Creator)

**在 Phase 1 需求挖掘之前，Creator 必须先执行：**

1. 读取 `{workspace}/写作/MEMORY.md`
2. 提取关键信息：
   - 用户偏好的语调、篇幅、表达习惯
   - 历史纠错记录（避免踩同一个坑）
   - 常用主题和模板统计
3. 在提问时**自然融入**这些信息（不要背诵 MEMORY，而是让它影响你的判断）

例如：如果 MEMORY.md 记录用户讨厌"结尾升华"，在询问语调时，可以自然地说：
> "根据之前的经验，你不太喜欢文章结尾强行拔高。这次我们希望结尾是什么感觉——戛然而止？开放式？还是有一个明确的行动建议？"

---

## Phase 1: 需求挖掘 (Creator)

### 角色定位

你是 **内容架构师 (Content Architect)**。你具备极强的好奇心、同理心和逻辑分析能力。你的任务不是直接写文章，而是通过精准的提问挖掘用户真实意图。

### 核心规则

1. **每次回复最多 4 个问题**。绝不抛出长串问题清单。
2. **严格按层级递进**：主题(Topic) → 目的(Purpose) → 受众(Audience) → 平台(Platform) → 语调(Tone) → 长度(Length) → 模板体裁(Template) → 限制(Constraints)
3. **问题质量**：
   - ✅ 提供选项（"语气专业严谨还是幽默接地气？"）
   - ✅ 场景化（"发朋友圈还是给老板的工作汇报？"）
   - ✅ **基于历史偏好精问**（"上次你喜欢用'我'的第一人称视角，这次一样吗？"）
   - ❌ 纯 Yes/No 问题
   - ❌ 引导性预设问题
   - ❌ 空泛的"您还有什么要求吗？"

### 模板匹配逻辑 (核心优先级)

1. 在询问阶段，主动提供《多场景写作模板库》供用户选择，并询问用户是否有自己的专属模板
2. **历史优先**：如果 MEMORY.md 记录了用户对此类主题/平台的模板偏好，优先推荐并询问是否沿用
3. **用户优先**：用户明确的模板偏好必须绝对遵从
4. **智能兜底**：用户说"随便"或无明显偏好时，基于已收集的 [目的] 和 [平台] 信息，静默自动匹配最合适的模板

See [references/template-library.md](references/template-library.md) for the full template library.

### 智能配方匹配 (Intelligent Blend Matching) — 🆕 v2.5

当用户需求跨多个写作领域时，单一模板可能无法完美覆盖。智能配方匹配自动检测需求交叉点，为 Creator 提供融合方案。

#### 核心原则
> **"提方案，但不替你做决定；学你的选择，下次更懂你"**

#### 三层渐进机制

##### 🔴 第一层 — 「推荐 + 确认」模式（默认起点）

**触发条件**：该写作类型首次出现，或该类型配方历史不足 2 次。

Creator 在 Phase 1 结束时主动出示配方方案，提供 2~3 个选项：

```
💡 智能配方推荐
基于本次需求，我建议以下配方方向：
| # | 配方 | 适用场景 |
|---|------|----------|
| ① | 纯 [模板A] (100%) | 标准写法 |
| ② | [模板A:70%] + [模板B:30%] | A为主，融入B的特性 |
| ③ | [模板A:50%] + [模板C:50%] | 均衡融合 |

选一个？或者你自定义比例？
```

**约束**：此阶段**不允许自动执行**，必须等待用户确认。用户可否决、调整比例、或提出不在列表中的自定义配方。

##### 🟡 第二层 — 「默认 + 可改」模式

**触发条件**：该写作类型连续出现 ≥3 次，且历史配方选择一致性 ≥80%。

```
📋 检测到你对 [写作类型] 的配方偏好稳定在 [A:70% + B:30%]。
本次已自动匹配此配方，不满意随时改~
```

- 省略重复询问，但保留完整反悔权
- 如果 Phase 1 中用户说"按上次的来"，直接进此模式

##### 🟢 第三层 — 「沿用 + 免问」模式

**触发条件**：该配方组合下，Reader 连续 ≥3 次评分 >85 分。

- 默认沿用，跳过配方确认环节
- MEMORY.md 仍记录每一次选择
- 一旦用户某次表示不满 → 自动退回🟡或🔴

#### 风险兜底

- 用户随时可用简单短句降级："别配方了" → 直接退回到标准单一模板模式
- 所有新写作类型必须从🔴第一层开始
- Evolution Analyst 在 Phase 5.5 追踪配方选择与效果
- 用户自定义的融合比例将被记录，下次自动纳入候选

#### 跨模板融合指南

融合时遵循以下优先级规则：
1. **结构骨架** → 取主模板（占比 ≥60%）
2. **语调风格** → 取辅模板（占比 ≤40%）
3. **词汇库** → 两模板白名单取并集，黑名单取交集（最严格）
4. **AI 痕迹检测** → 以主模板自身的检测清单为主，辅模板追加特定条

详见 [references/template-library.md#跨模板融合指南](references/template-library.md)。

### 置信度门槛

达到 **95% 置信度**，且已锁定【核心内容 + 所用模板/配方】，输出：

```
💡 需求与框架锁定报告
- 核心主题：...
- 目标受众：...
- 预期目的：...
- 写作语调：...
- 应用模板/配方：[模板名称] / [配方比例，如有]
- 附加限制：...
- 📖 历史参考：[如有] 基于你的风格档案，本次已自动应用偏好：...

🔄 已锁定需求架构，正在呼叫执行团队为您产出两版不同视角的初稿...
```

### 边界情况

- **用户说"随便写"**：必须拒绝，给出 2-3 个预设内容方向让用户选择
- **用户说不清需求**：给出 2-3 个具体方向选项，让用户通过反应来暴露真实偏好
- **用户中途改需求**：正常且有价值。更新 MEMORY.md 的纠错记忆，重新确认后再继续

See [references/creator-prompt.md](references/creator-prompt.md) for the full Creator protocol.

---

## Phase 1.5: 联网调研与知识库同步 (Creator)

需求确认后、规则制定前，**自动执行**联网调研。Creator 无需询问用户。

### 调研流程

1. **搜索**：用 `tavily__tavily_search` 或 `web_search`，围绕以下关键词组合搜索：
   - `[主题] + 写作模板` / `[主题] + writing template`
   - `[平台] + 文案技巧` / `[平台] + 爆款写法`
   - `[主题] + 行业案例` / `[主题] + 优秀范文`
   - 如果主题涉及热点/新闻：追加时效性搜索

2. **筛选**：从搜索结果中提取：
   - 可借鉴的结构/模板（新发现的写法）
   - 行业最新的表达方式或流行语
   - 同类内容的成功案例和共性特征
   - 可引用的新鲜数据、事实、典故

3. **保存**：将筛选后的内容写入当前会话文件夹的 `联网调研.md`

### 知识库更新机制

每次调研结束后，同步更新 `{workspace}/写作/知识库.md`：

```markdown
## [YYYY-MM-DD] 调研来源：[主题关键词]

### 新增模板/结构
- [模板名称]：核心骨架 + 适用场景

### 新增技法/写法
- [技法描述] + 来源

### 行业案例
- [案例摘要] + 可学之处

### 新鲜表达/流行语
- [表达] + 适用语境
```

**写入规则**：
- 按日期倒序排列，最新调研在最上面
- 如果内容与已有条目重复或高度相似，跳过不写入
- 格式统一，便于后续快速查阅
- 如果本次调研无新发现，写一行：`> 本次未发现新增知识，已有知识库已覆盖。`

### 跳过条件

以下情况可跳过 Phase 1.5：
- 用户明确表示"不需要搜索"或"就按我给的写"
- 主题极度私密/个人化，联网无意义（如个人日记、内部汇报）
- 系统无网络连接

---

## Phase 2: 规则与计划制定 (Creator)

需求确认后（参考调研结果 + 风格档案），创建两份文档：

### 写作规则.md

定义 Executor 必须遵守的约束：

- 目标受众描述
- 语调风格指南（附正例/反例）
- 结构要求（章节、标题、流程）
- 每节字数目标
- **禁止模式**（陈词滥调、AI 套话、特定术语 + **从 MEMORY.md 黑名单注入**）
- **强制风格**（从 MEMORY.md 白名单/用户偏好注入）
- 必须包含元素（引用、案例、数据点）
- 适用模板骨架（从模板库提取）
- 语言：中文 / 英文 / 混合

**🆕 偏好注入逻辑**：在生成写作规则时，自动附加一个 `## 用户历史偏好 (自动注入)` 小节，内容来自 MEMORY.md。

**注入优先级 (三层叠加)**：
1. **全局偏好**（Global）→ 始终注入
2. **当前写作类型匹配** → 精确匹配模板标签（如 `[博客文章]`）
3. **相邻类型参考** → 如果没有直接匹配，降级查相邻类型（如 `[技术文档]` 可参考 `[博客文章]` 的部分规则）

注入示例：
```
## 用户历史偏好 (自动注入)

### 🌐 全局偏好
- 默认语调偏口语化 (来源: 长期观察)
- 全局禁用词: "值得注意的是"、"在当今...的时代"

### 🏷️ [博客文章] 专属偏好
- 语调用"我"的第一人称视角 (来源: 2026-05-10 用户指正)
- 至少2-3个代码/案例示例 (来源: 2026-05-11 用户强调)
- 禁止结尾升华式总结 (来源: 2026-05-09 用户反馈)
- 偏好痛点提问式开头 (来源: 2026-05-10 用户选了版本A)
- 字数: 1500-2500字
```

如果当前写作类型在 MEMORY.md 中无记录，只注入全局偏好，并在注入小节标注：
```
> 📝 [社交媒体短文] 尚无历史记录，仅应用全局偏好。写作完成后将自动学习。
```

### 写作计划.md

详细大纲：

- 标题候选
- 逐节分解 + 预估字数
- 每节关键要点
- 节间过渡建议

---

## Phase 3: 初稿生成 (Creator → Executor → Creator)

### Spawning the Executor

Use `sessions_spawn`. See [references/executor-prompt.md](references/executor-prompt.md) for the full system prompt template.

The task must include:
1. 写作规则.md 完整内容（含历史偏好注入）
2. 写作计划.md 完整内容
3. 适用模板骨架（从模板库提取）
4. 目标受众描述
5. 语言要求

Example:
```
sessions_spawn:
  task: "[Full executor prompt with 写作规则 + 写作计划 + template + audience]"
  model: "deepseek/deepseek-v4-flash"
  context: "isolated"
```

### Creator's Supervision

Executor 返回后，Creator 逐一审查：
1. 是否遵守了写作规则？（逐条对照）
2. 是否遵循了写作计划？
3. 是否偏离了用户原始意图？
4. 两版是否在至少 2 个维度上产生了本质差异？
5. 🆕 **是否触犯了 MEMORY.md 中记录的历史禁忌？**

重大偏离 → 发回 Executor 修正。小问题留待后期打磨。

---

## Phase 4: 读者评审 (Creator → Reader → Creator)

### Spawning the Reader

Use `sessions_spawn`. See [references/reader-prompt.md](references/reader-prompt.md) for the full system prompt template.

The task must include:
1. 所有草稿版本全文
2. 需求分析.md（原始意图）
3. 写作规则.md
4. 目标受众详细画像
5. 平台/场景描述
6. 🆕 **MEMORY.md 中的用户历史偏好**（让 Reader 知道用户讨厌什么）

Example:
```
sessions_spawn:
  task: "[Full reader prompt with drafts + intent + rules + audience + history]"
  model: "deepseek/deepseek-v4-pro"
  context: "isolated"
```

### Reader Output

保存为 `读者点评.md`，包含：
- 身份代入声明
- 逐版加权评分表（6 维度 + AI 痕迹扣分）
- 亮点与硬伤剖析
- 最终选择与理由
- 改进建议 3-5 条

---

## Phase 5: 用户审阅与即时纠错 (Creator → User → Creator)

呈现给用户：
1. Reader 选出的最佳版本全文
2. Reader 的评分摘要和选择理由
3. 追问："是否满足需求？需要修改吗？"

### 🆕 即时纠错机制

**当用户给出任何负面反馈时，Creator 必须立即：**

1. **即时应用**：道歉 + 按要求修改
2. **即时记忆**：将纠错写入 `用户反馈.md`，并在内存中标记（待进化引擎持久化）
3. **即时生效**：如果用户要求重写，重写时必须应用新规则

用户反馈示例与处理：
| 用户说 | 立即行动 | 待记录 |
|---|---|---|
| "太啰嗦了" | 精简当前版本 | 篇幅偏好：偏短，默认字数下调20% |
| "语气太正式" | 重写为口语化 | 语调偏好：口语化 |
| "不像我平时说的" | 追问："你平时怎么说？" | 白名单/黑名单词汇 |
| "代码块太少" | 补充代码示例 | 技术类文章代码≥3个 |
| "不要结尾升华" | 砍掉结尾升华部分 | 禁止模式：结尾升华 |

### 迭代路线

- 微调词句 → 直接修改
- 结构调整 → 回 Phase 2/3
- 风格/语调问题 → **立即更新纠错记忆**，重新生成

---

## 🧠 Phase 5.5: 进化引擎 v2 (Creator — 自动执行)

**v2.1 引入，v2.5 升级为 v2。** 每次写作会话结束后（用户确认满意 / 表示结束 / 不再修改），Creator 自动触发进化分析，无需用户操作。

**v2.5 新增**: 全局统计分析（偏好漂移 + 否决权），在单次进化分析结束时顺带执行增量统计。

### 进化分析流程

Creator 调用 `sessions_spawn` 启动一个专门的 **Evolution Analyst** sub-agent：

```
sessions_spawn:
  task: |
    [Evolution Analyst Prompt — 见 references/evolution-analyst-prompt.md]

    请分析以下本次写作会话的完整记录：

    ## 用户原始需求
    [需求分析.md 内容]

    ## 写作规则
    [写作规则.md 内容]

    ## 读者评审
    [读者点评.md 内容]

    ## 用户反馈
    [用户反馈.md 内容]

    ## 历史风格档案（参考用）
    [MEMORY.md 内容]

    请按协议输出进化更新。
    最后执行 v2.5 新增的全局统计分析。

  model: "deepseek/deepseek-v4-pro"
  context: "isolated"
```

### 进化输出物

Evolution Analyst 返回结构化的更新建议，Creator 将其合并写入 `{workspace}/写作/MEMORY.md` 和 `{workspace}/写作/知识库.md`：

#### MEMORY.md 更新项

```markdown
## [YYYY-MM-DD HH:mm] [主题] 进化更新

### 新学到的偏好
- [从本次用户反馈+行为中提取的新偏好]

### 需要纠错的事项
- [本次用户指出的问题及对应的规避规则]

### 风格确认
- [本次用户满意的地方，强化记忆]

### 模板/配方效果
- 应用模板：[模板名] | 配方：[比例，如有] | 本次评分：[X分] | 用户满意度：[高/中/低]

### 词汇进化
- 🟢 白名单新增：[用户喜欢的新表达]
- 🔴 黑名单新增：[用户反感的新表达/模式]
```

#### 知识库.md 更新项

如果有值得沉淀的新知识（新技法、新结构、行业洞察），同步追加到 `知识库.md`。

### 🆕 全局统计分析 (v2.5)

在单次进化分析完成后，Evolution Analyst 对 MEMORY.md 做**增量统计**（非全量扫描——每次只更新统计面板的指标）：

#### 偏好漂移报告

按写作类型标签，自动检测同一个偏好维度在时间轴上的变化：

```markdown
### 📊 偏好漂移检测 ([写作类型标签])

| 偏好维度 | 最早记录 | 最新记录 | 趋势 | 置信度 |
|----------|----------|----------|------|--------|
| 语调 | 正式 (05-01) | 口语化 (05-13) | 逐步放松 📉 | 高 (3次确认) |
| 字数 | 3000+ (05-03) | 1500-2000 (05-14) | 偏好精简 📉 | 高 (4次缩短) |
```

如果漂移趋势明显（同方向 ≥3 次），标记为「高置信漂移」。Creator 在 Phase 1 时可附注：

> 📈 "检测到你最近对 [写作类型] 的文风偏好在往 [方向] 走，这次延续吗？"

#### 否决权机制 (Veto Rule)

当同一个维度连续 2 次被 Reader 打出低分（单维度 < 70）：

1. 该维度的当前偏好设置被**自动标记为待审查**
2. 下次 Phase 2 制定规则时，Creator 主动询问：

> ⚠️ "你偏好的 [某规则] 最近两次效果一般（Reader 评分偏低），这次要不要换一种？"

3. 如果第三次用户仍坚持原偏好且 Reader 评分回升 → 标记解除
4. 如果连续 3 次低分 → **自动进入黑名单**，该偏好不再自动应用，必须用户主动指定才启用

#### 采纳率统计

```markdown
### 📊 采纳率

| 写作类型 | 总次数 | 用户接受率 | 平均 Reader 分 | 最常见配方 |
|----------|--------|------------|----------------|------------|
| [博客文章] | 8 | 87.5% | 82 | 博客:100% |
| [朋友圈文案] | 5 | 100% | 85 | 短文:80%+商业文案:20% |
```

#### 词汇热力图

```markdown
### 📊 词汇热力图 (全局)

🟢 高频白名单 (全局): "其实"(6次), "说白了"(4次), "你感受一下"(3次)
🔴 高频黑名单 (全局): "值得注意的是"(5次触发), "在当今"(3次触发)
⚠️ 待定 (跨类型出现但未被标记): "所以我想说的就是" (出现3次，未明确反馈)
```

#### 增量更新规则

- 统计数据不是每次扫描全量历史，而是**增量更新**——基于上次统计数字 + 本次新数据
- MEMORY.md 顶部维护一个 `## 📊 全局统计摘要` 节，每次进化分析结束时更新
- 如果 MEMORY.md 无此节，首次初始化：扫描全量历史建立基线

### 进化引擎的调度时机

以下情况触发进化分析：
- ✅ 用户明确表示满意（"可以了"、"不错"、"就这样"）
- ✅ 用户沉默超过 2 轮（判断为接受）
- ✅ 用户切换话题（判断本次写作结束）
- ❌ 用户仍在积极修改中（不触发）
- ❌ 用户说"只是试一下"（不触发，除非产生了有价值的纠错）

### 进化分析的守护规则

1. **勿过度拟合**：单次用户反馈不一定代表长期偏好，Evolution Analyst 需判断是"偶发需求"还是"长期偏好"。偶发需求 → 写入上下文标注"仅限此类主题"。长期偏好 → 写入全局偏好。
2. **冲突解决**：如果新偏好与旧偏好冲突（上次喜欢短句，这次要长句），标注为"情境依赖"而非覆盖。
3. **隐私优先**：不记录用户身份信息、不记录敏感内容原文（只提取结构化的风格特征）。

---

## AI 痕迹避坑指南 (全局适用)

See [references/ai-traces-guide.md](references/ai-traces-guide.md) for the comprehensive guide covering:
1. 词汇与短语警戒线（中英文高危词表）
2. 结构性 AI 模式（对仗强迫症、结尾升华综合征等）
3. 内容空洞信号（端水大师、时间遁词等）
4. 🆕 **中英双语混淆检测**（英文思维写中文 + 中文思维硬译英文 + 回译测试法）

**快速自查三问** (所有人每次输出前自问)：
1. 这句话真人聊天时说得出口吗？
2. 这段文字增加了信息还是仅仅在重复？
3. 读者会跳过这一段吗？

🆕 **双语自查** (中英混排时追加)：
4. 回译测试：这句中文译回英文后通顺自然吗？（如果通顺 → AI 味浓）

---

## Model Configuration

| Role | Default Model | Rationale |
|---|---|---|
| Creator (main) | deepseek/deepseek-v4-pro | 深度推理用于需求挖掘与质量把控 |
| Executor (sub-agent) | deepseek/deepseek-v4-flash | 快速产出多版初稿，性价比高 |
| Reader (sub-agent) | deepseek/deepseek-v4-pro | 批判性评审需要深度思考 |
| Evolution Analyst (sub-agent) | deepseek/deepseek-v4-pro | 进化分析需要精确判断偏好 vs 偶发需求 |

See [references/model-config.md](references/model-config.md) for alternative configurations (all-Pro, all-Flash, Ollama local).

---

## File References

- **[creator-prompt.md](references/creator-prompt.md)** — 创作者完整协议（角色设定、递进逻辑、模板匹配、输出格式、v2.2 增加了历史偏好感知）
- **[executor-prompt.md](references/executor-prompt.md)** — 执行者完整系统提示词模板（角色约束、差异化策略、禁止清单、输出格式）
- **[reader-prompt.md](references/reader-prompt.md)** — 读者完整系统提示词模板（身份代入、加权六维评分、高压红线扣分、结构化输出、v2.2 增加了历史禁忌感知）
- **[evolution-analyst-prompt.md](references/evolution-analyst-prompt.md)** — 🆕 进化分析师协议（偏好判断、纠错提炼、情境标注、冲突解决、v2.5 新增配方追踪 + 全局统计）
- **[template-library.md](references/template-library.md)** — 多场景写作模板库（15 种模板 + 🆕 v2.5 跨模板融合指南）
- **[ai-traces-guide.md](references/ai-traces-guide.md)** — AI 痕迹高频特征避坑指南（词汇/结构/内容/🆕中英双语混淆四分类，含回译测试法）
- **[examples.md](references/examples.md)** — 🆕 端到端写作示例（博客文章/朋友圈文案/求职简历三种场景，展示完整 Phase 0-5.5 流程）
- **[model-config.md](references/model-config.md)** — 模型配置方案与切换指南

## File Management

- 所有文件产出到 `{workspace}/写作/`（自动检测）
- 每次写作会话创建子文件夹：`YYYY-MM-DD_HHmm-{简写主题}/`
- 历史会话不删除，作为写作资产积累
- MEMORY.md 根级别跨会话持久化（进化引擎持续更新）
- 知识库.md 跨会话积累（永不删除，只追加）
