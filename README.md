# obsidian-skill

把 AI 学习对话、教材和你的 Obsidian 笔记库接起来的 Codex/Claude skill。

当前仓库包含 `book-to-obsidian`：它会优先读取你从 ChatGPT 或其他 AI 导出的学习对话 Markdown，再用教材做结构、术语、公式和覆盖面的校验补充，直接在 Obsidian vault 里新建或更新课程笔记。

## 推荐工作流

最推荐这样用：

1. 在 Codex 或 Claude 中，把 Obsidian vault 文件夹作为项目打开。
2. 把导出的学习对话 `.md` 文件直接拖进对话。
3. 提供教材：上传文件、给本地路径，或明确要求联网搜索公开资料。
4. 调用 `$book-to-obsidian`，让它在当前 vault 中原地创建或更新笔记。

```text
请使用 $book-to-obsidian，以当前项目作为 Obsidian vault。
读取我拖进来的对话 Markdown 和教材。
先判断当前 vault 里有没有对应课程/教材的总文件夹：
如果有，就沿着已有章节和命名方式继续生成；
如果没有，就先创建一个课程总文件夹，再在里面生成章节和子文件。
不要创建额外的 vault 子目录，不要删除或重命名我已有的笔记。
```

如果你已经有：

```text
ObsidianVault/
└── 数据结构/
    ├── 01-绪论/
    ├── 02-线性表/
    └── 06-图/
```

现在学到第七章，它会继续生成：

```text
ObsidianVault/
└── 数据结构/
    └── 07-排序/
```

如果还没有 `数据结构/`，则先创建课程总文件夹，再在里面生成章节、小节和子文件。

## 它怎么写笔记

`book-to-obsidian` 不是教材摘要工具。它默认使用：

```text
AI 学习对话：60%
教材：40%
```

对话负责发现你真正卡住的问题、反复追问、误解、例子、类比和学习偏好。教材负责章节结构、术语校准、公式、系统覆盖和错误纠正。

生成的笔记会：

- 保留教材章节层级；
- 一章一个文件夹，一个最小有意义小节一个 Markdown；
- 支持只新增某章、某节或某个知识点；
- 更新已有笔记时保留 front matter、标签、别名、双链和手写内容；
- 采用正式学习文档风格，不写成聊天复盘；
- 只把对话中的薄弱点内化为学习重点、易错点、例题和复习检查；
- 用 Markdown 组织结构，用 LaTeX 表达公式；
- 按主题补充练习题、案例、数据集、工具、可视化、官方文档、论文或权威参考；
- 为外部资源添加用途注释，而不是只堆链接。

## 教材来源

教材可以来自两种方式：

| 方式 | 说明 |
| --- | --- |
| 上传或本地文件 | 支持当前环境能读取或转换的格式，例如 PDF、Markdown、txt、Word、EPUB、HTML、扫描页、章节文件夹或 zip。 |
| 联网搜索 | 用户明确允许后，搜索公开教材、课程讲义、官方文档、论文或参考资料。需要记录 URL、标题、访问日期和可信度。 |

如果同时有本地教材和联网资料，默认以本地教材为主，联网资料用于补漏、例题、外部资源和交叉校验。

## 省 Token 读取策略

skill 不应该默认一次性读取完整本教材。读取教材正文前，会先确认策略：

1. `按章节读取`，推荐。先读目录和当前目标章节，再匹配导出的对话 Markdown。
2. `直接读取完整本教材`，只适合教材很短或你明确需要全书统筹。
3. `指定读取范围`，例如某章、某节、页码范围或知识点。

推荐说法：

```text
教材按章节读取。先读目录和当前目标章节，再匹配我上传的对话 Markdown。
如果内容不够，再继续读取相关前置章节或引用章节。
```

## 安装

把整个 `book-to-obsidian` 文件夹复制到你的 skill 目录：

```text
~/.agents/skills/book-to-obsidian
```

或：

```text
~/.codex/skills/book-to-obsidian
```

安装后目录应类似：

```text
book-to-obsidian/
├── SKILL.md
├── references/
└── assets/
```

## 其他用法

### 只拖拽文件并生成新 Vault

```text
请使用 $book-to-obsidian，读取我刚刚拖进来的对话 Markdown 和教材，
按对话 60%、教材 40% 的权重生成 Obsidian vault，输出到 ./vault。
```

### 使用独立工作区

```text
workspace/
├── inputs/
│   ├── conversations/
│   ├── textbook/
│   └── notes/
└── vault/
```

## 输出结构

在新 vault 或课程文件夹中，输出通常类似：

```text
课程名/
├── 00-课程目录.md
├── 第01章-绪论/
│   ├── 01-现实动机/
│   │   ├── 01.1-为什么需要自动化数据分析.md
│   │   └── 01.2-商业工业互联网与物联网应用.md
│   ├── 02-什么是数据挖掘/
│   │   ├── 02.1-数据挖掘的定义.md
│   │   └── 02.2-哪些任务不属于典型数据挖掘.md
│   └── 99-本章复习.md
│   └── ...
├── indexes/
│   ├── 术语表.md
│   ├── 公式速查.md
│   ├── 对话问题索引.md
│   ├── 学习薄弱点索引.md
│   └── 数据集与外部资源.md
└── mappings/
    ├── conversation-inventory.yaml
    ├── dialogue-to-section-map.yaml
    └── synthesis-weight-report.yaml
```

`assets/`、`mappings/`、`manifests/`、`indexes/` 这类辅助目录只有在有实际内容时才创建。空的 `assets/images/` 没有价值，不应该生成。

## 笔记质量标准

- 详尽、清晰、有条理，适合长期复习。
- 正式、规范，不使用聊天式标题或口吻。
- 每个知识点尽量讲清楚定义、直觉、机制、例子、边界、易错点和复习问题。
- 避免无意义扩写和过度复杂表达。
- 善用 Markdown：标题、列表、表格、callout、链接、代码块和检查清单。
- 善用 LaTeX：公式、递推式、复杂度、概率、矩阵、距离度量和优化目标。
- 写出公式后解释符号含义、适用条件和常见错误。
- 外部资源必须有简短注释，说明为什么值得看或怎么用。

详细规则见：

```text
book-to-obsidian/SKILL.md
book-to-obsidian/references/quality-checklist.md
```

## 仓库结构

```text
.
├── README.md
└── book-to-obsidian/
    ├── SKILL.md
    ├── README.md
    ├── CHANGELOG-v0.4.md
    ├── references/
    └── assets/
```
