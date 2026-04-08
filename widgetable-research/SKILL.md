---
name: widgetable-research
description: "Widgetable 需求调研：当用户提出 Widgetable 功能点子时，在写 PRD 之前先执行深度调研。从 App Store、Reddit、TikTok、Instagram 采集用户声音，分析竞品功能，验证痛点真实性，输出结构化调研报告。触发词：调研、research、需求调研、做个调研、分析一下这个方向。"
---

# Widgetable 需求调研 Skill

## 触发条件
当用户提出一个 Widgetable 功能点子/方向时，在写 PRD 之前先执行此 skill 做深度调研。

## 输入
用户的模糊需求描述（一两句话的想法）

## 输出
`docs/RESEARCH-[功能名].md` — 需求调研报告

## 调研范围

### 1. 用户真实反馈采集
从以下渠道采集与该需求方向相关的用户声音：

**App Store / Google Play 评论**
- 搜索 Widgetable 的用户评论中与该方向相关的反馈
- 关注负面评论中的功能缺失抱怨
- 关注正面评论中的功能期望
- 工具：web_search + web_fetch 抓 App Store/Google Play 评论分析

**Reddit**
- r/Widgetable（2.3K 订阅者）
- r/iOSwidgets、r/iOS、r/androidapps
- r/LongDistance、r/LDR（异地恋群体）
- 搜索关键词：功能名 + Widgetable / widget / couples app
- 工具：web_search "site:reddit.com" + web_fetch

**TikTok / Instagram**
- 搜索 #widgetable 相关内容下的评论和需求讨论
- 工具：web_search

**竞品的用户反馈**
- 搜索竞品（Locket、BondBee、Between 等）中是否已有类似功能
- 如果有，看用户对该功能的评价（好评/差评/改进建议）
- 工具：web_search + web_fetch

### 2. 竞品功能分析
调研所有可能做了类似功能的产品：

**直接竞品**（社交小组件类）
- Locket Widget
- BondBee
- Widgetsmith
- WidgetClub
- Couple Widget

**间接竞品**（社交/情侣/闺蜜类）
- Between
- Paired
- Love Nudge
- Zenly（已关闭但有参考价值）

**跨界参考**（其他 App 中的类似功能）
- 微信/LINE/KakaoTalk 的相关功能
- Snapchat 的社交功能
- Instagram Close Friends

对每个竞品分析：
- 是否有类似功能？
- 具体怎么做的？（截图/描述）
- 用户评价如何？
- 有什么做得好/做得差的地方？

### 3. 用户痛点验证
基于采集到的数据，回答以下问题：

- 这个需求是**真实痛点**还是**伪需求**？
- 有多少用户提到过类似需求？（量级感知）
- 用户目前是怎么绕过这个问题的？（现有替代方案）
- 这个需求在 Widgetable 的核心用户群（13-25岁/Z世代/女性/异地恋）中是否有共鸣？

### 4. 可做性分析
从以下维度评估：

**用户价值**（1-5分）
- 解决的痛点有多痛？
- 受益用户群有多大？
- 使用频率预估？

**竞争优势**（1-5分）
- 竞品是否已做？做得如何？
- Widgetable 做这个有什么独特优势？
- 能否形成差异化？

**技术可行性**（1-5分）
- Demo 阶段实现难度？
- 是否依赖特殊 API/权限？
- 是否需要后端支持？

**商业价值**（1-5分）
- 对留存/拉新/付费的影响？
- 是否可以设计付费点？
- 与 Widgetable 现有盈利模式是否匹配？

**综合评分** = 四项平均分
- 4-5分：强烈推荐做
- 3-4分：建议做，但需注意风险
- 2-3分：谨慎考虑，可能需要调整方向
- 1-2分：不建议做，建议换方向

### 5. 功能建议
基于调研结论，给出：

- **推荐做的核心功能**（Must Have）— 解决最核心痛点的最小功能集
- **可以做的增强功能**（Nice to Have）— 锦上添花但不影响核心价值
- **建议不做的功能**（Not Now）— 调研发现用户不需要或竞品已证明行不通的
- **意外发现**（Surprise）— 调研中发现的、用户没有直接提但值得考虑的方向

## 执行步骤

### Step 1: 解析需求方向
从用户的模糊描述中提取：
- 核心关键词（用于搜索）
- 所属功能域（社交互动/养成/美化/新方向）
- 目标用户群假设

### Step 2: 并行调研（spawn 子任务提高效率）
同时进行：
- 路线A：用户反馈采集（App Store + Reddit + 社交媒体）
- 路线B：竞品功能分析（直接竞品 + 间接竞品 + 跨界参考）

搜索关键词组合策略：
```
[功能关键词] + Widgetable
[功能关键词] + widget app + couples / besties / LDR
[功能关键词] + [竞品名] + review / feature / request
[功能关键词] + iOS widget + request / wish / need
```

### Step 3: 数据整理与分析
- 汇总所有采集到的用户声音
- 去重、分类、标注情感倾向（正面/负面/中性）
- 统计提及频率，识别高频痛点

### Step 4: 写调研报告
按 RESEARCH_TEMPLATE.md 格式输出

### Step 5: 交付
1. 保存到 `docs/RESEARCH-[功能名].md`
2. 给出明确结论：建议做 / 调整后做 / 不建议做
3. 如果建议做，列出推荐的功能清单供下一步 PRD 使用

## 注意事项
- 调研要有**数据支撑**，不能空谈。每个结论都要附来源链接或引用
- 用户原声很重要——直接引用用户的原话比总结更有说服力
- 竞品分析要客观——不为了证明"应该做"而选择性忽略负面信息
- 调研报告是给老板（和稀）看的，语言要简洁直接，结论先行
