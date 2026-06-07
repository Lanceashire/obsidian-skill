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

## 推荐输入

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
