# Writing Triadic — Self-Evolving 3-Role Collaborative Writing Framework

> Writing is not a one-shot AI generation task. It requires **deep intent understanding**, **precise execution**, and **authentic reader feedback**.  
> v2.3 extends to 15 writing templates, covering everything from thesis to social posts. The more you use it, the smarter it gets.

## ✨ Core Philosophy

Traditional AI writing is "you say something, it writes something." Writing Triadic decomposes writing into three roles plus an evolution engine:

| Role | Name | Responsibility |
|---|---|---|
| 🧠 **Creator** | Content Architect | Deep intent mining (≤4 questions/round), auto template matching, drives evolution engine |
| ✍️ **Executor** | Precision Writer | Produces ≥2 drafts with meaningful divergence based on rules + templates |
| 👁️ **Reader** | Soul Audience | Inhabits the target reader's persona; weighted 6-dimension scoring; picks the best version |
| 🧬 **Evolution Analyst** | Evolution Analyst | Auto-extracts preferences after each session, tagged by writing type |

## 🆕 v2.3 Highlights

### 15 Writing Templates (v2.2 + refined)

From academic thesis to WeChat Moments — one framework, all scenarios:

| Category | Template |
|---|---|
| Technical Docs | TL;DR → Prerequisites → Step-by-Step → Gotchas |
| Blog Posts | Hook → Why It Matters → Body → Actionable Takeaway → CTA |
| Personal Essays | Micro Detail → Associative Drift → Inner Conflict → Open Ending |
| Academic Papers | Abstract → Introduction → Methodology → Findings → Limitations |
| Business Copy | Problem → Agitation → Solution → Proof → CTA |
| Narrative Fiction | Inciting Incident → Rising Action → Climax → Resolution |
| Social Shorts | Title Hook → Persona Anchor → Point-by-Point → Tags + Engagement |
| Video Scripts | 0-3s Hook → Value Promise → Progressive Points → CTA |
| Email / Business Letters | Subject Line → Core Message → Details → Closing |
| Speeches | Opening Bomb → Body Expansion → Emotional Peak → Memorable Close |
| Product Manuals | Safety Warning → Quick Start → Detailed Ops → Troubleshooting |
| Thesis / Dissertation | Introduction → Lit Review → Methodology → Results & Analysis → Discussion → Conclusion |
| Resume / CV | Header → Objective → Education → Experience → Projects → Skills |
| Social Post (Moments) | Hook → Narrative → Emotional Landing |
| Product Review | One-Liner Verdict → Quick Specs → Deep Experience → Comparison → Buying Advice |

### Writing Type × Dimension Memory Matrix

```
[Business Copy] tagline → "don't hard-sell" → softer tone, ban puffery adjectives
[Blog Post] tech share → "too few code examples" → ≥3 code samples
[Thesis] proposal → "lit review too thin" → every subsection must critique prior work
[Social Post] daily share → "sounds like a work report" → drop bullet points, use conversational narrative
...
```

**Preferences from different writing types stay isolated.** What you learn about writing ad copy won't leak into your technical blog.

### Three-Layer Preference Injection

```
Global Preferences (cross-type, always injected)
  ↓
Type-Specific Exact Match ([Blog Post] preferences)
  ↓
Adjacent Type Reference ([Tech Doc] falls back to [Blog Post] if no records)
```

### Instant Correction + Auto Memory

You say "too verbose" → it fixes immediately + persists to MEMORY.md. Next time auto-avoided. **One session, one lesson learned.**

### 🆕 v2.3: Bilingual Cross-Contamination Detection

New Section 4 in the AI Traces Guide. Detects:
- **English brain writing Chinese** (passive voice overload, "it is worth noting that" calques, connector-stuffed sentence starts)
- **Chinese brain writing English** ("With the development of...", "plays an important role in", "not only... but also" overuse)
- Back-translation test: if translating your Chinese back to English reads naturally, the Chinese is probably AI-flavored

