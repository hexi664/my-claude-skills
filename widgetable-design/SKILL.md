---
name: widgetable-design
description: "Widgetable PRD 转设计图：当 PRD 文档已确认，需要产出 Pencil.dev 设计图时使用。读取 PRD 中的 ASCII 线框图和交互说明，通过 Pencil MCP 生成 .pen 设计文件，包含所有页面状态和 Design System 组件。触发词：出设计图、画设计、做设计、design、设计图、pen 文件。"
---

# Widgetable PRD → 设计图 Skill

## 触发条件
当 PRD 文档（docs/PRD-[功能名].md）已经过老板确认，要求产出设计图时触发。

## 前置条件
1. `docs/PRD-[功能名].md` — 已确认的 PRD（含 ASCII 原型、用户故事、交互说明）
2. `docs/RESEARCH-[功能名].md` — 调研报告（竞品截图/参考）
3. Pencil 桌面版已启动，MCP 连接正常（Claude Code 中 /mcp 可见 Pencil）
4. Design System 文件已就绪（见 Step 1）

## 输入
- PRD 文档（核心输入）
- Widgetable Design System（design-system.pen 或 tokens）
- Widgetable 参考截图（放在 design/references/ 目录）

## 输出
- `design/[功能名].pen` — Pencil 设计文件
- 每个页面一个 Frame（移动端 390x844）
- 包含所有状态：默认态、空状态、加载态、错误态

---

## 执行步骤

### Step 1: 准备 Design System（首次使用需要，后续复用）

> 这一步只需做一次，做完后所有新功能都复用同一套 Design System。

#### 1a. 收集 Widgetable 参考截图
```
在 design/references/ 目录放入 Widgetable App 截图（3-5张核心页面）：
- 首页/发现页（看整体布局和导航）
- 某个小组件详情页（看卡片、按钮、间距风格）
- 设置/个人页（看表单、列表风格）
- 弹窗/浮层（看弹窗风格）
- 小组件在锁屏的效果（看小组件视觉）
```

#### 1b. 从截图提取 Design Tokens
方式一：让 Claude 视觉分析截图，产出 tokens：
```
分析 design/references/ 下的所有截图，提取：
1. 色彩系统（primary/secondary/background/surface/text/accent）
2. 字体系统（标题/正文/说明/按钮 的字号和字重）
3. 间距系统（内边距/外边距/组件间距的规律）
4. 圆角系统（卡片/按钮/输入框/头像的圆角值）
5. 阴影系统（卡片投影参数）
保存到 design/tokens.md
```

方式二：如果已有 tokens.md，跳过。

#### 1c. 在 Pencil 中建立 Design System
通过 Claude Code + Pencil MCP 执行：
```
读取 design/tokens.md，在 design/design-system.pen 中创建：
1. Variables（颜色/字体/间距/圆角 全部设为变量）
2. 基础 Components（按钮/卡片/输入框/导航栏/Tab Bar/弹窗）
3. 每个 Component 包含变体（primary/secondary/disabled/hover）
4. 设置亮色/暗色两套主题列（如果需要）
```

验证：
```
通过 Pencil MCP get_screenshot 截取 design-system.pen 的预览
与 design/references/ 中的截图人工对比
不一致则调整 → 重新验证
```

---

### Step 2: 读取 PRD 并规划设计任务

```
读取 docs/PRD-[功能名].md，提取：
1. 所有页面列表（从 §4 ASCII 线框图章节）
2. 每个页面的：
   - 布局结构（从 ASCII 图）
   - 内容元素（文案/图标/图片占位）
   - 交互说明（点击/滑动/长按）
   - 状态列表（默认/空/加载/错误）
3. 页面间的导航关系
4. 需要的组件（哪些可复用 design-system 里的，哪些需要新建）
```

产出设计任务清单：
```markdown
## 设计任务清单

### 页面（每个页面一个 Frame）
- [ ] Page 1: [页面名] — 390x844 — 状态：默认/空/加载/错误
- [ ] Page 2: [页面名] — 390x844 — 状态：默认/空
- [ ] ...

### 新组件（design-system 里没有的）
- [ ] Component A: [描述]
- [ ] Component B: [描述]

### 弹窗/浮层
- [ ] Modal A: [描述]
- [ ] Bottom Sheet B: [描述]
```

---

### Step 3: 在 Pencil 中生成设计图

通过 Claude Code + Pencil MCP 执行，按以下顺序：

