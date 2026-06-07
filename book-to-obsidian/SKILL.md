---
name: book-to-obsidian
description: Create or update course notes directly inside an Obsidian vault opened as the current Codex/Claude project. Use exported AI conversation Markdown attachments as the primary learning source (60%) and the textbook as the supporting source (40%). First locate an existing matching course/book folder in the vault; if it exists, continue its existing chapter route, naming style, indexes, and note structure; if it does not exist, create a new top-level course/book folder and generate chapter/subsection notes inside it. Support incremental work such as adding Chapter 7 after Chapters 1-6 already exist, expanding a single knowledge point into child notes, or updating an existing note from new conversation evidence. Do not create an extra nested vault folder when the current project is already the target Obsidian vault.
---

# Book to Obsidian

Convert exported AI learning conversations and a textbook into hierarchical Obsidian-compatible Markdown notes.

The primary workflow is to open the user's existing Obsidian vault as the current Codex/Claude project, attach or drag in exported AI conversation Markdown files plus the textbook, then create or update notes directly inside that vault.

If the matching course/book folder already exists, continue its existing structure. If it does not exist, create a new top-level course/book folder first, then generate the chapter and subsection notes inside it.

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
- textbook source, either uploaded/local files in any readable format or user-approved web search results
- target Obsidian vault or output directory

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

Use this priority for sources and target:

1. Current workspace root as the target when it is an Obsidian vault.
2. Existing course/book folders inside the current vault.
3. Current conversation attachments as source files, including dragged-in Markdown files, PDFs, notes, or text files.
4. Uploaded or local textbook files in any readable format.
5. Explicit filesystem paths supplied by the user.
6. User-approved web search for public textbook or reference sources.
7. The recommended `inputs/` layout below.

Do not require the user to copy conversation Markdown files into the workspace if the same files are already attached and readable in the current conversation.

If attached files are available only as summarized chat context and not as readable file contents, ask the user for either:

- the actual attached file content,
- a workspace path,
- or a zip/folder containing the files.

When using attached files, preserve each attachment name as `source_file` in inventories, mappings, front matter, and source notes.

### 3.1.1 Textbook source modes

Support two textbook input modes.

#### Mode A: uploaded or local textbook files

The user may upload, drag in, or provide local paths to the textbook in any format the current environment can read or convert, including:

- PDF
- Markdown
- plain text
- Word documents
- EPUB
- HTML
- images or scanned pages when OCR is available
- folders containing chapter files, notes, slides, code, figures, or textbook companion material
- zip archives when the environment can extract them

When using uploaded or local files:

- preserve original filenames and paths in provenance records;
- extract or inspect the table of contents before reading large content;
- ask for the textbook reading strategy before reading textbook body content;
- if a format is unreadable, ask the user for a converted file, OCR text, or a different source.

#### Mode B: web-searched textbook or reference source

The user may ask the agent to search the web for public textbooks, open course notes, official documentation, papers, datasets, or other references.

Before using web search:

- ask for permission unless the user has already explicitly requested web search;
- clarify the target book/course/topic when ambiguous;
- prefer official, public, legal, and authoritative sources;
- record source URLs, titles, access dates, and confidence;
- distinguish a true textbook/source from supplementary references;
- do not use random summaries, SEO pages, or unreliable blog posts as the textbook authority;
- do not claim to have read a full book from search results unless the actual readable content was accessed.

When both uploaded/local files and web sources are available:

- treat the uploaded/local textbook as the primary textbook source unless the user says otherwise;
- use web sources for补漏, examples, exercises, datasets, official documentation, or cross-checking;
- clearly label web-derived additions in source notes.

### 3.2 Primary workflow: Obsidian vault project mode

The preferred use is:

1. Open the Obsidian vault folder as the current Codex/Claude project.
2. Attach or drag in exported conversation Markdown files from ChatGPT or other AI tools.
3. Provide the textbook by uploading a file, giving a local path, or asking for user-approved web search.
4. Ask this skill to create or update notes directly inside the current vault.

