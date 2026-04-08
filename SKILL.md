---
name: summarizer
description: Analyze and summarize articles, podcast transcripts, video transcriptions, and long-form content into structured reports with Obsidian callouts. Use when the user shares a URL, file path, or pastes text and asks to summarize, analyze, digest, review, or extract insights from any content. Also triggers on phrases like "summarize this article", "help me analyze", "summarize this", "digest this transcript", or when user provides a transcript/article for analysis.
---

# Summarizer — Bilingual Content Analysis & Structured Summarization

Transform any article, podcast transcript, or video transcript into a structured, polished Obsidian note — including a core summary, mind map, in-depth theme analysis, and actionable recommendations.

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

- **Theme count confirmation**: "Content is approximately X words. I identified the following N core themes: [list theme names]. Does the count and direction look right?" with options like the suggested number, +1, -1, or "You decide"
- **Only ask additional questions if genuinely uncertain** about something — e.g., if the content covers multiple distinct domains and you're unsure which angle the user cares about, or if the title is ambiguous

If the user has already specified preferences in their prompt (e.g., "generate 5 themes", "focus on the technical section"), skip the corresponding question and respect their instruction.

### Phase 3: Generate Report

**Before writing the report**, load the `obsidian:obsidian-markdown` skill using the Skill tool to ensure correct Obsidian formatting syntax (especially callouts and properties).

Generate the report following the Output Template below. Apply these principles:

- **Bilingual mixed style** — English for topic names, dimension names, key concepts; Chinese for detailed descriptions and explanations
- **Information density first** — Every sentence should carry meaning. No filler, no redundant transitions
- **Evidence-driven** — Insights must be backed by specific quotes, data, or examples from the source
- **Actionability** — Every theme must include concrete, actionable recommendations
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
title: "[Original Title]"
date: [YYYY-MM-DD]
type: content-analysis
source: [url/file/paste]
tags:
  - summary
  - [content-domain-tag]
---

# [Original Title (in its original language)] - [Title translated to English if not already in English]

## Core Summary

> [!abstract] TLDR
> [One-sentence summary of the core theme and positioning]
>
> - **[Dimension 1]**: [Key finding, concise and impactful]
> - **[Dimension 2]**: [Key finding]
> - **[Dimension 3]**: [Key finding]
> - **[Conclusion/Bottom Line]**: [Final judgment]

Core Summary requirements: Lead with one sentence that states the theme clearly, then use 3-5 bullets to cover the article's core dimensions. Each bullet starts with a bold stem followed by a concise description. Keep total length to 5-8 lines.

---

## Mind Map

Use box-drawing characters (├──, └──, │) to draw a text-based tree mind map showing the complete content structure and logical relationships.

---

## Theme Analysis

### Theme [N]: [English Theme Name]

| Dimension | Insight | Supporting Evidence |
|-----------|---------|---------------------|
| **[Dimension 1]** | [Core insight, one sentence] | [Specific quote/data/case] |
| **[Dimension 2]** | [Core insight] | [Specific quote/data] |
| **[Dimension 3]** | [Core insight] | [Specific quote/data] |

> [!tip]- Top 3 Actionable Recommendations
> 1. **[Action title]**: [Specific recommendation synthesized from across multiple dimensions]
> 2. **[Action title]**: [Specific recommendation]
> 3. **[Action title]**: [Specific recommendation]

The table keeps three columns: Dimension, Insight, Supporting Evidence. Below the table, use a foldable callout for Top 3 Actionable Recommendations.

Key principle for Recommendations: do not map one-to-one with table Dimensions — instead, synthesize all dimensions and distill the top 3 most valuable action recommendations. Each recommendation should be specific, actionable, and have a clear expected benefit.

For themes that contain risk/warning content, use `> [!warning]-` instead of `> [!tip]-`.

#### Data Visualization (include only when it aids understanding)

Use text symbols to draw charts: comparison matrices, flowcharts, timelines, etc. Do not visualize for the sake of visualization.

---

[Repeat Theme sections, separate each theme with ---]

---

## PACER Application

> [!important] PACER Classification: [X] — [Full Name]
> **Rationale**: [One sentence explaining why this classification was chosen, based on the dominant knowledge type of the content]

### Digest Actions

[Generate the corresponding action content based on PACER classification — see PACER Classification Guide]

### Reflection Questions

[Embed 2-4 targeted questions in coaching tone, using checkbox format]

