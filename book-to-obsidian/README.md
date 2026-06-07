# Book to Obsidian v0.4 — 对话主导版

## 核心权重

```text
AI 对话：60%
教材：40%
```

## 含义

这不是教材摘要工具。

它会：

1. 先读你导出的 AI 对话；
2. 找出你真正卡住的问题；
3. 找出反复追问、误解、纠正、类比和例题；
4. 再用教材补漏、校验和完善；
5. 输出按章节、小节拆分的 Obsidian Vault。

## 内容要求

- 笔记必须高质量、有条理、实用，知识点要讲详尽。
- 避免无意义扩写和过度复杂的表达。
- 善用 Markdown：标题、列表、表格、callout、链接、代码块和检查清单。
- 善用 LaTeX：公式、递推式、复杂度、概率、矩阵、距离度量和优化目标。
- 写出公式后解释符号含义、适用条件和常见错误。
- 数据结构主题可补充 LeetCode、算法可视化和权威参考链接。
- 数据挖掘主题可补充 UCI、Kaggle、OpenML、Hugging Face Datasets 等数据集来源。
- 外部资源必须加简短注释，说明为什么值得看或怎么用。

## 主要用法：打开 Obsidian Vault 后运行

推荐把 Obsidian 的 vault 文件夹作为 Codex/Claude 项目打开，然后把从 ChatGPT 或其他 AI 导出的对话 Markdown 直接拖进对话，再调用：

```text
请使用 $book-to-obsidian，以当前项目作为 Obsidian vault。读取我拖进来的对话 Markdown 和教材。先判断当前 vault 里有没有对应课程/教材的总文件夹：如果有，就沿着已有章节和命名方式继续生成；如果没有，就先创建一个课程总文件夹，再在里面生成章节和子文件。不要创建额外的 vault 子目录，不要删除或重命名我已有的笔记。
```

例如当前已有 `数据结构/01-绪论` 到 `数据结构/06-图`，现在学到第七章排序，就继续创建 `数据结构/07-排序`。如果还没有 `数据结构/`，就先创建这个课程总文件夹。

也可以只生成或更新某个局部知识点，比如第七章排序里的一个小节，并按教材和对话继续拆成子文件。

## 次要用法：只拖拽并生成新 Vault

可以直接把导出的对话 Markdown 和教材拖到 Codex 对话里，然后说：

```text
请使用 $book-to-obsidian，读取我刚刚拖进来的对话 Markdown 和教材，按对话 60%、教材 40% 的权重生成 Obsidian vault，输出到 ./vault。
```

这种模式下，不需要提前把对话 Markdown 放进工作区。

如果附件不可读、文件很多或教材很长，再使用下面的目录结构。

## 次要用法：推荐输入目录

```text
workspace/
├── inputs/
│   ├── conversations/
│   ├── textbook/
│   └── notes/
└── vault/
```

## 安装

复制整个文件夹到：

```text
~/.agents/skills/book-to-obsidian
```

调用：

```text
请使用 $book-to-obsidian，按对话 60%、教材 40% 的权重，将 conversations 中的学习记录和教材整合成 Obsidian 笔记。
```
