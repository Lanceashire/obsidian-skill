---
name: book-to-obsidian
description: Convert exported AI learning conversations plus a textbook into a hierarchical Obsidian vault. Use conversation Markdown as the primary source of teaching focus and explanation style (60%) and the textbook as the supporting source for structure, terminology, coverage, correction, and systematic completion (40%). Create one folder per chapter and one Markdown note per smallest meaningful subsection. Prioritize the user's real questions, misunderstandings, follow-ups, analogies, examples, and preferred explanations, while using the textbook to fill gaps and ensure coverage. Automatically adjust depth based on both conversation evidence and textbook importance.
---

# Book to Obsidian

Convert exported AI learning conversations and a textbook into a hierarchical Obsidian-compatible Markdown vault.

## 1. Core synthesis model

Use a weighted synthesis model:

```text
AI learning conversations: 60%
Textbook: 40%
```

This weighting applies to:

- explanation emphasis
- note depth
- teaching order inside each subsection
- examples
- common mistakes
- review questions
- which topics deserve extra expansion

It does **not** mean ignoring textbook correctness.

### 1.1 Conversation layer — primary teaching source (60%)

The exported AI conversation Markdown files are the main source for:

- what the learner actually asked
- which points caused confusion
- which explanations were helpful
- repeated follow-up questions
- user misunderstandings
- corrected reasoning
- analogies
- worked examples
- debugging notes
- learning preferences
- unfinished questions
- topics that need extra depth

The notes should reflect the learner's real learning path, not merely restate the textbook.

### 1.2 Textbook layer — supporting and correcting source (40%)

The textbook is used to:

- verify terminology
- preserve chapter and subsection hierarchy
- fill missing concepts
- complete definitions
- add formulas and examples not discussed in conversation
- ensure systematic coverage
- correct mistakes or speculative claims from conversation
- prevent important textbook sections from being skipped

### 1.3 Conflict handling

If a conversation conflicts with the textbook:

1. Preserve the textbook-correct version.
2. Record the conversation-derived misunderstanding as an easy-to-confuse point.
3. Add a visible callout:

```markdown
> [!warning] 对话中暴露的易错点
> 学习对话中曾出现……
> 教材中更准确的表述是……
```

Do not silently preserve an incorrect dialogue answer.

## 2. Output principle

The final vault is not a textbook summary.

It is a learner-specific study system:

```text
对话中的真实学习路径
+
教材中的系统性知识
=
适合长期复习的 Obsidian 笔记
```

## 3. Required inputs

Required:

- one or more exported AI learning conversation `.md` files, either attached in the current Codex conversation or available as filesystem paths
- textbook source file or textbook directory, either attached in the current Codex conversation or available as filesystem paths
- output directory for the generated vault

Optional:

- personal Markdown notes
- preferred note language
- preferred depth
- topics that need special attention

Default to:

- conversation weight: `0.60`
- textbook weight: `0.40`
- same language as the user
- detailed notes
- textbook hierarchy preserved
- one chapter folder per chapter
- one Markdown note per smallest meaningful subsection

### 3.1 Input priority

Prefer attached or dragged-in files when the user provides them in the current Codex conversation.

Use this priority:

1. Current conversation attachments, including dragged-in Markdown files, PDFs, notes, or text files.
2. Explicit filesystem paths supplied by the user.
3. The current workspace root when it is an Obsidian vault.
4. The recommended `inputs/` layout below.

Do not require the user to copy conversation Markdown files into the workspace if the same files are already attached and readable in the current conversation.

If attached files are available only as summarized chat context and not as readable file contents, ask the user for either:

- the actual attached file content,
- a workspace path,
- or a zip/folder containing the files.

When using attached files, preserve each attachment name as `source_file` in inventories, mappings, front matter, and source notes.

### 3.2 Obsidian vault project mode

The user may open an existing Obsidian vault folder as the current Codex project/workspace and ask this skill to generate or update notes in place.

Detect this mode when:

- the current workspace contains `.obsidian/`;
- the user says the current project is the Obsidian vault;
- the requested output is `.`, the current folder, or an existing vault path;
- existing Obsidian notes, chapter folders, indexes, or mappings are already present.

In this mode:

- treat the current workspace root as the vault root;
- write generated chapter folders, indexes, mappings, manifests, and assets directly under the vault root unless the user names a subfolder;
- do not create an extra nested `vault/` directory;
- preserve existing Obsidian folder order, naming conventions, front matter, tags, aliases, backlinks, and user-written notes;
- update existing generated notes only when they map to the same chapter/section identity;
- prefer patch-style edits over full rewrites when updating a note;
- never delete or rename existing user notes unless the user explicitly asks;
- record update decisions in `mappings/update-log.md` or `manifests/update-log.md`.

For an existing note, use this update policy:

1. Read the existing note first.
2. Preserve front matter keys that are not owned by this skill.
3. Preserve user sections that are outside generated section headings.
4. Refresh generated sections when new dialogue or textbook evidence changes the content.
5. Add new backlinks and index entries without breaking old links.
6. If a conflict is unclear, create a proposed replacement note or a diff note instead of overwriting.

Recommended invocation:

```text
Please use $book-to-obsidian in the current Obsidian vault. Read the attached conversation Markdown and textbook, then create or update notes directly in this vault.
```

### 3.3 Drag-and-run usage

The user may drag exported conversation Markdown files and a textbook file directly into the Codex conversation, then invoke:

```text
Please use $book-to-obsidian on the attached conversation Markdown files and attached textbook. Generate the Obsidian vault in ./vault.
```

In this mode:

- treat attached Markdown files that look like exported AI conversations as `inputs/conversations`;
- treat attached textbook PDFs, Markdown files, text files, or directories as `inputs/textbook`;
- treat attached personal notes as `inputs/notes`;
- generate all durable outputs in the requested workspace output directory, or directly in the current Obsidian vault when vault project mode is active;
- still create `mappings/`, `indexes/`, and provenance records.

## 4. Recommended filesystem input layout

This layout is optional when files are provided as current conversation attachments or when the current project is already the target Obsidian vault.

```text
workspace/
├── inputs/
│   ├── conversations/
│   │   ├── 数据挖掘学习对话-01.md
│   │   ├── 数据挖掘学习对话-02.md
│   │   └── ...
│   ├── textbook/
│   │   └── 数据挖掘导论.pdf
│   └── notes/
│       └── 我的补充笔记.md
├── mappings/
├── extracted/
├── chunks/
└── vault/
```

## 5. Hierarchical output rule

Preserve the textbook chapter hierarchy, but let conversation content drive emphasis inside each note.

Never place an entire chapter into one Markdown file if the textbook contains multiple sections.

If vault project mode is active, the `vault/` root shown below means the current Obsidian workspace root, not a nested folder named `vault`.

Create:

```text
vault/
├── 00-全书目录.md
├── chapters/
│   ├── 01-绪论/
│   │   ├── 00-本章目录.md
│   │   ├── 01-01-什么是数据挖掘.md
│   │   ├── 01-02-数据挖掘要解决的问题/
│   │   │   ├── 00-小节目录.md
│   │   │   ├── 01-02-01-可伸缩性.md
│   │   │   ├── 01-02-02-高维性.md
│   │   │   └── ...
│   │   └── 99-本章复习与日志.md
│   └── ...
├── indexes/
│   ├── 术语表.md
│   ├── 公式速查.md
│   ├── 数据集与外部资源.md
│   ├── 对话问题索引.md
│   ├── 学习薄弱点索引.md
│   └── 待深入学习的问题.md
├── mappings/
│   ├── conversation-inventory.yaml
│   ├── dialogue-to-section-map.yaml
│   ├── synthesis-weight-report.yaml
│   └── unresolved-dialogue-fragments.md
├── manifests/
└── assets/
    └── images/
```

## 6. Smallest meaningful subsection

A smallest meaningful subsection is the deepest textbook heading that still forms a coherent teaching unit.

Rules:

- Preserve textbook numbering.
- Create one note per smallest meaningful subsection.
- If a parent section has independent explanatory content, create `XX-XX-00-概述.md`.
- If a conversation contains an important supplementary topic that does not map cleanly to a textbook subsection, place it in:
  ```text
  chapters/<chapter>/supplementary/
  ```
  or:
  ```text
  mappings/unresolved-dialogue-fragments.md
  ```
