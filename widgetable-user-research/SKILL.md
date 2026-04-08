---
name: widgetable-user-research
description: "Widgetable 用户群体需求扫描：从 Reddit、TikTok、Instagram 及 Web 全面扫描 Widgetable 目标用户群体（异地恋情侣、Z世代女性、闺蜜社交）的需求信号，结构化写入飞书多维表格，并生成深度洞察报告文档。触发词：需求扫描、用户需求、user research、群体需求、需求挖掘、跑一下需求扫描、扫描用户需求。"
---

# Widgetable 用户群体需求扫描

从 Reddit、TikTok、Instagram + Web 全面扫描 Widgetable 目标用户群体的需求信号，覆盖竞品用户、潜在用户和周边社区。与产品反馈采集（widgetable-feedback）的区别：反馈采集聚焦"现有用户对 Widgetable 的吐槽和好评"，需求扫描聚焦"目标用户群体在更广泛场景下的未满足需求和市场机会"。

## 飞书多维表格信息

- **Base Token**: `PvZdbnjBpamsaaskD3Jc3ki3nqe`
- **Table ID (用户群体需求)**: `tblzhWnhrNVmMIm4`
- **Base URL**: https://my.feishu.cn/base/PvZdbnjBpamsaaskD3Jc3ki3nqe

## 表字段结构（11 字段）

| 字段名 | 类型 | 写入方式 | 说明 |
|--------|------|----------|------|
| ID | auto_number | 只读 | 自动编号，不写入 |
| 需求描述 | text | 字符串 | 一句话概括需求（如"情侣小游戏集合 — 双人互动小游戏"） |
| Widgetable 现状 | select | 字符串 | 可选值：未满足 / 部分满足 / 痛点 / 机会 / 优势 |
| 渠道 | select | 字符串 | 主要来源渠道：Reddit / TikTok / Instagram |
| 用户画像 | multi_select | 字符串数组 | 可选值：异地恋情侣 / Z世代女性 / 闺蜜社交 |
| 日期 | datetime | "YYYY-MM-DD HH:mm:ss" | 发现日期 |
| 需求类型 | select | 字符串 | 可选值：新功能 / 功能增强 / 体验优化 / 市场机会 / Bug修复 / 设计方向 / 平台扩展 |
| 机会评估 | select | 字符串 | 可选值：高 / 中 / 低 |
| 竞品关联 | text | 字符串 | 涉及的竞品名称和关系 |
| 原始内容 | text | 字符串 | 多渠道采集的原始信息汇总 |
| 产品启发及优化方向 | text | 字符串 | 具体的产品建议和实现路径 |

## 扫描范围

### 目标用户群体
- **异地恋情侣（LDR）**: r/LongDistance、r/LDR、TikTok #ldr #longdistance
- **Z世代女性（13-25岁）**: r/iOSwidgets、r/iOS、TikTok #widgetable #homescreen、Instagram #phoneaesthetic
- **闺蜜社交**: r/besties、TikTok #bff #bestfriendwidget、Instagram 闺蜜相关话题

### 扫描渠道与关键词

#### Reddit
- **直接社区**: r/Widgetable、r/iOSwidgets、r/androidapps
- **用户群体社区**: r/LongDistance、r/LDR、r/teenagers
- **搜索关键词组合**:
  - `widget app couples / besties / LDR`
  - `Widgetable vs Locket / Pookie / Between`
  - `best couple app 2026`
  - `long distance relationship app recommendation`

#### TikTok
- **话题标签**: #widgetable、#couplewidget、#ldrcouple、#phoneaesthetic、#homescreensetup
- **竞品话题**: #locket、#pookie、#coupleapp
- **搜索关键词**: `couple widget app`、`long distance app`、`bff widget`

#### Instagram + Web
- **Instagram 标签**: #widgetable、#homescreenaesthetic、#couplegoals + widget
- **Web 搜索**: 竞品评测文章、App 对比博客、产品猎人讨论
- **竞品动态**: Locket / Pookie / Paired / SumOne / Between 最近更新和用户反响