The user may be continuing an existing course, such as adding Chapter 7 after Chapters 1-6 already exist, or generating a deeper child note for one knowledge point inside an existing chapter.

Detect this mode when:

- the current workspace contains `.obsidian/`;
- the user says the current project is the Obsidian vault;
- the requested output is `.`, the current folder, or an existing vault path;
- existing Obsidian notes, chapter folders, indexes, or mappings are already present.

In this mode:

- treat the current workspace root as the vault root;
- first locate the matching course/book root folder inside the vault;
- if the matching course/book root folder exists, continue that folder's existing route, naming style, chapter order, indexes, and note structure;
- if no matching course/book root folder exists, create a new top-level course/book folder under the vault root and generate the course structure inside it;
- write generated chapter folders, indexes, mappings, manifests, and assets inside the course/book root folder unless the user names another target;
- do not create an extra nested `vault/` directory;
- preserve existing Obsidian folder order, naming conventions, front matter, tags, aliases, backlinks, and user-written notes;
- update existing generated notes only when they map to the same chapter/section identity;
- prefer patch-style edits over full rewrites when updating a note;
- never delete or rename existing user notes unless the user explicitly asks;
- record update decisions in `mappings/update-log.md` or `manifests/update-log.md`.

### 3.2.1 Course/book root selection

Before generating files in vault project mode:

1. Inspect the vault root and likely study folders.
2. Look for an existing folder whose name, aliases, index files, chapter names, tags, or front matter match the textbook/course.
3. If one clear match exists, use it as the course/book root.
4. If multiple plausible matches exist, ask the user which folder to use.
5. If no match exists, create a new top-level folder named after the course/book, using the vault's existing naming style when possible.

Examples:

```text
ObsidianVault/
├── 数据结构/
│   ├── 01-绪论/
│   ├── 02-线性表/
│   └── 06-图/
```

If the user asks to generate Chapter 7 Sorting, create:

```text
ObsidianVault/
└── 数据结构/
    └── 07-排序/
```

If `数据结构/` does not exist, create it first:

```text
ObsidianVault/
└── 数据结构/
    ├── 00-课程目录.md
    └── 07-排序/
```

### 3.2.2 Incremental generation

Support partial, incremental generation:

- add a new chapter after existing chapters;
- add a missing subsection inside an existing chapter;
- expand one knowledge point into child notes;
- update one existing note using new dialogue evidence;
- refresh indexes only for affected folders unless the user asks for a full rebuild.

Do not require all textbook chapters to be generated in one run. If the user is currently learning Chapter 7, focus on Chapter 7 and its dependencies, while preserving links to existing Chapters 1-6.

For an existing note, use this update policy:

1. Read the existing note first.
2. Preserve front matter keys that are not owned by this skill.
3. Preserve user sections that are outside generated section headings.
4. Refresh generated sections when new dialogue or textbook evidence changes the content.
5. Add new backlinks and index entries without breaking old links.
6. If a conflict is unclear, create a proposed replacement note or a diff note instead of overwriting.

Recommended invocation:

```text
Please use $book-to-obsidian in the current Obsidian vault. Read the attached conversation Markdown, use the uploaded/local textbook or ask before web-searching for a public source, locate or create the matching course folder, then create or update notes directly there.
```

### 3.3 Drag-and-run usage

The user may drag exported conversation Markdown files and a textbook file directly into the Codex conversation, then invoke:

```text
Please use $book-to-obsidian on the attached conversation Markdown files and attached textbook. If the current project is an Obsidian vault, locate or create the course folder and update it in place; otherwise generate the vault in ./vault.
```

In this mode:

- treat attached Markdown files that look like exported AI conversations as `inputs/conversations`;
- treat attached textbook PDFs, Markdown files, text files, or directories as `inputs/textbook`;
- treat attached personal notes as `inputs/notes`;
- generate all durable outputs in the requested workspace output directory, or directly in the current Obsidian vault when vault project mode is active;
- still create `mappings/`, `indexes/`, and provenance records.