- Do not distort the textbook hierarchy merely to fit a conversation.

## 7. Token-efficient workflow

Never load all textbook pages and all conversation files in one pass.

Use:

1. Inspect conversation files.
2. Extract useful dialogue fragments.
3. Inspect textbook.
4. Build textbook hierarchy.
5. Map dialogue fragments to textbook subsections.
6. Rank subsections by:
   - conversation frequency
   - confusion intensity
   - textbook importance
   - prerequisite importance
7. Process one smallest meaningful subsection at a time.
8. Read only:
   - relevant dialogue fragments
   - matching textbook chunk
   - needed templates
9. Generate note.
10. Update chapter index.
11. Generate global indexes.
12. Validate and fix only affected notes.

## 8. Stage 1 — inspect conversations first

Read `references/conversation-primary-rules.md`.

For every exported conversation `.md` file:

1. Preserve filename.
2. Split into useful fragments.
3. Extract:
   - user question
   - AI answer
   - follow-up
   - correction
   - misconception
   - analogy
   - worked example
   - edge case
   - debugging note
   - unresolved issue
   - learner preference
4. Remove:
   - greetings
   - filler
   - repeated low-value turns
   - unrelated private information
   - off-topic content
5. Create:
   ```text
   mappings/conversation-inventory.yaml
   ```

## 9. Stage 2 — inspect textbook

Use textbook to:

1. Detect chapters, sections, and subsections.
2. Preserve numbering.
3. Identify formulas, tables, examples, diagrams, URLs, datasets, and references.
4. Build a machine-readable outline.
5. Mark missing or unreadable content.
6. Use textbook as a coverage checklist.

## 10. Stage 3 — map conversations to textbook subsections

For every useful dialogue fragment, record:

```yaml
fragment_id: dialogue-001-fragment-003
source_file: 数据质量学习对话.md
type: misconception
summary: 用户容易混淆噪声和离群点
target_section: "02.02.01"
confidence: high
conversation_weight: 0.60
textbook_weight: 0.40
action: integrate
```

Rules:

- Prefer mapping to textbook subsections.
- Allow multiple dialogue fragments per subsection.
- Do not force-map uncertain fragments.
- Store unresolved fragments separately.
- Record how much dialogue material influenced each note.

## 11. Stage 4 — choose note depth

Read `references/depth-routing-rules.md`.

Choose:

- `orientation`
- `standard`
- `deep-concept`
- `algorithm-or-math`

### 11.1 Dialogue-driven upgrade

Upgrade depth when conversations show:

- repeated questions
- explicit confusion
- multiple corrections
- requests for deeper explanation
- need for extra examples
- hidden edge cases
- code or debugging difficulty
- prerequisite weakness

### 11.2 Textbook-driven upgrade

Upgrade depth when textbook content is:

- central to later chapters
- formula-heavy
- algorithmic
- a foundational concept
- frequently reused
- easy to confuse
- clearly developed in depth

### 11.3 Weighting rule

When deciding depth:

```text
conversation evidence: 60%
textbook importance: 40%
```

If conversation evidence and textbook importance disagree:

- repeated user difficulty can upgrade a topic
- important textbook core topics must not be downgraded below the minimum depth required for correctness

## 12. Stage 5 — generate subsection notes

Each note should reflect the user's actual learning path.

Use front matter:

```yaml
---
chapter: 第 X 章
section: X.X
title: 小节标题
textbook_source: 教材来源
conversation_weight: 0.60
textbook_weight: 0.40
dialogue_enrichment: true
dialogue_sources:
  - 数据挖掘学习对话-01.md
depth_level: deep-concept
depth_upgrade_reason:
  - 用户反复追问
status: draft
---
```

Recommended structure:

```markdown
# X.X 小节标题

## 1. 你在对话中真正卡住的问题

## 2. 本小节核心结论

## 3. 直觉理解

## 4. 教材中的规范定义

## 5. 为什么需要它

## 6. 机制、分类或步骤

## 7. 教材补充：对话中未覆盖但必须掌握的内容

## 8. 对话中出现的典型例子

## 9. 易混淆概念

## 10. 边缘情况、限制与常见误区

## 11. 对话中的典型问题

## 12. 与其他知识点的联系

## 13. 工程日志与待确认事项

## 14. 来源说明

## 15. 本小节复习清单
```