### 竞品监控列表

| 竞品 | 类型 | 关注点 |
|------|------|--------|
| Locket Widget | 直接竞品 | 锁屏照片、Android 表现、免费策略 |
| Pookie | 直接竞品 | 游戏、白板、相册全家桶 |
| BondBee | 直接竞品 | 情侣社交 widget |
| Paired | 间接竞品 | 每日问答、关系测试 |
| SumOne | 间接竞品 | 问答喂宠模式 |
| Between | 间接竞品 | 共享日历、相册 |
| Friends App | 跨界参考 | AI 伴侣对话 |
| Mellow | 跨界参考 | AI 情绪支持 |
| Bond Touch | 跨界参考 | 物理触觉异步互动 |

## 执行流程

### 第一步：确认扫描范围

默认扫描最近 **7 天**的内容。如用户指定其他时间范围则按用户要求。

### 第二步：三渠道并行采集

使用 Agent 工具并行启动 **3 个采集 Agent**，最大化效率：

#### Agent 1: Reddit 深度采集
1. 用 `mcp__agent-browser` 或 `mcp__exa__web_search_exa` 扫描上述 Reddit 社区
2. 搜索关键词组合，提取与 Widgetable 目标用户群相关的讨论
3. 重点关注：功能请求帖、竞品对比帖、"求推荐"帖、吐槽帖
4. 对每条有价值的发现记录：原文、社区、互动量、用户画像推断

#### Agent 2: TikTok 趋势采集
1. 用 `mcp__agent-browser` 浏览 TikTok 相关话题和竞品账号
2. 用 `mcp__exa__web_search_exa` 搜索 TikTok 上的趋势讨论
3. 重点关注：病毒视频话题、评论区需求呼声、竞品推荐/吐槽视频
4. 记录播放量/互动量作为需求热度参考

#### Agent 3: Instagram + Web 综合采集
1. 用 `mcp__exa__web_search_exa` 搜索 Instagram 公开内容和 Web 文章
2. 用 `mcp__firecrawl__firecrawl_scrape` 抓取竞品评测文章
3. 重点关注：设计趋势、市场机会、竞品新功能上线反响、UGC 传播模式
4. 补充搜索 Product Hunt、App Store 评论聚合站的竞品信息

**每个 Agent 的输出要求**:
- 每条发现包含：需求描述、来源渠道、用户画像、原始内容、竞品关联、机会评估
- 要求去重（同一需求在一个渠道内只记一次，跨渠道的在后续步骤合并）
- 数量目标：每个渠道 15-25 条原始发现

### 第三步：去重整合与结构化

三个 Agent 返回后，进行全局去重和结构化：

1. **跨渠道去重**: 同一需求在多个渠道被提及的，合并为一条记录，在"原始内容"中注明多渠道来源
2. **需求分类**: 判断每条需求的类型（新功能/功能增强/体验优化/市场机会/Bug修复/设计方向/平台扩展）
3. **机会评估**: 根据以下维度综合评定高/中/低
   - 用户痛点强度（强烈需要 vs 锦上添花）
   - 提及频率（多渠道高频 vs 单渠道偶尔）
   - 竞品验证（竞品已做且用户好评 = 需求验证）
   - Widgetable 可行性（技术难度、与现有功能的协同）
   - 商业价值（拉新/留存/付费潜力）
4. **Widgetable 现状判断**:
   - 未满足：Widgetable 完全没有此功能
   - 部分满足：有相关功能但不够好
   - 痛点：现有功能存在问题
   - 机会：外部市场机会（非功能层面）
   - 优势：Widgetable 已有的竞争优势

目标：去重后产出 **25-35 条**唯一需求记录。

### 第四步：写入飞书多维表格

将每条需求构造为 JSON 文件，通过 `lark-cli` 写入：

```bash
lark-cli base +record-upsert \
  --base-token PvZdbnjBpamsaaskD3Jc3ki3nqe \
  --table-id tblzhWnhrNVmMIm4 \
  --json @record.json
```

