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