## 13. Provenance callouts

Use:

```markdown
> [!note] 对话补充
> 来自学习对话中有价值的解释、类比或例子。

> [!info] 教材补充
> 对话未涉及，但教材要求掌握的系统性内容。

> [!warning] 对话中暴露的易错点
> 对话中出现过的误解，以及教材校准后的正确表述。

> [!question] 对话中尚未解决的问题
> 仍需继续确认的问题。
```

## 14. Automatic depth rules

### `orientation`

For introductions, previews, roadmaps, and transitions.

Must explain:

- what it is
- why it matters
- what problem it solves
- one example
- later connections
- what to learn now

### `standard`

For normal concepts.

Cover:

- motivation
- definition
- intuition
- mechanism
- example
- confusion points
- edge cases
- review questions

### `deep-concept`

For foundational concepts such as:

- noise
- missing values
- duplicate data
- data quality
- preprocessing
- sampling
- similarity and dissimilarity
- dimensionality reduction
- feature selection
- overfitting
- clustering objectives
- anomaly definitions
- class imbalance

Cover thoroughly:

- background
- strict definition
- intuition
- subtypes
- judgment criteria
- handling methods
- method comparison
- formulas
- complete examples
- counterexamples
- edge cases
- misconceptions
- downstream impact
- practical use
- Python ideas or pseudocode when useful
- review questions

### `algorithm-or-math`

For algorithms, formulas, measures, derivations, and evaluation metrics.

Cover:

- problem
- input
- output
- variables
- formula
- step-by-step process
- worked example
- assumptions
- limitations
- common errors
- complexity when relevant
- related-method comparison
- Python or pseudocode when useful

## 15. Chapter indexes

After each chapter:

1. Create `00-本章目录.md`.
2. Link all subsection files.
3. Add a knowledge map.
4. List:
   - most frequently discussed topics
   - repeated confusions
   - dialogue-driven depth upgrades
   - textbook-only补漏 topics
   - unresolved questions
5. Create `99-本章复习与日志.md`.

## 16. Global indexes

Create:

```text
indexes/术语表.md
indexes/公式速查.md
indexes/数据集与外部资源.md
indexes/对话问题索引.md
indexes/学习薄弱点索引.md
indexes/待深入学习的问题.md
```

`学习薄弱点索引.md` should summarize:

- repeated questions
- unresolved misconceptions
- prerequisites needing review
- concepts requiring extra exercises
- topics upgraded due to dialogue evidence

## 17. Quality requirements

- Conversation content drives emphasis and learning flow.
- Textbook content fills gaps and guarantees systematic coverage.
- Use 60/40 weighting explicitly.
- Preserve textbook hierarchy.
- Create one note per smallest meaningful subsection.
- Label conversation-derived and textbook-only additions.
- Do not paste raw transcripts.
- Do not skip important textbook content merely because it was not discussed.
- Do not let textbook summary style erase the learner's real questions.
- Upgrade depth based on actual confusion.
- Keep OCR and engineering logs visible when useful.

## 18. Gotchas

- Never treat textbook as the only source.
- Never treat dialogue as merely an appendix.
- Never mechanically summarize the textbook while ignoring the learner's questions.
- Never paste full chat transcripts.
- Never preserve incorrect dialogue claims without correction.
- Never skip textbook-only essentials.
- Never create only one file for an entire chapter with multiple sections.
- Never flatten nested subsections.
- Never make later chapters shallower because context is long.
- Never force-map unrelated dialogue fragments.

## 19. Definition of done

Complete only when:

- conversation files have been inventoried
- textbook hierarchy has been extracted
- dialogue fragments have been mapped
- every textbook chapter has a folder
- every smallest meaningful subsection has a note
- each note records conversation/textbook weighting
- dialogue-driven emphasis is visible
- textbook-only补漏 content is visible
- incorrect dialogue claims are corrected
- recurring questions are indexed
- learning weak points are indexed
- unresolved dialogue fragments remain visible
- core topics are detailed enough
- vault passes validation
