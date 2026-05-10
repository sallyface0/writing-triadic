# CHANGELOG

## v2.1.0 (2026-05-10)

### 🧬 核心新增：自我进化引擎

- **Phase 0 读取风格档案** — 每次写作前，Creator 自动读取 MEMORY.md，将用户的历史偏好注入决策
- **Phase 5.5 进化分析师** — 新增 Evolution Analyst sub-agent，每次写作后自动分析会话、提炼知识
- **即时纠错机制** — 用户指正（如"太啰嗦了"）立即应用并自动记忆，下次自动规避

### 🏷️ 写作类型 × 维度矩阵记忆

- MEMORY.md 从被动日志升级为结构化风格档案
- 偏好按写作类型标签双层索引（如 `[商业文案]`、"宣传语"别名保留）
- 三层偏好注入：全局 → 类型精确匹配 → 相邻类型参考
- 不同写作类型的偏好相互隔离，不会串味
- 支持跨类型关联发现（如宣传语语调偏好可继承至社交媒体短文）

### 📝 模板库扩展

- 从 6 种扩展至 11 种写作模板
- 新增：社交媒体短文、视频脚本/口播稿、邮件/商务信函、演讲稿、产品说明书

### 👁️ Reader 升级

- 新增历史禁忌检查维度（触犯额外扣 10 分/项）
- 评分表新增 "🆕 历史禁忌扣分" 行

### 🔧 模型配置更新

- 新增 Evolution Analyst 角色模型配置
- 支持全 Pro / 全 Flash / Ollama 本地隐私模式（含 Evolution Analyst）

### 📁 文件变更

- `SKILL.md` — 完全重写，新增 Phase 0 / Phase 5.5，更新偏好注入逻辑
- `references/creator-prompt.md` — 新增历史偏好感知、三层注入、即时纠错逻辑
- `references/reader-prompt.md` — 新增历史禁忌检查
- `references/evolution-analyst-prompt.md` — 🆕 新增，进化分析师完整协议
- `references/template-library.md` — 扩展至 11 种模板
- `references/model-config.md` — 新增 Evolution Analyst 配置

---

## v1.0.0 (2026-05-08)

### 🎉 首次发布

- 三角色协作写作框架：Creator + Executor + Reader
- 5 Phase 完整工作流
- 6 种写作模板
- AI 痕迹避坑指南（词汇/结构/内容三分类）
- 联网调研 + 知识库自动更新
- 模型配置（默认/全Pro/全Flash/Ollama隐私模式）