#### 3a. 创建新组件（如果有）
```
在 design/design-system.pen 中，使用现有 Variables 创建新组件：
- [Component A]：[基于 PRD 描述和 Widgetable 风格创建]
- [Component B]：...
使用 get_variables 确保引用了正确的设计变量，不要写死值
```

#### 3b. 逐页面生成设计
对每个页面执行：
```
在 design/[功能名].pen 中创建 Frame "[页面名]"（390x844）

参考：
- PRD §4 中的 ASCII 线框图（布局结构）
- PRD §4 中的交互说明（标注交互热区）
- design/references/ 中的截图（视觉风格）
- design/design-system.pen 中的 Variables 和 Components

要求：
1. 使用 Variables 引用颜色/字体/间距，不写死值
2. 复用 design-system 中的 Components
3. 移动端布局，注意安全区域（顶部状态栏 + 底部 Home Indicator）
4. 图片用灰色占位符 + 图标标注
5. 文案用 PRD 中的内容，没有指定的用合理占位
```

#### 3c. 生成状态变体
对需要多状态的页面：
```
为 [页面名] 创建额外 Frames：
- [页面名] - Empty State（空数据时的引导页）
- [页面名] - Loading（骨架屏或加载动画占位）
- [页面名] - Error（网络错误/服务端错误）
每个状态参考 PRD §4 和 §6 的说明
```

#### 3d. 标注交互流
```
创建一个 Flow Frame，用箭头标注页面间的跳转关系：
Page 1 → [点击XX] → Page 2
Page 2 → [点击返回] → Page 1
Page 2 → [点击YY] → Modal A
```

---

### Step 4: 设计自查

#### 视觉一致性检查
```
对每个 Frame 执行 get_screenshot，逐一检查：
- [ ] 颜色是否全部引用 Variables（没有写死的 HEX 值）
- [ ] 字体层级是否一致（标题/正文/说明不混用）
- [ ] 间距是否遵循 spacing 变量（不是随意数值）
- [ ] 圆角是否一致（同类元素圆角相同）
- [ ] 整体视觉是否与 Widgetable 参考截图风格匹配
```

#### PRD 对照检查
```
对照 PRD 逐项验证：
- [ ] 每个 ASCII 原型都有对应的 Frame
- [ ] 每个交互说明都在设计中体现
- [ ] 每个状态（空/加载/错误）都有对应 Frame
- [ ] PRD 中提到的所有 UI 元素都在设计中
- [ ] 没有 PRD 中没提到的多余元素（防止范围蔓延）
```

#### 可开发性检查
```
确保设计对开发友好：
- [ ] 所有 Frame 使用 Auto Layout（不用绝对定位）
- [ ] 组件命名有意义（不是 Frame 42）
- [ ] 层级结构清晰（页面 > 区域 > 组件 > 元素）
- [ ] 可复用元素都是 Component Instance
```

---

### Step 5: 交付

1. 设计文件：`design/[功能名].pen`
2. 通知老板：列出所有页面的截图预览
3. 等待确认：老板审核视觉和交互
4. 如有修改意见 → 回到 Step 3 对应页面调整

---

## Design System 维护规则

- **新颜色**：不直接用新 HEX，先加到 Variables 再引用
- **新组件**：先加到 design-system.pen，再在功能设计中使用
- **修改 Token**：改 Variables 面板，自动全局更新
- **跨功能复用**：所有功能设计都引用同一个 design-system.pen 的 Variables

## 与下一步（代码生成）的衔接

设计图确认后，下一步 skill 会：
1. 通过 Pencil MCP `batch_get` 读取 .pen 文件结构
2. 通过 `get_variables` 提取 tokens → 生成 globals.css
3. 通过 `get_screenshot` 获取每个 Frame 截图作为视觉参考
4. 基于以上信息生成 Next.js 代码

因此设计图的**命名规范**和**层级结构**直接影响代码生成质量。

## Pencil MCP 常用操作速查

| 操作 | MCP 工具 | 说明 |
|------|----------|------|
| 创建/修改元素 | batch_design | 插入、复制、更新、移动、删除节点 |
| 读取结构 | batch_get | 读取组件层级、搜索元素 |
| 截图预览 | get_screenshot | 渲染 Frame 预览图 |
| 分析布局 | snapshot_layout | 检查定位问题、重叠检测 |
| 读取变量 | get_variables | 读取 design tokens |
| 设置变量 | set_variables | 创建/更新 design tokens |
| 编辑器状态 | get_editor_state | 当前选中元素、活动文件 |
