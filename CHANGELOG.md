# CHANGELOG

## v2.4.0 (2026-05-13)

### 🏗️ 结构优化

- **SKILL.md 瘦身** — 637 行 → 491 行（-23%）。移除冗余 MEMORY.md 结构示例（~120行），替换为简要说明+引用。将 mermaid 流程图改为文字摘要。不影响功能，提升模型解析效率。
- **frontmatter 补齐** — 新增 `version: 2.4.0` 和 `author: sallyface0` 字段，方便平台自动索引和版本通知。

### 🧠 Executor 差异化决策树

- **executor-prompt.md 新增差异决策表** — 根据用户核心意图（说服/告知/表达/娱乐/汇报/推销）自动匹配最优的两个差异维度组合。减少 Creator 手动判断负担，避免长周期使用中陷入相同维度组合的惯性。

### 📁 文件变更

- `SKILL.md` — 瘦身 + frontmatter 更新
- `references/executor-prompt.md` — 新增差异化决策树
- `CHANGELOG.md` — 本文

---

## v2.3.0 (2026-05-13)

### 🔍 P1 — 中英双语混淆检测

- **ai-traces-guide.md 大幅扩展** — 新增第 4 节「中英双语混淆检测」
  - 4.1 英文思维写中文检测：被动语态泛滥、It is + adj + that 直译、连接词首句、"对……进行……"万能句式等 8 种模式
  - 4.2 中文思维硬译英文检测：With the development of...、plays an important role、not only... but also 等 8 种 Chinglish 模式
  - 4.3 双语混淆评分表：Reader 评审时额外检查，每处 -3 分
  - 回译测试法：中文译回英文，如果英文通顺自然 → 中文八成是 AI 味
- 更新快速自查清单：新增 5 项双语检查
- 新增英文区对比示例（AI Chinglish vs. Native English）

### 📖 P1 — 端到端写作示例 (examples.md)

- 新增 `references/examples.md` — 三个完整实战示例：
  1. **博客文章** — "如何用好 AI 编程助手"，展示完整 Phase 0-5.5，含真实 Creator 提问、Executor 双版本产出、Reader 评分表、Evolution Analyst 输出
  2. **朋友圈文案** — 项目完成后的社交分享，展示首次使用的写作类型自动初始化流程
  3. **求职简历优化** — 从"负责XXX"流水账改造为 STAR 句式，含改写前后对比
- 附设计原则总结

### 🌍 P1 — 英文版 README (README_EN.md)

- 新增 `README_EN.md` — 完整英文版介绍
- 包含：核心理念、15 种模板一览、双语混淆检测亮点、快速开始、模型配置
- 与国际用户（OpenAI/Anthropic 模型用户）的切换指南

### 🔧 配套更新

- SKILL.md：版本号 v2.2 → v2.3、标题与导语更新、AI 痕迹章节引用更新（增加双语混淆+回译测试）、File References 补齐 examples.md
- README.md：版本号同步 v2.3、新增 v2.3 changelog 章节、文件结构更新、AI 检测亮点更新

### 📁 文件变更

- `SKILL.md` — 版本号与引用更新
- `README.md` — v2.3 changelog + 文件结构 + AI 亮点更新
- `CHANGELOG.md` — 本文
- `README_EN.md` — 🆕 新增
- `references/ai-traces-guide.md` — 重写（新增第 4 节 + 双语检测体系）
- `references/examples.md` — 🆕 新增

---

## v2.2.0 (2026-05-11)

### 📝 模板库重大升级：11 种 → 15 种

新增 4 种写作模板：
- **毕业论文/学位论文** (#12) — 完整学术论文骨架：绪论→文献综述→方法→结果→讨论→结论→附录。含引用规范和质量标准。
- **求职简历** (#13) — ATS 优化的 STAR 句式简历：一页原则、量化成果、严禁假大空形容词。
- **朋友圈文案** (#14) — 人格化社交文案：钩子→叙事→情绪落点。像朋友聊天而非微商广告。
- **产品评测** (#15) — 购买决策导向评测：一句话结论置顶、优缺点对等、场景化分组的深度体验。

### 🔧 既有模板修正

- 学术论文 (#4)：新增文献综述章节 + 参考文献格式要求
- 小说/故事 (#6)：优化对话标签描述（从"全用说道"改为"避免重复单一标签"）
- 邮件 (#9)："200字内"明确为"日常事务邮件"，复杂事项允许更长但建议拆分
- 产品说明书 (#11)：删除"不含图"不合理禁止项

### 🔧 版本号统一 (P0 修复)

- SKILL.md 版本号统一为 v2.2（标题行、State Machine、MEMORY.md 结构、Phase 5.5、File References）
- creator-prompt.md / reader-prompt.md / evolution-analyst-prompt.md / model-config.md 版本号修正
- 模板匹配速查表补齐 4 个新增模板（毕业论文、求职简历、朋友圈文案、产品评测）
- README.md 全面更新：模板 11→15、v2.2 changelog、文件结构
- 正式 skill 目录补充 README.md + CHANGELOG.md + LICENSE（原仅 GitHub repo 有）

---

## v2.1.2 (2026-05-11)

### 🔧 元数据修正

- 修复 ClawHub License 显示问题 — 在 SKILL.md frontmatter 中显式声明 `license: MIT`
- ClawHub 显示 License 从 MIT-0 更新为 MIT

---

## v2.1.1 (2026-05-11)

### 🔧 文档与元数据修正

- 修正 SKILL.md 中多处版本号 `v2.0` → `v2.1`（内容已是 v2.1，版本号未同步）
- 优化 ClawHub package description，压缩摘要长度使其在搜索结果中完整展示
- README.md 补充 License 声明（MIT-0）

### 📄 License

- 新增 MIT-0 LICENSE 文件，与 ClawHub 保持一致

---

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
