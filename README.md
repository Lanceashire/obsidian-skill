# obsidian-skill

这是一个 Codex/Agents skill 仓库，目前包含 `book-to-obsidian`：

把导出的 AI 学习对话和教材整合成 Obsidian 笔记。主要用法是：把 Obsidian 的笔记文件夹当作 Codex/Claude 项目打开，直接在现有 vault 里新建或更新课程笔记。

## 核心能力

- 以 AI 学习对话作为主要教学来源，权重为 60%。
- 以教材作为结构、术语、公式、覆盖面和正确性来源，权重为 40%。
- 保留教材章节层级，按章节、小节、最小有意义小节生成 Markdown 笔记。
- 自动识别反复追问、误解、纠正、类比、例题和薄弱点。
- 将对话中的真实困惑写进笔记，而不是把对话简单附在末尾。
- 善用 Markdown 结构和 LaTeX 公式，生成清晰、详尽、实用但不过度冗长的笔记。
- 用教材纠正对话中的错误或不严谨说法。
- 对数据结构、数据挖掘等主题补充高价值外部资源，例如 LeetCode 练习、数据集网站、可视化工具和资料链接，并添加用途注释。
- 生成对话问题索引、学习薄弱点索引、外部资源索引和权重报告。

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

## 主要用法：打开 Obsidian Vault 后直接运行

推荐工作流：

1. 在 Codex 或 Claude 中，把 Obsidian 的 vault 文件夹作为项目文件夹打开。
2. 把从 ChatGPT 或其他 AI 导出的学习对话 `.md` 文件直接拖进对话。
3. 同时提供教材 PDF、Markdown、文本文件或教材路径。
4. 调用 `$book-to-obsidian`，让它直接在当前 vault 里创建或更新笔记。

调用示例：

```text
请使用 $book-to-obsidian，以当前项目作为 Obsidian vault。
读取我拖进来的对话 Markdown 和教材。
先判断当前 vault 里有没有对应课程/教材的总文件夹：
如果有，就沿着已有章节和命名方式继续生成；
如果没有，就先创建一个课程总文件夹，再在里面生成章节和子文件。
不要创建额外的 vault 子目录，不要删除或重命名我已有的笔记。
```

比如你已经有：

```text
ObsidianVault/
└── 数据结构/
    ├── 01-绪论/
    ├── 02-线性表/
    └── 06-图/
```

现在学到第七章排序，skill 应该继续生成：

```text
ObsidianVault/
└── 数据结构/
    └── 07-排序/
```

如果当前 vault 里还没有 `数据结构/`，skill 应该先创建：

```text
ObsidianVault/
└── 数据结构/
    ├── 00-课程目录.md
    └── 07-排序/
```

它也支持更小粒度的增量更新，比如只生成第七章里的某个知识点、把某个知识点拆成子文件、或根据新的对话更新已有笔记。

## 次要用法一：只拖拽文件并生成新 Vault

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

## 次要用法二：工作区目录

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

如果当前项目就是 Obsidian vault，可以这样说：

```text
请使用 $book-to-obsidian，以当前项目作为 Obsidian vault，
读取本轮上传的对话和教材。先定位或创建对应课程总文件夹，
再在该课程文件夹里新建或更新章节、知识点和子文件。
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

## 内容风格

默认要求：

- 知识点讲解详尽、清晰、有条理，适合长期复习；
- 避免无意义扩写和过度复杂的表述；
- 善用 Markdown 的标题、列表、表格、callout、链接、代码块和检查清单；
- 善用 LaTeX 表达公式、递推式、复杂度、概率、矩阵、距离度量和优化目标；
- 写出公式后解释符号含义、适用条件和常见错误；
- 数据结构主题可补充 LeetCode、算法可视化、权威参考等链接；
- 数据挖掘主题可补充 UCI、Kaggle、OpenML、Hugging Face Datasets 等数据集来源；
- 外部资源必须有简短注释，说明为什么值得看或怎么用。

## 质量要求

使用时建议检查：

- 是否明确体现对话 60%、教材 40%；
- 是否保留教材章节层级；
- 是否每个最小有意义小节都有独立 Markdown；
- 是否善用了 Markdown 和 LaTeX；
- 是否把反复困惑写成重点讲解；
- 是否纠正了对话中的错误结论；
- 是否为合适主题补充了带注释的外部资源；
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
