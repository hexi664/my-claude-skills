# 各渠道采集详细指南

两轮验证总结的实战经验，遇到问题时参考此文件。

## Reddit

### 采集路径
1. `https://www.reddit.com/r/Widgetable/new/` — 按时间排序的最新帖
2. `https://www.reddit.com/r/Widgetable/hot/` — 热门帖（可能有高价值旧帖）

### 经验教训
- **不要点击帖子链接进入详情页**，Reddit 经常出现点击后重定向到错误 subreddit 或黑屏覆盖层的情况
- 从列表页用 `browser_get_text` 提取帖子标题和预览文本即可
- 如果确实需要帖子详情，用 `browser_navigate` 直接拼接完整 URL 导航，不要点击
- r/Widgetable 社区较小，每周约 4-5 个帖子，有价值的帖子要仔细甄别
- 关注帖子评论中的讨论，有时评论比帖子本身更有价值

### 高价值帖子特征
- 带截图的 Bug 报告
- 带有详细描述的功能建议
- 多人点赞/评论的帖子
- 涉及宠物系统、小组件、付费模式的讨论

## TikTok

### 采集路径
1. `https://www.tiktok.com/@widgetable` — 官方账号页面
2. 从最新视频中提取评论

### 经验教训
- **TikTok 搜索功能必须登录**，未登录会弹出登录框
- 未登录时只能从官方账号页面采集公开可见的评论
- 官方视频评论中通常有大量真实用户反馈（一个视频可能有 20-50+ 条评论）
- 用 `browser_scroll` 往下滑加载更多评论，然后用 `browser_get_text` 一次性提取
- 如果 `browser_scroll` 返回 0 像素（到底了），改用 `browser_get_text` 获取当前可见内容
- 很多评论是西班牙语，需要翻译
- TikTok 登录时可能跳转到注册页面而不是登录页面，需要切换

### 评论中常见的反馈类型
- 设备兼容性问题（"no es compatible"）
- 安装失败
- 社交匹配需求
- 宠物系统建议

## App Store

### 采集路径
1. `https://apps.apple.com/app/widgetable-besties-couples/id1641107226` — 美区页面
2. JustUseApp 论坛：搜索 `site:justuseapp.com Widgetable`
3. 用 `mcp__firecrawl__firecrawl_scrape` 抓取具体页面

### 经验教训
- App Store 页面直接用 `browser_get_text` 可以获取到最新的精选评论（通常 3-5 条）
- 这些评论质量很高，往往是长文详细反馈
- JustUseApp 论坛有用户自发提交的问题报告，用 Firecrawl 抓取效果好
- App Store 评论涵盖了 Widgetable 的核心痛点（广告、小组件稳定性、付费模式）

### Widgetable 在 JustUseApp 上的常见问题
- Google 账号登录失败
- 锁屏小组件无法编辑/更新
- 聊天功能不可用
- 睡眠小组件不显示
- 数据同步问题

## Instagram

### 采集路径
1. 用 `mcp__exa__web_search_exa` 搜索公开内容
2. 如已登录：`https://www.instagram.com/widgetableapp/`

### 经验教训
- **Instagram 登录最容易出问题**：手机验证码经常收不到
- 替代方案：用 Exa 语义搜索 `Widgetable Instagram feedback` 获取公开可见的帖子概览
- 用 Firecrawl 抓取 Instagram 公开帖子页面（如 `instagram.com/p/xxx`）
- Instagram 上主要是 UGC 内容（用户展示小组件截图），负面反馈相对少
- #widgetable 话题下有大量高互动 Reels

## Discord

### 采集路径
1. 需要先找到 Widgetable 的 Discord 邀请链接
2. 用 `mcp__agent-browser` 导航到服务器

### 经验教训
- **Discord 必须登录才能看到任何内容**
- 如果浏览器未登录，直接告诉用户需要先登录 Discord
- Discord 是 Widgetable 最活跃的反馈渠道，有专门的 feedback/bug-report 频道
- 登录后重点关注：#feedback、#bug-reports、#suggestions 频道
- Cookie 在同一浏览器会话内会保持，跨渠道导航不会丢失登录状态

## 通用注意事项

1. **并行采集**：尽量用 Agent 工具并行启动多个渠道的采集任务
2. **JSON 传参**：Windows 环境下 lark-cli 必须用文件方式传 JSON（`@filename.json`），直接传字符串会有引号转义问题
3. **临时文件管理**：将 JSON 文件写入工作目录，采集完成后统一清理
4. **去重**：写入前可以先用 `+record-list` 查看现有记录，避免重复
5. **渠道优先级**：Reddit > App Store > TikTok > Discord > Instagram（按数据可获取性排序）
