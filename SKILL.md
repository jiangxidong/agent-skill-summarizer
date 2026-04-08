---
name: summarizer
description: Analyze and summarize articles, podcast transcripts, video transcriptions, and long-form content into structured bilingual (Chinese-English) reports with Obsidian callouts. Use when the user shares a URL, file path, or pastes text and asks to summarize, analyze, digest, review, or extract insights from any content. Also triggers on phrases like "总结这篇文章", "帮我分析", "summarize this", "digest this transcript", or when user provides a transcript/article for analysis.
---

# Summarizer — 双语内容分析与结构化总结

将任意文章、播客转录、视频文字稿转化为结构化、美观的Obsidian笔记，包含核心概括、思维导图、主题深度解析和可操作建议。

## Workflow

### Phase 1: Input Detection & Content Acquisition

Determine input type and acquire content:

- **File path** — Read the file directly
- **URL** — Use WebFetch to fetch the page content
- **Pasted text** — Use the text directly from conversation
- **Browser tab / clipboard** — Ask user to paste or provide path

If the content is empty or unreadable, inform the user and suggest alternatives.

### Phase 2: Pre-Analysis & User Confirmation

After reading the content, perform a quick assessment:

1. **Estimate content length** — count approximate word/character count
2. **Identify content type** — article, podcast transcript, video transcript, interview, technical doc, etc.
3. **Propose theme count** — based on content complexity:
   - Short content (<2000 words): suggest 2-3 themes
   - Medium content (2000-5000 words): suggest 3-4 themes
   - Long content (>5000 words): suggest 4-6 themes

Then ask the user **1-3 quick confirmation questions** using the AskUserQuestion tool. The questions should include:

- **Theme count confirmation**: "内容约X字，识别出以下N个核心主题：[列出主题名称]。数量和方向是否合适？" with options like the suggested number, +1, -1, or "你来决定"
- **Only ask additional questions if genuinely uncertain** about something — e.g., if the content covers multiple distinct domains and you're unsure which angle the user cares about, or if the title is ambiguous

If the user has already specified preferences in their prompt (e.g., "生成5个主题", "重点关注技术部分"), skip the corresponding question and respect their instruction.

### Phase 3: Generate Report

**Before writing the report**, load the `obsidian:obsidian-markdown` skill using the Skill tool to ensure correct Obsidian formatting syntax (especially callouts and properties).

Generate the report following the Output Template below. Apply these principles:

- **中英混合风格** — English for topic names, dimension names, key concepts; Chinese for detailed descriptions and explanations
- **信息密度优先** — Every sentence should carry meaning. No filler, no redundant transitions
- **证据驱动** — Insights must be backed by specific quotes, data, or examples from the source
- **可操作性** — Every theme must include concrete, actionable recommendations
- **Obsidian Callouts** — Use callouts to highlight the most important content (see Formatting Guide below)

**PACER Analysis** — After completing the Theme Analysis, determine the single most relevant PACER category for the entire article based on its dominant knowledge type and most likely learning goal. Then generate the PACER Application section using the matching action template (see PACER Classification Guide below).

### Phase 4: Save Output

Save the report as a new file:

- **If input is a file**: save in the same directory with a descriptive name based on content (e.g., `GSD2-vs-Claude-Code.md`)
- **If input is URL/paste**: save in the current working directory
- **File naming**: Use the pattern `[Topic-in-English].md`, kebab-case

---

## Output Template

