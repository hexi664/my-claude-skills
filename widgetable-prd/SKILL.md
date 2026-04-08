---
name: widgetable-prd
description: "Widgetable PRD 撰写：当需求调研报告已确认，需要产出 PRD 文档时使用。基于调研报告生成结构化 PRD，包含用户故事、ASCII 线框图、数据接口定义、边界情况处理。触发词：写 PRD、出 PRD、产品需求文档、PRD 文档、widgetable prd。"
---

# Widgetable PRD Skill

## 触发条件
当需求调研报告已确认，用户要求产出 PRD 时，使用此 skill。

## 前置条件
**必须先完成需求调研**（widgetable-research skill），产出 `docs/RESEARCH-[功能名].md` 并经老板确认。

## 输入
1. 用户的原始需求描述
2. `docs/RESEARCH-[功能名].md` — 已确认的调研报告（核心输入）

## 输出
`docs/PRD-[功能名].md` — 一份完整的需求文档

## 执行步骤

### Step 1: 读取上下文
```
必须先读取以下文件：
- docs/RESEARCH-[功能名].md               （调研报告 — 最重要的输入！）
- skills/widgetable-prd/PRODUCT_CONTEXT.md  （产品定位、用户画像、功能模块、竞品）
- skills/widgetable-prd/PRD_TEMPLATE.md     （PRD 固定骨架）
- skills/widgetable-prd/CHECKLIST.md        （质量检查清单）
```

从调研报告中提取：
- 结论中的推荐功能清单（Must Have / Nice to Have / Not Now）
- 用户痛点验证结果（痛点强度、频率、替代方案）
- 竞品做得好/差的地方（借鉴 + 避坑）
- 风险与调整建议

### Step 2: 需求范围确认（与用户对话）
基于调研报告的推荐功能清单，与用户确认：
1. Must Have 功能是否全部纳入 Demo？
2. Nice to Have 要不要挑几个加入？
3. 调研中的"意外发现"是否要考虑？
4. Demo 需要做到什么程度？

**如果调研报告的结论已经足够明确，可以跳过此步直接生成。**

### Step 3: 按模板生成 PRD
严格按照 `PRD_TEMPLATE.md` 的结构填写每个章节：

#### 重点章节要求：

**§3 用户故事**：
- 每个故事原子化
- 验收标准必须可直接转化为 Playwright 测试用例
- 示例：`- [ ] AC1.1: 用户点击"领养"按钮后，页面跳转到 /adopt，展示可选宠物列表`

**§4 ASCII 线框图**：
- 线框级（布局框架 + 关键元素 + Tab Bar）
- 不需要具体文案细节
- 必须标注交互说明和状态说明
- 宽度统一 33 字符

**§5 数据与接口**：
- 每个接口必须标注 Demo 处理方式：
  - 🟢 真实接口
  - 🔸 客户端 Mock / 写死
  - 🔴 不在 Demo 范围
- 给出 Mock 数据的 JSON 示例

**§6 边界情况**：
- 必须覆盖五大类：输入边界、网络异常、设备环境、权限、业务边界
- 每种都要有具体的处理方式，不能写"待补充"

### Step 4: 自查
按 `CHECKLIST.md` 逐项检查：
- 必须通过项全部 ✅ → 提交
- 有未通过项 → 修改后重新检查
- 触碰红线 → 必须修改

### Step 5: 输出
1. 保存到 `docs/PRD-[功能名].md`
2. 告诉用户 PRD 已生成，请审阅
3. 列出需要用户决策的开放问题（§10）

## 设计风格提醒
PRD 中涉及视觉描述时，需与 Widgetable 风格一致：
- 可爱、活泼、圆润
- 高饱和暖色系
- 圆角卡片、气泡对话框
- 情感化动效（弹跳/闪烁/心跳）

## 注意事项
- PRD 是给 Claude Code 消费的，格式要 AI-friendly（Markdown、结构化、可引用）
- 验收标准会直接用于 Playwright 测试生成，务必精确
- Demo 阶段所有数据默认 Mock，除非用户特别要求接真实接口
- 范围要克制——这是 Demo 不是完整产品
