---
name: writing-triadic
license: MIT
description: >
  Self-evolving 3-role writing framework: Creator mines intent via progressive Q&A, Executor produces 2 distinct drafts, Reader scores with weighted 6-D review. Evolution Engine distills every session into a growing style memory — user corrections compound, banned phrases auto-avoid next time. 15 templates. The more you use it, the smarter it gets.
---

# Writing Triadic v2.3 — 自我进化的三角色协作写作框架

> **v2.3 升级:** 中英双语混淆检测、端到端写作示例 (examples.md)、英文版 README (README_EN.md)。v2.2 的 15 种模板全部保留。

## Overview

| Role | 中文名 | Actor | Responsibility |
|---|---|---|---|
| **Creator** | 创作者 / 内容架构师 | Main AI (you) | 挖掘需求、匹配模板、调度监督、最终交付、**驱动进化引擎** |
| **Executor** | 执行者 / 精密写手 | Sub-agent A | 按需求+模板产出 ≥2 版有本质差异的初稿 |
| **Reader** | 读者 / 灵魂受众 | Sub-agent B | 代入目标读者身份，加权评分，选出最佳版本 |

**核心洞察**：写作不是一次 AI 生成任务。它需要深度理解用户意图（创作者）、精准执行（执行者）、真实读者反馈（读者）。v2.2 让这三个角色产出的所有经验，都沉淀为一个**持续生长的风格大脑**，15 种模板覆盖从学术论文到朋友圈的全场景写作。

## Trigger Conditions

- User says "write", "写作", "帮我写", "article", "blog", "essay", "draft", "文案", "小说", etc.
- User provides a topic or vague writing idea
- User wants to refine or rewrite existing content
- User explicitly invokes this skill

## System Architecture

```mermaid
flowchart TB
    U(用户) --> C(创作者): 提出初步需求
    C --> U: 询问需求 (最多4问/轮)
    U --> C: 回答
    C --> M[风格档案 MEMORY.md]: 读取用户偏好
    M --> C: 注入历史偏好
    C --> C: 置信度≥95%? 匹配模板
    C --> W(联网调研): 搜索最新模板/技法/案例
    W --> K(知识库): 有增量则更新
    W --> C: 调研结果注入规则
    C --> E(执行者): 下发需求+模板+规则+偏好
    E --> C: 返回版本A & 版本B
    C --> R(读者): 提交所有版本
    R --> C: 加权评分 + 选出最佳
    C --> U: 呈现最佳版本+评审意见
    U --> C: 接受 / 修改 / 指正
    C --> EV[进化引擎]: 触发进化分析
    EV --> M: 更新风格档案
    EV --> K: 更新知识库
```

**State Machine (v2.2):**

```mermaid
stateDiagram-v2
    [*] --> 需求挖掘
    需求挖掘 --> 需求挖掘: 继续提问(≤4轮)
    需求挖掘 --> 模板匹配: 置信度≥95%
    模板匹配 --> 联网调研
    联网调研 --> 知识库同步: 筛选增量写入
    联网调研 --> 规则计划: 调研结果注入+偏好注入
    规则计划 --> 初稿生成
    初稿生成 --> 读者评审
    读者评审 --> 交付完成: 用户接受
    读者评审 --> 初稿生成: 用户要求重写/指正
    读者评审 --> 需求挖掘: 需求有误需重新澄清
    交付完成 --> 进化分析: 自动触发
    进化分析 --> [*]: 知识沉淀完毕
```

---

## 🧠 核心概念：风格进化档案

当前 skill 维护一个结构化的 `MEMORY.md`，远不止"记录日志"——它是一份**活的风格档案**，每次写作都在更新。Creator 在 Phase 1 提问前**必须先读取** MEMORY.md，将用户的历史偏好注入决策。

### MEMORY.md 结构 (v2.2 — 写作类型 × 维度矩阵)

MEMORY.md 采用**双层索引**结构：第一层按写作类型分类，第二层按偏好维度分类。
这样用户对"宣传语"的指正不会错误应用到"技术博客"上。