```markdown
---
title: "[原标题]"
date: [YYYY-MM-DD]
type: content-analysis
source: [url/file/paste]
tags:
  - summary
  - [content-domain-tag]
---

# [原标题(保留原本语言)] - [非中文翻译为中文的标题]

## Core Summary - 核心概括

> [!abstract] TLDR
> [一句话概括核心主题和定位]
>
> - **[维度1]**：[关键发现，简洁有力]
> - **[维度2]**：[关键发现]
> - **[维度3]**：[关键发现]
> - **[结论/底线]**：[最终判断]

Core Summary 的要求：先用一句话点明主题，然后用 3-5 个 bullets 分别覆盖文章的核心维度。每个 bullet 用 bold stem 开头，后跟简洁描述。总长度控制在 5-8 行。

---

## Mind Map - 思维导图大纲

使用 box-drawing 字符（├──、└──、│）绘制树状文本思维导图，展示完整的内容结构和逻辑关系。

---

## Theme Analysis - 主题深度解析

### Theme [N]: [English Theme Name]

| Dimension | Insight | Supporting Evidence |
|-----------|---------|---------------------|
| **[维度名1]** | [核心洞察，一句话] | [具体引用/数据/案例] |
| **[维度名2]** | [核心洞察] | [具体引用/数据] |
| **[维度名3]** | [核心洞察] | [具体引用/数据] |

> [!tip]- Top 3 Actionable Recommendations
> 1. **[行动标题]**：[具体建议，综合多个维度的洞察提炼而成]
> 2. **[行动标题]**：[具体建议]
> 3. **[行动标题]**：[具体建议]

表格保留三列：Dimension、Insight、Supporting Evidence。表格下方用可折叠 callout 放 Top 3 Actionable Recommendations。

Recommendations 的关键原则：不与表格中的 Dimension 一一对应，而是综合分析所有维度后，提炼出最有价值的 top 3 行动建议。每条建议应具体、可操作、有预期收益。

对于包含风险/警告内容的 theme，使用 `> [!warning]-` 替代 `> [!tip]-`。

#### Data Visualization（仅在有助于理解时包含）

使用文本符号绘制图表：对比矩阵、流程图、时间线等。不要为了可视化而可视化。

---

[重复 Theme sections，每个 theme 之间用 --- 分隔]

---

## PACER Application - 知识消化行动方案

> [!important] PACER 分类：[X] — [Full Name]（[中文名]）
> **判断依据**：[一句话解释为什么选择这个分类，基于内容的主导知识类型]

### 消化行动

[根据 PACER 分类生成对应的行动内容，参见 PACER Classification Guide]

### 思考问题

[嵌入 2-4 个针对性问题，教练式语气，用 checkbox 格式]

- [ ] [问题1]
- [ ] [问题2]
- [ ] [问题3]
```

---

## Formatting Guide — Obsidian Callouts 使用规范

Use Obsidian callouts strategically to highlight key content. Do not overuse — typically 2-4 callouts per report.

**Recommended callout usage:**

- `> [!abstract] TLDR` — Wrap the Core Summary (always use this one)
- `> [!important] Key Finding` — For the single most important discovery across all themes
- `> [!warning] Risk/Caveat` — For critical risks, caveats, or counterarguments worth highlighting
- `> [!tip] Actionable Insight` — For the most valuable actionable recommendation

**Callout formatting rules:**
- Callouts support full markdown inside them (bold, bullets, links)
- Use foldable callouts `> [!type]-` for supplementary details that readers may want to expand
- Never nest callouts more than one level deep — it hurts readability
- The Core Summary callout is mandatory; others are optional based on content

---

## Style Rules

1. Use `-` for bullet points, never `*`
2. Bold the opening stem of each bullet for scannability: `- **Key point.** Follow-up explanation.`
3. Keep paragraphs short — max 3-4 sentences
4. English for: theme names, dimension names, key concepts, technical terms
5. Chinese for: descriptions, explanations, analysis, recommendations
6. Tables may contain bulleted lists for complex cells
7. Mind Map uses box-drawing characters (├── └── │) for clean text-based tree rendering
8. Data Visualization uses text-based charts (ASCII art): comparison matrices, flowcharts, timelines, etc.
9. Avoid content duplication — if data appears in a callout, do not repeat it in a table or visualization
10. No trailing summaries or "以上是..." closings — the report ends with the PACER Application section

---

## Handling Different Content Types

**Podcast/Video Transcripts:**
- If speakers are identifiable, attribute key viewpoints to specific speakers
- Tolerate transcription errors — infer correct terms from context
- Focus on substantive arguments, skip filler/banter

**Technical Articles:**
- Preserve technical terminology accurately
- Include code snippets or command examples in evidence column when relevant

**News/Opinion Pieces:**
- Clearly distinguish facts from opinions in the Insight column
- Note the author's perspective/bias if detectable

**Research Papers/Reports:**
- Prioritize methodology and findings over literature review
- Highlight statistical claims with specific numbers

---

## PACER Classification Guide

PACER 将信息分为五类，每类对应特定的消化策略。在生成报告时，综合分析整篇内容的主导知识类型，选择**最相关的一个**分类，然后使用对应的行动模板生成 PACER Application section。

### Classification Decision Logic

按以下优先级判断内容的主导类型：

1. **内容是否主要教你"如何做"某事？** → P（Procedural）
   - 教程、操作指南、工具使用方法、编程教学、流程说明