**关键注意事项：**
- 必须将 JSON 写入工作目录下的临时文件，用 `@filename.json` 传参（Windows 下直接传 JSON 字符串有转义问题）
- 文件路径必须是相对路径（lark-cli 限制）
- 写入完成后清理临时 JSON 文件
- 串行写入，每条间隔 0.5 秒，避免并发冲突
- 每写一条确认返回 `"ok": true`

JSON 示例：
```json
{
  "需求描述": "情侣小游戏集合 — 双人互动小游戏（真心话大冒险、情侣问答等）",
  "Widgetable 现状": "未满足",
  "渠道": "TikTok",
  "用户画像": ["异地恋情侣", "Z世代女性"],
  "日期": "2026-04-05 00:00:00",
  "需求类型": "新功能",
  "机会评估": "高",
  "竞品关联": "Pookie（内置多款游戏）、Paired、Between",
  "原始内容": "TikTok 上 couple games 话题热度极高...",
  "产品启发及优化方向": "开发轻量双人小游戏模块（2-3款起步）..."
}
```

### 第五步：生成洞察报告

参考 `references/report_template.md` 中的模板格式，生成深度洞察报告。

报告框架：
1. **高机会需求深度分析**（每条高机会需求包含）:
   - 需求概述
   - 用户痛点验证（真/伪需求判断 + 痛点强度 + 受众覆盖）
   - 竞品现状表格（竞品名 / 是否满足 / 具体实现 / 用户评价）
   - 可参考竞品
   - Widgetable 实现路径（可行性 + 推荐方案 + 与现有功能联动 + 落地建议）
   - 商业价值评估（拉新/留存/付费点/风险）
2. **竞品威胁矩阵**

用 `lark-cli docs +create` 创建飞书文档：
```bash
lark-cli docs +create --markdown "$(cat report.md)" --title "Widgetable 用户群体需求扫描报告（起始日期~结束日期）"
```

### 第六步：汇总展示

向用户展示本次执行结果：
- 各渠道采集条数
- 去重后总记录数
- 飞书 Base 链接
- 洞察报告文档链接
- 高机会需求 TOP 5 速览

## 已知限制与应对

| 问题 | 应对方案 |
|------|---------|
| TikTok 搜索需登录 | 不登录时用 Exa 搜索 + 官方账号页面采集 |
| Instagram 手机验证码 | 改用 Exa 搜索 + Firecrawl 抓取公开页面 |
| Reddit 帖子点击后重定向/黑屏 | 从列表页直接提取文本，不进入单个帖子 |
| lark-cli JSON 转义问题 | 始终用临时文件 + `@filename.json` 方式传参 |
| lark-cli 文件路径限制 | 必须使用相对路径，文件放在当前工作目录下 |

## 与产品反馈采集（widgetable-feedback）的区别

| 维度 | 产品反馈采集 | 用户群体需求扫描 |
|------|------------|-----------------|
| 聚焦点 | Widgetable 现有用户的反馈 | 目标用户群的广泛需求 |
| 数据来源 | Widgetable 官方渠道为主 | 竞品社区、用户群体社区为主 |
| 写入表 | 产品反馈记录 (tblUyCkgbEL8ljpo) | 用户群体需求 (tblzhWnhrNVmMIm4) |
| 产出 | 反馈周报（问题驱动） | 洞察报告（机会驱动） |
| 频率 | 每周 | 每周或按需 |

## Widgetable 产品背景

Widgetable 是一款面向海外用户的社交小组件应用：
- **核心功能**: 共养宠物、社交小组件（距离/睡眠/心情）、桌面个性化
- **用户群体**: 13-25岁 Z 世代，女性为主，大量异地恋情侣和闺蜜用户
- **市场**: 全球，以美国和西语地区为主
- **竞品格局**: Locket（锁屏照片）、Pookie（全家桶）、Paired（关系问答）、Between（情侣工具）
