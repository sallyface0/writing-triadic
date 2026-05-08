# Writing Triadic — 三角色协作写作 Skill


> 写作不是一次 AI 生成任务。它需要**深度理解意图**、**精准执行**、**真实读者反馈**。

## ✨ 核心理念

传统的 AI 写作就是"你说一句，它写一段"。Writing Triadic 把写作拆成三个角色：

| 角色 | 中文名 | 职责 |
|---|---|---|
| 🧠 **Creator** | 创作者 / 内容架构师 | 深层挖掘用户意图（4问/轮），自动匹配最佳写作模板 |
| ✍️ **Executor** | 执行者 / 精密写手 | 按规则产出 ≥2 版有本质差异的初稿 |
| 👁️ **Reader** | 读者 / 灵魂受众 | 代入目标读者身份，加权六维评分，选出最佳版本 |

## 🚀 快速开始

### 安装

```bash
openclaw skills install writing-triadic
```

### 触发方式

直接说你想写什么就行：

- "帮我写一篇关于 AI 的博客"
- "写个产品文案"
- "帮我写个工作总结"
- "写个小说开头，科幻的"

### 工作流程

```
用户提需求 → 创作者挖掘意图（最多4问/轮）
    ↓
置信度≥95% → 自动联网调研 + 更新知识库
    ↓
匹配模板 → 制定规则和计划
    ↓
执行者产出 ≥2版初稿
    ↓
读者以人类视角评审 → 选出最佳
    ↓
呈现给用户 → 迭代修改
```

## 📚 支持的写作类型

| 场景 | 模板 |
|---|---|
| 技术文档 | TL;DR → 先决条件 → 操作步骤 → 避坑指南 |
| 博客文章 | 钩子 → 重要性 → 正文 → 可落地结论 → CTA |
| 随笔/散文 | 微观切入 → 延展联想 → 内在冲突 → 留白结尾 |
| 学术/行业论文 | 摘要 → 引言 → 方法论 → 发现 → 局限性 |
| 商业文案 | 痛点 → 放大痛苦 → 方案 → 信任背书 → 行动 |
| 小说/故事 | 破局点 → 升级 → 高潮 → 余波 |

## 🛡️ AI 痕迹检测

内置完善的 AI 痕迹避坑指南，涵盖：
- **词汇警戒线**：40+ 中英文高危 AI 词汇
- **结构性模式**：对仗强迫症、结尾升华综合征、破折号滥用
- **内容空洞信号**：端水大师、时间遁词、模糊归因

## 🔧 模型配置

| 角色 | 默认模型 | 说明 |
|---|---|---|
| Creator | `deepseek/deepseek-v4-pro` | 深度推理 |
| Executor | `deepseek/deepseek-v4-flash` | 快速生成 |
| Reader | `deepseek/deepseek-v4-pro` | 批判评审 |

支持自定义：全 Pro 模式、全 Flash 模式、Ollama 本地隐私模式。

## 📁 文件结构

```
writing-triadic/
├── SKILL.md                    # 主文件
└── references/
    ├── creator-prompt.md       # 创作者协议
    ├── executor-prompt.md      # 执行者 system prompt
    ├── reader-prompt.md        # 读者 system prompt
    ├── template-library.md     # 6 种写作模板
    ├── ai-traces-guide.md      # AI 痕迹避坑指南
    └── model-config.md         # 模型配置方案
```

## 🌍 国际化

默认使用 DeepSeek 模型，其他用户可替换为：
- OpenAI: `openai/gpt-5` + `openai/gpt-5-mini`
- Anthropic: `anthropic/claude-sonnet-4-20250514` + `anthropic/claude-haiku-4-5-20251001`