## 4. Recommended filesystem input layout

This layout is secondary. Use it when the current project is not the target Obsidian vault, or when the user prefers a separate staging workspace.

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

If vault project mode is active, the `vault/` root shown below means the selected course/book root folder inside the current Obsidian workspace, not a nested folder named `vault`.

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

Before reading textbook content, confirm the textbook reading scope unless the user has already specified it.

Ask the user to choose one of:

1. Chapter-by-chapter reading, recommended for token efficiency and incremental Obsidian updates.
2. Full-book reading, only when the book is short enough or the user explicitly wants a full global pass.
3. User-specified reading range, such as a chapter, section, page range, topic, or existing note that needs updating.

Default recommendation:

```text
建议按章节读取教材：先读取目录和当前目标章节，再匹配导出的对话 Markdown；如果内容不够，再继续读取相关章节或前置章节。
```

Do not silently read the whole textbook when the user has not chosen a strategy.

If the user chooses chapter-by-chapter reading:

- read the table of contents or outline first;
- identify the target chapter, section, or knowledge point from the user's request, existing vault structure, and conversation evidence;
- read only that chapter or section first;
- match relevant exported conversation fragments to that chapter;
- generate or update notes when the chapter content and dialogue evidence are sufficient;
- if evidence is insufficient, read adjacent prerequisite, follow-up, or cross-referenced sections only as needed;
- continue chapter by chapter until the requested scope is complete.

If the user chooses full-book reading:

- still prefer outline-first processing;
- summarize or chunk the textbook instead of holding the full book in context;
- process chapters sequentially and validate outputs incrementally.

If the user specifies a range:

- read only that range plus the minimum prerequisite or referenced material needed for correctness;
- do not expand to unrelated chapters unless the user approves or the note would be incorrect without them.

Use:

1. Ask or confirm the textbook reading strategy.
2. Inspect conversation files.
3. Extract useful dialogue fragments.
4. Inspect the textbook outline or table of contents.
5. Read the selected chapter, full-book chunks, or user-specified range.
6. Build or update the textbook hierarchy for the selected scope.
7. Map dialogue fragments to textbook subsections.
8. Rank subsections by:
   - conversation frequency
   - confusion intensity
   - textbook importance
   - prerequisite importance
9. Process one smallest meaningful subsection at a time.
10. Read only:
   - relevant dialogue fragments
   - matching textbook chunk
   - needed templates
11. Generate note.
12. Update chapter index.
13. Generate or refresh only affected global indexes.
14. Validate and fix only affected notes.

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

1. Ask or confirm whether to read chapter-by-chapter, full book, or a user-specified range.
2. Detect chapters, sections, and subsections from the outline or selected scope.
3. Preserve numbering.
4. Identify formulas, tables, examples, diagrams, URLs, datasets, and references in the selected scope.
5. Build a machine-readable outline.
6. Mark missing or unreadable content.
7. Use textbook as a coverage checklist for the selected scope.

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

Content quality requirements for every note:

- Provide high-quality, well-organized, practical notes.
- Be detailed enough for real study and review, especially for core knowledge points.
- Avoid unnecessary verbosity, duplicated explanations, and overly complex wording.
- Use Markdown actively: headings, short paragraphs, ordered steps, tables, checklists, callouts, internal links, external links, code blocks, and mermaid diagrams when they genuinely improve understanding.
- Use LaTeX actively for formulas, recurrences, complexity expressions, probability, matrices, optimization objectives, distance measures, and algorithmic definitions.
- Prefer readable display math for important formulas:
  ```markdown
  $$
  T(n)=2T\left(\frac{n}{2}\right)+O(n)
  $$
  ```
- Use inline math for small symbols or terms, such as `$O(n \log n)$`, `$k$`, `$d(x_i,x_j)$`.
- Explain formulas immediately after writing them, including variable meanings, assumptions, and common mistakes.
- For algorithms and data-structure operations, include practical examples, pseudocode or code snippets when useful, edge cases, and complexity.

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

