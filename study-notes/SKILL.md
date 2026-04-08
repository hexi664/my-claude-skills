---
name: study-notes
version: 1.0.0
description: "学习笔记速记：当用户说「记一下」「学到了」「记个笔记」「知识点」等表达时触发，将知识点分析整理后写入飞书多维表格。也支持「复习」「回顾」来查看近期笔记。"
---

# 学习笔记速记 Skill

快速将对话中学到的知识点记录到飞书多维表格，方便后续查阅和定期复习。

## 触发词

当用户消息中包含以下意图时自动触发：
- 记录类：「记一下」「记个笔记」「记录一下」「学到了」「知识点」「帮我记」「存一下」
- 复习类：「复习」「回顾」「看看笔记」「最近学了什么」「知识点回顾」

## 多维表格信息

- **Base Token**: `VuC8biARTaoJsbsbS3ictfQsn3b`
- **Table ID**: `tblXoF22tLASOsJk`
- **Base URL**: https://my.feishu.cn/base/VuC8biARTaoJsbsbS3ictfQsn3b

## 表字段结构

| 字段名 | 字段ID | 类型 | 写入方式 | 说明 |
|--------|--------|------|----------|------|
| ID | fldEZGzoMt | auto_number | 只读 | 自动编号 NO.001 |
| 笔记标题 | fld1pPxRHm | text | 字符串 | 用一句话概括知识点 |
| 学习主题 | fldYXsVgcB | select（单选） | 字符串 | 可选值：编程、数学、英语、AI/机器学习、设计、其他 |
| 笔记内容 | fldekmIqBH | text | 字符串 | 知识点的详细说明，结构化整理，便于未来复习 |
| 关键收获 | fldyvTMr9S | text | 字符串 | 用最精炼的几句话总结核心收获，方便快速回顾 |
| 重要程度 | fldavqjILq | number (rating) | 数字 1-5 | 星级评分，根据知识点的重要性和实用性判断 |
| 学习日期 | fldQDRAUHm | datetime | "YYYY-MM-DD HH:mm:ss" | 记录时的日期 |

## 记录知识点流程

当用户要求记录知识点时：

### 1. 分析用户输入

从用户的描述中提取：
- **标题**：用一句简短的话概括知识点核心
- **主题**：判断属于哪个分类（编程/数学/英语/AI机器学习/设计/其他）
- **内容**：用清晰易懂的语言重新整理知识点，包含关键概念、原理、示例。目标是未来看到这段文字就能快速回忆起来
- **来源**：如果用户提到了参考链接则记录
- **重要程度**：根据实用性和基础性评 1-5 星
- **标签**：判断属于基础/进阶/实战/理论，新记录默认加上「待复习」

### 2. 直接写入（不用确认）

用户确认后，构造 JSON 并写入：

```bash
lark-cli base +record-upsert \
  --base-token VuC8biARTaoJsbsbS3ictfQsn3b \
  --table-id tblXoF22tLASOsJk \
  --json '@record.json'
```

JSON 示例：
```json
{
  "笔记标题": "Python 列表推导式比 for 循环快的原因",
  "学习主题": "编程",
  "笔记内容": "列表推导式在 CPython 中会编译为专用的 LIST_APPEND 字节码...",
  "学习来源": "https://example.com/article",
  "重要程度": 4,
  "学习日期": "2026-04-04 12:00:00",
  "已掌握": false,
  "标签": ["进阶", "待复习"]
}
```

**注意**：由于 Windows PowerShell 的 JSON 转义问题，必须将 JSON 写入临时文件，然后用 `@filename.json` 方式传参。临时文件放在当前工作目录下，写入完成后清理。

### 4. 确认结果

写入成功后告知用户，并附上飞书表格链接。

## 复习知识点流程

当用户要求复习/回顾时：

### 1. 查询最近的笔记

```bash
lark-cli base +record-list \
  --base-token VuC8biARTaoJsbsbS3ictfQsn3b \
  --table-id tblXoF22tLASOsJk \
  --limit 20
```

### 2. 整理复习清单

按主题分组展示，优先显示：
- 标签含「待复习」的
- 「已掌握」为 false 的
- 重要程度高的

格式：
```
📚 本周学习回顾（共 N 条）

🔵 编程（X 条）
  1. [标题] ⭐⭐⭐⭐ — 核心要点简述
  2. ...

🟣 AI/机器学习（X 条）
  1. ...

---
哪些知识点你已经掌握了？告诉我编号，我帮你标记。
```

### 3. 标记已掌握

用户确认掌握后，更新对应记录：

```bash
lark-cli base +record-upsert \
  --base-token VuC8biARTaoJsbsbS3ictfQsn3b \
  --table-id tblXoF22tLASOsJk \
  --record-id recXXX \
  --json '@update.json'
```

将「已掌握」设为 true，并移除「待复习」标签。

## 重要规则

1. **内容质量优先**：不要原封不动记录用户的话，要用更清晰、结构化的方式重新整理，让未来的自己一看就能想起来
2. **先确认再写入**：每次写入前都要展示整理结果让用户确认
3. **JSON 文件方式传参**：在 Windows 上必须用文件方式传 JSON，避免 PowerShell 转义问题
4. **只写存储字段**：不要写入 ID（auto_number）等只读字段
5. **日期格式**：统一用 `YYYY-MM-DD HH:mm:ss` 格式