### 🆕 v2.3: End-to-End Examples (`references/examples.md`)

Three complete walkthroughs:
1. **Blog Post** — "How to Use AI Coding Assistants Effectively" (full Phase 0-5.5 with evolution output)
2. **Social Post** — Project milestone celebration (30-min WeChat Moments from scratch)
3. **Resume Optimization** — From "job description clone" to STAR-sentence power resume

Each example shows real Creator questions, Executor outputs, Reader scores, and Evolution Analyst summaries.

## 🚀 Quick Start

### Installation

```bash
openclaw skills install writing-triadic
```

### Usage

Just say what you want to write:

- "Write a blog post about prompt engineering"
- "Draft a product launch copy"
- "Write my performance review"
- "Start a sci-fi story"
- "Optimize my resume"
- "Write a WeChat Moments post"

### Workflow

```
User request
    ↓
Phase 0: Read style evolution archive (knows what you hate & love)
    ↓
Phase 1: Creator mines intent (≤4 questions/round, history-informed)
    ↓
Phase 1.5: Auto web research + knowledge base update
    ↓
Phase 2: Template matching (15 choose 1) → rules (with history injection)
    ↓
Phase 3: Executor produces ≥2 drafts
    ↓
Phase 4: Reader evaluates as human audience → picks best
    ↓
Phase 5: Present to you → your corrections instantly memorized
    ↓
Phase 5.5: Evolution Analyst auto-extracts → updates style archive 🧬
```

## 🛡️ AI Trace Detection

Built-in comprehensive AI trace avoidance guide covering:
- **Vocabulary Warning List**: 40+ high-risk AI words (Chinese + English)
- **Structural Patterns**: Parallel-holic, ending-elevation syndrome, em dash abuse
- **Content Hollow Signals**: Fence-sitting, time-dodging, vague attribution
- **Historical Taboo Check**: Reader prioritizes checking user history, -10 points per violation
- 🆕 **Bilingual Cross-Contamination**: English-brain-Chinese, Chinese-brain-English detection with back-translation test

## 🔧 Model Configuration

| Role | Default Model | Rationale |
|---|---|---|
| Creator | `deepseek/deepseek-v4-pro` | Deep reasoning for intent mining & QC |
| Executor | `deepseek/deepseek-v4-flash` | Fast generation, multi-draft cost efficiency |
| Reader | `deepseek/deepseek-v4-pro` | Critical evaluation needs reasoning |
| Evolution Analyst | `deepseek/deepseek-v4-pro` | Preference vs one-off judgment |

Customizable: All-Pro mode, All-Flash mode, Ollama local privacy mode.

For non-DeepSeek users:
- OpenAI: `openai/gpt-5` + `openai/gpt-5-mini`
- Anthropic: `anthropic/claude-sonnet-4-20250514` + `anthropic/claude-haiku-4-5-20251001`

## 📁 File Structure

```
writing-triadic/
├── SKILL.md                         # Main file (v2.3)
├── README.md                        # Chinese README
├── README_EN.md                     # This file
├── CHANGELOG.md                     # Update log
├── LICENSE                          # MIT License
└── references/
    ├── creator-prompt.md            # Creator protocol (with history awareness)
    ├── executor-prompt.md           # Executor system prompt
    ├── reader-prompt.md             # Reader system prompt (with taboo check)
    ├── evolution-analyst-prompt.md  # Evolution Analyst protocol
    ├── template-library.md          # 15 writing templates
    ├── ai-traces-guide.md           # AI trace avoidance guide (v2.3: +bilingual detection)
    ├── examples.md                  # 🆕 End-to-end writing examples
    └── model-config.md              # Model configuration reference
```

## 📄 License

[MIT](LICENSE) — Copyright (c) 2026 sallyface0

## 📝 Changelog

See [CHANGELOG.md](CHANGELOG.md)
