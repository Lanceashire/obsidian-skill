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

## 最省事的用法：直接拖拽

可以直接把导出的对话 Markdown 和教材拖到 Codex 对话里，然后说：

```text
请使用 $book-to-obsidian，读取我刚刚拖进来的对话 Markdown 和教材，按对话 60%、教材 40% 的权重生成 Obsidian vault，输出到 ./vault。
```

这种模式下，不需要提前把对话 Markdown 放进工作区。

如果附件不可读、文件很多或教材很长，再使用下面的目录结构。

## 直接在 Obsidian Vault 中运行

也可以把 Obsidian 的 vault 文件夹作为 Codex 项目打开，然后说：

```text
请使用 $book-to-obsidian，以当前项目作为 Obsidian vault。读取我拖进来的对话 Markdown 和教材，直接在当前 vault 中新建或更新笔记。不要创建额外的 vault 子目录，不要删除或重命名我已有的笔记。
```

这种模式会直接把生成结果写进当前 vault，适合后续按章节、按小节或按单个文件增量更新。

更新已有文件时，应保留已有 front matter、标签、别名、双链和手写内容；不确定能否安全覆盖时，先生成候选稿或更新日志。

## 推荐输入目录

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