- [ ] [Question 1]
- [ ] [Question 2]
- [ ] [Question 3]
```

---

## Formatting Guide — Obsidian Callouts Usage

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
10. No trailing summaries or closing remarks — the report ends with the PACER Application section

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

PACER divides information into five categories, each corresponding to a specific digestion strategy. When generating a report, comprehensively analyze the dominant knowledge type of the entire content, select **the single most relevant** category, and then use the corresponding action template to generate the PACER Application section.

### Classification Decision Logic

Determine the dominant content type using the following priority order:

1. **Does the content primarily teach you "how to do" something?** → P (Procedural)
   - Tutorials, operation guides, tool usage, programming instruction, process explanations
2. **Does the content primarily explain "what/why"?** → C (Conceptual)
   - Theoretical frameworks, design philosophy, principle explanations, subject knowledge, mental models
3. **Does the content primarily provide cases/data to support an argument?** → E (Evidence)
   - Case studies, data reports, experiment results, industry analysis
4. **Is the content highly relevant to existing reader knowledge, enabling analogies?** → A (Analogous)
   - Cross-domain comparisons, old-vs-new method comparisons, competitive analysis, paradigm shifts
5. **Is the content primarily detailed reference information?** → R (Reference)
   - API documentation, configuration parameters, formula lists, glossaries

Note: A (Analogous) can be nested within P and C. If content is primarily P or C but has clear analogy opportunities, add an "analogy anchor" in the action template — no need to change the overall classification to A.

### Action Templates by Category

#### P — Procedural → Practice (practice as early as possible)

```
### Digest Actions

The core of this content is **procedural knowledge** — you need to digest it through practice, not memorization.

1. **[Specific practice step 1]**: [First action extracted from the article content]
2. **[Specific practice step 2]**: [Second action]
3. **[Specific practice step 3]**: [Third action]
4. **Record findings**: After practicing, write down your key discoveries and problems encountered
5. **Compare against expectations**: How does the actual experience differ from the article's description?

If you don't currently have the conditions to practice, stop consuming more content and come back when you do.

### Reflection Questions

- [ ] [Reflection question related to the practice experience]
- [ ] [Question comparing with existing workflows]
- [ ] [Question about when/how to apply in real scenarios]
```

#### A — Analogous → Critique (critical examination)

```
### Digest Actions

This content is highly relevant to your existing knowledge — the key to digestion is **building and critiquing analogies**.

**Analogy 1**: [Core concept from article] ↔ [Concept the reader likely already knows]
- Similarities: [List specifically]
- Key differences: [List specifically]

**Analogy 2**: [Another concept] ↔ [Another known concept]
- Similarities: [List specifically]
- Key differences: [List specifically]

### Reflection Questions

- [ ] In what circumstances would this analogy break down?
- [ ] If you used the lens of [known concept] to understand [new concept], what would you miss?
- [ ] Can you find a better analogy to understand this concept?
```

#### C — Conceptual → Mapping (networked notes)

```
### Digest Actions

The core of this content is **conceptual knowledge** — you need to digest it by building a knowledge network, not linear memorization.

**Core concept nodes**:
1. **[Concept A]** — [One-sentence definition]
2. **[Concept B]** — [One-sentence definition]
3. **[Concept C]** — [One-sentence definition]

**Key connections**:
- [Concept A] → [Concept B]: [Relationship description]
- [Concept B] → [Concept C]: [Relationship description]

Get out paper/tablet and draw a concept map using these nodes and connections. Reorganize node positions as your understanding deepens.

### Reflection Questions

- [ ] What implicit relationship exists between [Concept A] and [Concept B]?
- [ ] If you removed [Concept C], would the entire framework still hold?
- [ ] What is the weakest link in this framework?
```

#### E — Evidence → Store & Rehearse (store + applied review)

```
### Digest Actions

This content primarily provides **evidential information** — used to support a larger conceptual framework.

**Key evidence worth storing**:
1. **[Fact/data point 1]** — supports [some concept]
2. **[Fact/data point 2]** — supports [some concept]
3. **[Fact/data point 3]** — supports [some concept]

**Storage recommendation**: Add the above information to Obsidian notes or Anki flashcards.
**Review method**: Try using this evidence to explain or argue a viewpoint, rather than pure memorization.

### Reflection Questions

- [ ] What is the core argument that this evidence supports?
- [ ] If you were to use one of these data points to write an argument, how would you write it?
```

#### R — Reference → Store & Rehearse (store + spaced repetition)

```
### Digest Actions

This content is primarily **reference information** — detailed facts that you only need to be able to look up when needed.

**Reference information worth storing**:
1. **[Reference item 1]** — [Brief description of its use]
2. **[Reference item 2]** — [Brief description of its use]
3. **[Reference item 3]** — [Brief description of its use]

**Storage recommendation**: Store in Obsidian or Anki. If you need to retrieve it from memory, use Anki spaced repetition; if you only need to look it up, store it in notes.
**Do not try to memorize this information while reading** — save that time for more important conceptual and procedural knowledge.
```