## 15. 外部练习、数据集与延伸资源

## 16. 本小节复习清单
```

### 12.1 External resources

For every subject, add curated external resources with short annotations when they improve learning, practice, application, or verification.

Choose resources by subject type, not by a fixed list:

- practice-heavy subjects: exercises, problem sets, judges, quizzes, worked examples, or labs;
- data-heavy subjects: datasets, benchmark suites, data portals, reproducible notebooks, or dataset documentation;
- tool-heavy subjects: official documentation, API references, tutorials, examples, or configuration guides;
- theory-heavy subjects: papers, standards, textbook companion pages, proofs, lecture notes, or authoritative references;
- visual or process-heavy subjects: simulators, visualizers, interactive demos, diagrams, or case studies;
- applied subjects: real cases, domain reports, regulations, standards, templates, or practitioner guides.

Examples only:

- data structures and algorithms may use LeetCode, algorithm visualizers, or authoritative complexity references;
- data mining may use UCI Machine Learning Repository, Kaggle, OpenML, Hugging Face Datasets, government open data portals, benchmark datasets, papers, or library documentation;
- other subjects must use analogous high-value resources that fit that subject.

Rules:

- Do not dump long link lists.
- Prefer 3-7 high-value resources per note when external resources are relevant.
- Add one short annotation per resource explaining why it belongs here.
- Mark the resource type, such as practice, dataset, visualization, reference, documentation, or paper.
- If current links, dataset availability, or problem titles are uncertain, verify them before writing exact URLs.
- Put broad resources in `indexes/数据集与外部资源.md` or an equivalent external-resource index, and topic-specific resources inside the relevant note.

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

`数据集与外部资源.md` should summarize:

- external practice links grouped by topic;
- datasets, cases, labs, tools, visualizations, documents, papers, standards, or official references grouped by chapter, method, task, or subject;
- subject-appropriate resources, such as LeetCode-style practice for algorithms or dataset repositories for data-oriented topics;
- short annotations explaining how to use each resource;
- source notes and verification dates when exact external URLs are used.

## 17. Quality requirements

- Conversation content drives emphasis and learning flow.
- Textbook content fills gaps and guarantees systematic coverage.
- Notes are detailed, orderly, practical, and suitable for long-term review.
- Use Markdown structure well instead of long unbroken paragraphs.
- Use LaTeX well for formulas and mathematical expressions.
- Explain symbols, assumptions, and common formula mistakes.
- Use 60/40 weighting explicitly.
- Preserve textbook hierarchy.
- Create one note per smallest meaningful subsection.
- Label conversation-derived and textbook-only additions.
- Include curated external resources when they improve learning, using the resource types that fit the current subject.
- Do not paste raw transcripts.
- Do not skip important textbook content merely because it was not discussed.
- Do not let textbook summary style erase the learner's real questions.
- Do not add excessive link lists or unannotated external resources.
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
- Never write formulas as plain text when LaTeX would be clearer.
- Never add external links without explaining why they are useful.

## 19. Definition of done

Complete only when:

- conversation files have been inventoried
- textbook source mode has been recorded as uploaded/local or user-approved web search
- textbook reading strategy has been confirmed and recorded
- textbook hierarchy has been extracted
- dialogue fragments have been mapped
- the target Obsidian vault or output directory has been identified
- the matching course/book root folder has been located or created
- requested chapters, sections, or knowledge points have been generated or updated
- existing unrelated chapters and notes are preserved
- each note records conversation/textbook weighting
- dialogue-driven emphasis is visible
- textbook-only补漏 content is visible
- incorrect dialogue claims are corrected
- Markdown structure and LaTeX formula usage have been checked
- relevant external resources have been annotated and indexed
- recurring questions are indexed
- learning weak points are indexed
- unresolved dialogue fragments remain visible
- core topics are detailed enough
- affected notes, indexes, and mappings pass validation
