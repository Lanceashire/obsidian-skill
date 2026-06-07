# Book to Obsidian

对话主导的 Obsidian 学习笔记 skill。

它会把导出的 AI 学习对话和教材融合成 Obsidian 笔记：对话负责学习重点和真实困惑，教材负责结构、术语、公式、覆盖面和校验。

```text
AI 学习对话：60%
教材：40%
```

## 主用法

推荐把 Obsidian vault 文件夹作为 Codex/Claude 项目打开，然后把从 ChatGPT 或其他 AI 导出的对话 Markdown 直接拖进对话。

教材可以：

- 上传整个教材文件；
- 提供本地路径；
- 在用户允许后联网搜索公开教材或参考资料。

调用：

```text
请使用 $book-to-obsidian，以当前项目作为 Obsidian vault。
读取我拖进来的对话 Markdown 和教材。
如果当前 vault 已有对应课程总文件夹，就沿着已有章节继续生成；
如果没有，就先创建课程总文件夹。
不要创建额外的 vault 子目录，不要删除或重命名我已有的笔记。
```

## 省 Token 读取

读取教材正文前先确认：

1. 按章节读取，推荐。
2. 直接读取完整本教材。
3. 指定某章、某节、页码范围或知识点。

默认推荐：先读目录和当前目标章节，再匹配导出的对话 Markdown；内容不够时再读取相关前置章节或引用章节。

## 内容标准

- 笔记高质量、有条理、实用，知识点讲详尽。
- 善用 Markdown 和 LaTeX。
- 公式后解释符号含义、适用条件和常见错误。
- 按主题补充外部资源，例如练习题、案例、数据集、工具、可视化、官方文档、论文、标准或权威参考。
- 外部资源必须有用途注释。

## 参考文件

```text
SKILL.md
references/conversation-primary-rules.md
references/depth-routing-rules.md
references/quality-checklist.md
references/section-note-template.md
```
