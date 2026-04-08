---
name: widgetable-feedback
description: "Widgetable 产品反馈周采集：从 Discord、Reddit、TikTok、Instagram、App Store 等渠道采集用户反馈，分类写入飞书多维表格，并生成周报飞书文档。当用户提到 Widgetable 反馈采集、用户反馈周报、产品反馈收集、widgetable feedback、采集本周反馈、生成反馈周报时使用。即使用户只说「跑一下反馈采集」或「本周反馈」也应触发。"
---

# Widgetable 产品反馈周采集

从 Discord、Reddit、TikTok、Instagram、App Store 五大渠道采集 Widgetable 用户反馈，结构化写入飞书多维表格，并自动生成周报文档。

## 飞书多维表格信息

- **Base Token**: `PvZdbnjBpamsaaskD3Jc3ki3nqe`
- **Table ID (产品反馈记录)**: `tblUyCkgbEL8ljpo`
- **Base URL**: https://my.feishu.cn/base/PvZdbnjBpamsaaskD3Jc3ki3nqe

## 表字段结构（12 字段）

| 字段名 | 类型 | 写入方式 | 说明 |
|--------|------|----------|------|
| 频道/位置 | text | 字符串 | 具体来源，如 "r/Widgetable"、"App Store 评论 (美区)" |
| 渠道 | select | 字符串 | 可选值：Discord / Reddit / TikTok / Instagram / App Store |
| 中文摘要 | text | 字符串 | 一句话中文概括反馈核心内容 |
| 用户名 | text | 字符串 | 反馈者用户名，未知时填 "Anonymous" |
| 分类 | select | 字符串 | 可选值：Bug / 功能需求 / 体验槽点 / 好评 / 热点 |
| 严重度 | select | 字符串 | 可选值：P0-紧急 / P1-高 / P2-中 / P3-低 |
| 是否真实痛点 | select | 字符串 | 可选值：是 / 否 |
| 产品启发及优化方向 | text | 字符串 | 给产品团队的具体建议 |
| 原始内容 | text | 字符串 | 用户原文（英文/西语等原始语言） |
| 是否可做需求 | select | 字符串 | 可选值：是 / 否 |
| 日期 | datetime | "YYYY-MM-DD HH:mm:ss" | 反馈发布日期 |
| 相关功能 | multi_select | 字符串数组 | 可选值：宠物系统 / 小组件 / 社交 / 付费 / 其他 |

## 执行流程

运行此 skill 时，按以下步骤顺序执行：

### 第一步：确认采集范围

询问用户本次采集的时间范围（默认最近 7 天）。用当前日期计算起止日期。

### 第二步：多渠道并行采集

使用 Agent 工具并行启动多个采集任务，最大化效率。每个渠道的采集方法如下：

#### Reddit（r/Widgetable）
1. 用 `mcp__agent-browser` 导航到 `https://www.reddit.com/r/Widgetable/new/`
2. 获取页面文本，提取时间范围内的帖子标题和内容
3. 对于有价值的帖子，点击进入查看评论获取更多上下文
4. **注意**：如果点击帖子后出现黑屏或重定向到错误页面，改用列表页已展示的文本内容，不要继续尝试导航

#### TikTok（@widgetable 官方账号）
1. 导航到 `https://www.tiktok.com/@widgetable`
2. 获取页面文本，提取最新视频的评论内容
3. **注意**：TikTok 搜索功能需要登录。如未登录，只从官方账号公开页面采集评论
4. 用 `browser_scroll` 加载更多评论，然后用 `browser_get_text` 提取

#### App Store
1. 用 `mcp__firecrawl__firecrawl_scrape` 或 `mcp__agent-browser` 抓取 App Store 页面评论：
   - 美区: `https://apps.apple.com/app/widgetable-besties-couples/id1641107226`
2. 用 `mcp__exa__web_search_exa` 搜索 JustUseApp 等第三方评论聚合平台：
   - 搜索 `site:justuseapp.com Widgetable reviews`
3. 抓取搜到的具体帖子内容

#### Instagram（@widgetableapp）
1. 优先用 `mcp__exa__web_search_exa` 搜索 Instagram 上的 Widgetable 公开内容
2. 如用户已登录，用 `mcp__agent-browser` 导航 Instagram 采集
3. 如未登录或遇到验证码问题，使用搜索引擎间接采集公开可见的内容概览