```markdown
# 写作风格进化档案

## 👤 用户画像 (跨类型通用)
- 常用词汇/口头禅：[...]
- 人格特质：[...]
- 频繁出现的主题领域：[...]

---

## 📊 全局偏好 (Global Preferences — 所有类型默认应用)

### 语调
- 默认偏好：[轻松幽默 / 专业严谨 / ...]

### 篇幅
- 默认字数：[...]

### 全局黑名单 (跨类型禁用)
- [表达/模式] (来源: [日期] 用户反馈)
- ...

### 全局白名单 (跨类型偏好)
- [表达/模式]
- ...

---

## 🏷️ 按写作类型的偏好矩阵

> 格式: `[写作类型标签]` → 维度 → 具体规则 (来源: 日期, 置信度)
> 写作类型标签规范: 使用模板名称作为主标签（如 `[博客文章]`、`[商业文案]`），
> 如果用户使用了自定义称呼（如"宣传语"），保留用户称呼作为别名。

### [博客文章]
- **语调**: 口语化，用"我"视角 (来源: 2026-05-10 用户指正, 高)
- **结构**: 必须有2-3个代码示例 (来源: 2026-05-11 用户强调, 高)
- **禁用**: 结尾不要升华式总结 (来源: 2026-05-09 用户反馈, 高)
- **偏好开头**: 痛点提问或反常识数据 (来源: 2026-05-10 用户选了版本A, 中)
- **字数**: 1500-2500字 (来源: 2026-05-10 用户反馈, 中)

### [商业文案]
- **语调**: "不要太硬，软一点，像朋友推荐而不是推销" (来源: 2026-05-12 用户指正, 高)
- **禁用**: 假大空形容词堆砌 (来源: 2026-05-12 用户强调, 高)
- **结构**: PAS框架但Solution部分不要吹过头 (来源: 2026-05-12 用户反馈, 高)

### [随笔/散文]
- **语调**: "可以文艺一点，但不能酸" (来源: 2026-05-13 用户反馈, 高)
- **篇幅**: 800-1200字，短段为主 (来源: 2026-05-13 用户反馈, 中)

### [社交媒体短文]
- **语调**: 口语化、有梗、emoji适度 (来源: 2026-05-14 用户反馈, 高)
- **禁用**: 书面语、"值得注意的是"等套话 (来源: 2026-05-14 用户强调, 高)

### [技术文档]
- ...

### [...]
- (新增类型时按需追加)

---

## 📝 纠错日志 (Correction Log — 按时间倒序)

> 格式: `[日期] [写作类型] 主题 → 用户原话 → 已应用规则 → 偏好层级`

- [2026-05-12] [商业文案] 产品宣传语 → "不要硬推，太像广告了" → 商业文案语调偏软，禁用假大空形容词 → 情境-[商业文案]
- [2026-05-11] [博客文章] 技术分享 → "代码块太少，例子不够" → 技术类文章代码示例≥3个 → 情境-[技术文档/博客文章]
- [2026-05-10] [博客文章] 写作心得 → "太正式，不像我平时说话" → 语调偏口语化，开篇用"我"视角 → 情境-[博客文章]

---

## 📈 模板使用统计 (Template Stats)
| 模板 | 使用次数 | 平均评分 | 用户满意度 | 上次使用 |
|---|---|---|---|---|
| 博客文章 | 5 | 82分 | 高 | 2026-05-10 |
| 商业文案 | 2 | 78分 | 中 | 2026-05-08 |

---

## 📖 词汇进化 (Vocabulary Evolution)

### 全局
- 🟢 白名单: [跨类型偏好表达]
- 🔴 黑名单: [跨类型禁用表达]

### 按类型
- 🔴 `[商业文案]` 禁用: "颠覆"、"革命性"、"前所未有"
- 🟢 `[博客文章]` 偏好: "说实话"、"你懂的"、"一个有意思的点"
- 🔴 `[社交媒体短文]` 禁用: "值得注意的是"、"在当今"
```

### 读取时机

Creator 必须在以下时刻读取 MEMORY.md：
1. **Phase 1 开始前** — 了解用户偏好，让提问更精准
2. **Phase 2 规则制定前** — 将历史偏好+纠错记忆注入写作规则
3. **Phase 5 交付时** — 对比历史评分，提醒用户"这次风格和方法跟上次不太一样，注意到吗？"

### 写入时机（进化引擎）

Phase 5 完成后，Creator 自动执行进化分析（见 Phase 5.5）。

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

### 置信度门槛

达到 **95% 置信度**，且已锁定【核心内容 + 所用模板】，输出：

```
💡 需求与框架锁定报告
- 核心主题：...
- 目标受众：...
- 预期目的：...
- 写作语调：...
- 应用模板/体裁：[模板名称]
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

## 🧠 Phase 5.5: 进化引擎 (Creator — 自动执行)

**v2.1 引入，v2.2 持续优化。** 每次写作会话结束后（用户确认满意 / 表示结束 / 不再修改），Creator 自动触发进化分析，无需用户操作。

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

### 模板效果
- 应用模板：[模板名] | 本次评分：[X分] | 用户满意度：[高/中/低]

### 词汇进化
- 🟢 白名单新增：[用户喜欢的新表达]
- 🔴 黑名单新增：[用户反感的新表达/模式]
```

#### 知识库.md 更新项

如果有值得沉淀的新知识（新技法、新结构、行业洞察），同步追加到 `知识库.md`。

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
- **[evolution-analyst-prompt.md](references/evolution-analyst-prompt.md)** — 🆕 进化分析师协议（偏好判断、纠错提炼、情境标注、冲突解决）
- **[template-library.md](references/template-library.md)** — 多场景写作模板库（15 种模板，覆盖学术/商业/社交/评测全场景）
- **[ai-traces-guide.md](references/ai-traces-guide.md)** — AI 痕迹高频特征避坑指南（词汇/结构/内容/🆕中英双语混淆四分类，含回译测试法）
- **[examples.md](references/examples.md)** — 🆕 端到端写作示例（博客文章/朋友圈文案/求职简历三种场景，展示完整 Phase 0-5.5 流程）
- **[model-config.md](references/model-config.md)** — 模型配置方案与切换指南

## File Management

- 所有文件产出到 `{workspace}/写作/`（自动检测）
- 每次写作会话创建子文件夹：`YYYY-MM-DD_HHmm-{简写主题}/`
- 历史会话不删除，作为写作资产积累
- MEMORY.md 根级别跨会话持久化（进化引擎持续更新）
- 知识库.md 跨会话积累（永不删除，只追加）
