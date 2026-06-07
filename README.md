# obsidian-skill

这是一个 Codex/Agents skill 仓库，目前包含 `book-to-obsidian`：

把导出的 AI 学习对话和教材整合成一个可直接放进 Obsidian 使用的分层知识库。它不是简单的教材摘要，而是以你的真实学习对话为主线，结合教材做校验、补漏和系统化整理。

## 核心能力

- 以 AI 学习对话作为主要教学来源，权重为 60%。
- 以教材作为结构、术语、公式、覆盖面和正确性来源，权重为 40%。
- 保留教材章节层级，按章节、小节、最小有意义小节生成 Markdown 笔记。
- 自动识别反复追问、误解、纠正、类比、例题和薄弱点。
- 将对话中的真实困惑写进笔记，而不是把对话简单附在末尾。
- 用教材纠正对话中的错误或不严谨说法。
- 生成对话问题索引、学习薄弱点索引和权重报告。

## 仓库结构

```text
.
├── README.md
└── book-to-obsidian/
    ├── SKILL.md
    ├── README.md
    ├── CHANGELOG-v0.4.md
    ├── references/
    │   ├── conversation-primary-rules.md
    │   ├── depth-routing-rules.md
    │   ├── quality-checklist.md
    │   └── section-note-template.md
    └── assets/
        └── vault-template/
            ├── indexes/
            └── mappings/
```

## 安装方法

把整个 `book-to-obsidian` 文件夹复制到你的 skill 目录。

常见位置：

```text
~/.agents/skills/book-to-obsidian
```

如果你使用的是 Codex 本地 skill 目录，也可以放到：

```text
~/.codex/skills/book-to-obsidian
```

最终目录里应该能直接看到：

```text
book-to-obsidian/
├── SKILL.md
├── references/
└── assets/
```

## 使用方式一：直接拖拽文件

最省事的方式是直接把文件拖到 Codex 对话里，然后调用 skill。

你可以拖入：

- 导出的 AI 学习对话 `.md` 文件；
- 教材 PDF、Markdown 或文本文件；
- 你自己的补充笔记。

然后这样说：

```text
请使用 $book-to-obsidian，读取我刚刚拖进来的对话 Markdown 和教材，
按对话 60%、教材 40% 的权重生成 Obsidian vault，输出到 ./vault。
```

在这种模式下，不需要提前把对话 Markdown 放进 `inputs/conversations/`。skill 会优先使用当前对话里上传或拖拽的附件，并把附件文件名保存在来源记录里。

如果 Codex 当前环境无法直接读取附件内容，或者文件数量很大、教材很长，再改用下面的工作区目录方式。

## 使用方式二：工作区目录

如果文件很多，或者你希望以后反复运行同一批材料，建议为每次转换准备一个独立工作目录：

```text
workspace/
├── inputs/
│   ├── conversations/
│   │   ├── 学习对话-01.md
│   │   └── 学习对话-02.md
│   ├── textbook/
│   │   └── 教材.pdf
│   └── notes/
│       └── 我的补充笔记.md
└── vault/
```

### conversations

放从 ChatGPT、Claude、Gemini 或其他 AI 工具导出的学习对话 Markdown。

这些对话会被作为主要来源，用来判断：

- 你真正卡住的问题；
- 哪些概念需要重点展开；
- 哪些误区需要纠正；
- 哪些类比、例子或解释方式对你有效；
- 哪些问题还没有完全解决。

### textbook

放教材 PDF、Markdown、文本文件或教材目录。

教材负责：

- 保留章节结构；
- 校验定义和术语；
- 补全公式、表格、例题和系统知识；
- 纠正对话中的错误；
- 防止重要章节因为对话没有提到就被漏掉。

### notes

可选。放你自己的补充笔记、课堂记录、错题、代码实验记录等。

## 基本调用方式

在 Codex 或支持 skill 的 agents 环境里，可以这样说：

```text
请使用 $book-to-obsidian，按对话 60%、教材 40% 的权重，
将 inputs/conversations 中的学习记录和 inputs/textbook 中的教材
整合成 Obsidian 笔记，输出到 vault。
```

如果是拖拽附件模式，可以这样说：

```text
请使用 $book-to-obsidian，读取本轮对话里上传的 Markdown 对话和教材，
生成 Obsidian vault 到 ./vault。
```

也可以给得更具体：

```text
请使用 $book-to-obsidian，把这门数据挖掘课程整理成 Obsidian vault。
要求：
1. 对话是主要依据，教材用于校验和补漏；
2. 保留教材章节结构；
3. 每个最小小节单独生成一个 Markdown；
4. 对我反复问过的问题加深讲解；
5. 生成对话问题索引和学习薄弱点索引。
```

## 输出结果

生成的 Obsidian vault 通常类似：

```text
vault/
├── 00-全书目录.md
├── chapters/
│   ├── 01-绪论/
│   │   ├── 00-本章目录.md
│   │   ├── 01-01-什么是数据挖掘.md
│   │   └── 99-本章复习与日志.md
│   └── ...
├── indexes/
│   ├── 术语表.md
│   ├── 公式速查.md
│   ├── 对话问题索引.md
│   ├── 学习薄弱点索引.md
│   └── 待深入学习的问题.md
├── mappings/
│   ├── conversation-inventory.yaml
│   ├── dialogue-to-section-map.yaml
│   ├── synthesis-weight-report.yaml
│   └── unresolved-dialogue-fragments.md
└── assets/
    └── images/
```

## 单篇笔记格式

每个小节笔记会包含 front matter，用于记录来源和生成策略：

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
  - 学习对话-01.md
depth_level: deep-concept
depth_upgrade_reason:
  - 用户反复追问
status: draft
---
```

正文通常包含：

- 你在对话中真正卡住的问题；
- 本小节核心结论；
- 直觉理解；
- 教材中的规范定义；
- 机制、分类或步骤；
- 教材补充内容；
- 对话中出现的典型例子；
- 易混淆概念；
- 边缘情况、限制与常见误区；
- 对话中的典型问题；
- 与其他知识点的联系；
- 本小节复习清单。

## 深度等级

skill 会根据对话证据和教材重要性自动决定笔记深度。

```text
对话证据：60%
教材重要性：40%
```

支持四类深度：

- `orientation`：绪论、概览、过渡内容。
- `standard`：普通概念，覆盖定义、直觉、机制、例子和易错点。
- `deep-concept`：核心概念，例如噪声、缺失值、数据质量、预处理、相似度、过拟合等。
- `algorithm-or-math`：算法、公式、度量、推导和评价指标。

当对话里出现反复追问、明确困惑、多次纠正、代码困难或边缘情况时，笔记会自动升级深度。

## 质量要求

使用时建议检查：

- 是否明确体现对话 60%、教材 40%；
- 是否保留教材章节层级；
- 是否每个最小有意义小节都有独立 Markdown；
- 是否把反复困惑写成重点讲解；
- 是否纠正了对话中的错误结论；
- 是否没有复制长篇聊天原文；
- 是否生成了对话问题索引；
- 是否生成了学习薄弱点索引；
- 是否保留了无法映射的对话片段。

详细检查清单见：

```text
book-to-obsidian/references/quality-checklist.md
```

## 版本

当前版本：`v0.4`

主要变化：

- 将对话提升为主导来源；
- 设置对话 60%、教材 40% 的权重；
- 新增学习薄弱点索引；
- 新增教材补充 callout；
- 保留教材层级结构，但不机械摘要教材。

详见：

```text
book-to-obsidian/CHANGELOG-v0.4.md
```