#### Discord（Widgetable 官方服务器）
1. 用 `mcp__agent-browser` 导航到 Discord 服务器
2. **Discord 必须登录才能采集**。如果浏览器未登录，提醒用户先登录
3. 登录后浏览 feedback/bug-report/suggestions 频道，提取最近的帖子

### 第三步：结构化与分类

对每条采集到的反馈进行结构化处理：

1. **翻译**：非中文内容翻译为中文摘要
2. **分类**：判断属于 Bug / 功能需求 / 体验槽点 / 好评 / 热点
3. **定级**：根据影响范围和严重性评定 P0-P3
4. **归类功能**：判断涉及 宠物系统 / 小组件 / 社交 / 付费 / 其他
5. **产品建议**：为每条反馈撰写具体的产品优化方向

**严重度判定参考：**
- P0-紧急：核心功能崩溃、数据丢失、安全漏洞
- P1-高：核心功能异常（安装失败、小组件黑屏）、合规风险（不当广告）、阻断新用户获取
- P2-中：非核心功能Bug、用户明确的功能需求、影响体验但有替代方案
- P3-低：微小体验问题、信息不透明、极少数用户反馈

### 第四步：写入飞书多维表格

将每条反馈构造为 JSON 文件，通过 `lark-cli` 写入：

```bash
lark-cli base +record-upsert \
  --base-token PvZdbnjBpamsaaskD3Jc3ki3nqe \
  --table-id tblUyCkgbEL8ljpo \
  --json @record.json
```

**关键注意事项：**
- 必须将 JSON 写入临时文件，用 `@filename.json` 传参（Windows 下直接传 JSON 字符串会有转义问题）
- 写入完成后清理临时 JSON 文件
- 每写一条确认返回 `"ok": true`

JSON 示例：
```json
{
  "日期": "2026-04-05 00:00:00",
  "渠道": "App Store",
  "频道/位置": "App Store 评论 (美区)",
  "用户名": "raynoelb",
  "原始内容": "all of my widgets just stopped working...",
  "中文摘要": "所有小组件突然变黑无法恢复",
  "分类": "Bug",
  "相关功能": ["小组件"],
  "严重度": "P1-高",
  "是否真实痛点": "是",
  "是否可做需求": "是",
  "产品启发及优化方向": "紧急排查小组件渲染崩溃根因"
}
```

### 第五步：生成周报文档

采集和写入完成后，生成一份结构化周报。参考 `references/report_template.md` 中的模板格式。

1. 将周报内容写入本地 markdown 文件
2. 用 `lark-cli docs +create` 创建飞书文档：

```bash
lark-cli docs +create --markdown "$(cat weekly_report.md)" --title "Widgetable 用户反馈周报（起始日期-结束日期）"
```

3. 返回文档链接给用户

### 第六步：汇总展示

向用户展示本次执行结果：
- 各渠道采集条数
- 飞书 Base 链接
- 周报文档链接
- 本周发现的 P0/P1 紧急问题列表
- 下周建议关注的重点

## 已知限制与应对

| 问题 | 应对方案 |
|------|---------|
| Discord/TikTok/Instagram 需要登录 | 提醒用户在浏览器中先登录，登录后再执行采集 |
| TikTok 搜索需登录 | 不登录时只从 @widgetable 官方账号页面采集公开评论 |
| Instagram 手机验证码 | 改用 Exa 搜索 + Firecrawl 抓取公开页面 |
| Reddit 帖子点击后重定向/黑屏 | 从列表页直接提取文本，不进入单个帖子 |
| lark-cli JSON 转义问题 | 始终用临时文件 + `@filename.json` 方式传参 |
| `--base-token` 参数 | 使用 `+record-upsert` 命令（不是 `+record-add`） |

## Widgetable 产品背景

Widgetable 是一款面向海外用户的 iOS 社交小组件应用，核心功能：
- **共养宠物**：好友/情侣一起养虚拟宠物（核心功能）
- **社交小组件**：锁屏/主屏幕放置互动小组件（距离、睡眠、心情等）
- **个性化**：壁纸、主题、植物小组件
- **用户群体**：以13-25岁年轻女性为主，大量西语地区用户