2. **内容是否主要解释"是什么/为什么"？** → C（Conceptual）
   - 理论框架、设计哲学、原理解释、学科知识、思维模型
3. **内容是否主要提供案例/数据来支撑某个论点？** → E（Evidence）
   - 案例研究、数据报告、实验结果、行业分析
4. **内容是否与读者已有知识高度相关，可建立类比？** → A（Analogous）
   - 跨领域对比、新旧方法对比、竞品分析、范式迁移
5. **内容是否主要是细节性参考信息？** → R（Reference）
   - API 文档、配置参数、公式列表、术语表

注意：A（类比性）可嵌套于 P 和 C 中。如果内容主要是 P 或 C，但有明显的类比机会，在行动模板中补充"类比锚点"即可，不需要将整体改为 A。

### Action Templates by Category

#### P — Procedural → Practice（尽早实践）

```
### 消化行动

这篇内容的核心是**程序性知识**——你需要通过实践来消化，而不是记忆。

1. **[具体实践步骤1]**：[基于文章内容提取的第一步操作]
2. **[具体实践步骤2]**：[第二步操作]
3. **[具体实践步骤3]**：[第三步操作]
4. **记录发现**：实践后写下你的关键发现和遇到的问题
5. **对比预期**：实际体验与文章描述有何不同？

如果当前没有条件实践，停止消费更多内容，等有条件时再回来。

### 思考问题

- [ ] [与实践体验相关的反思问题]
- [ ] [与现有工作流对比的问题]
- [ ] [关于何时/如何在真实场景中应用的问题]
```

#### A — Analogous → Critique（批判性审视）

```
### 消化行动

这篇内容与你已有的知识高度相关，消化的关键是**建立并批判类比**。

**类比 1**：[文章核心概念] ↔ [读者可能已知的概念]
- 相似之处：[具体列出]
- 关键差异：[具体列出]

**类比 2**：[另一个概念] ↔ [另一个已知概念]
- 相似之处：[具体列出]
- 关键差异：[具体列出]

### 思考问题

- [ ] 这个类比在什么情况下会失效？
- [ ] 如果用[已知概念]的思路来理解[新概念]，会遗漏什么？
- [ ] 能否找到一个更好的类比来理解这个概念？
```

#### C — Conceptual → Mapping（网络化笔记）

```
### 消化行动

这篇内容的核心是**概念性知识**——你需要通过建立知识网络来消化，而不是线性记忆。

**核心概念节点**：
1. **[概念A]** — [一句话定义]
2. **[概念B]** — [一句话定义]
3. **[概念C]** — [一句话定义]

**关键连接**：
- [概念A] → [概念B]：[关系描述]
- [概念B] → [概念C]：[关系描述]

拿出纸/平板，用这些节点和连接画一张概念图。随着理解加深，重新组织节点位置。

### 思考问题

- [ ] [概念A]和[概念B]之间还有什么隐含的关系？
- [ ] 如果去掉[概念C]，整个框架还成立吗？
- [ ] 这个框架最薄弱的环节是什么？
```

#### E — Evidence → Store & Rehearse（存储 + 应用性复习）

```
### 消化行动

这篇内容主要提供**证据性信息**——用来支撑更大的概念框架。

**值得存储的关键证据**：
1. **[事实/数据点1]** — 支撑了[某概念]
2. **[事实/数据点2]** — 支撑了[某概念]
3. **[事实/数据点3]** — 支撑了[某概念]

**存储建议**：将以上信息加入 Obsidian 笔记或 Anki 闪卡。
**复习方式**：尝试用这些证据去解释或论证某个观点，而不是单纯背诵。

### 思考问题

- [ ] 这些证据支撑的核心论点是什么？
- [ ] 如果要用其中一个数据点写一段论证，你会怎么写？
```

#### R — Reference → Store & Rehearse（存储 + 间隔重复）

```
### 消化行动

这篇内容主要是**参考性信息**——细节性的事实，需要时能查到即可。

**值得存储的参考信息**：
1. **[参考项1]** — [简要说明用途]
2. **[参考项2]** — [简要说明用途]
3. **[参考项3]** — [简要说明用途]

**存储建议**：存入 Obsidian 或 Anki。如果需要从记忆中调取，使用 Anki 间隔重复；如果只需查阅，存入笔记即可。
**不要在阅读时死记这些信息**——把时间留给更重要的概念性和程序性知识。
```
