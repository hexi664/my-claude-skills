---
name: widgetable-code
description: "Widgetable 设计图转代码：当设计图已确认，需要产出 Next.js 前端代码时使用。通过 Pencil MCP 读取 .pen 设计文件，提取 Design Tokens 生成 CSS 变量，逐页面生成 React 组件代码，包含 Mock 数据层和移动端适配。触发词：出代码、写代码、code、开发、前端代码、实现、coding。"
---

# Widgetable 设计图 → Next.js 代码 Skill

## 触发条件
当设计图（.pen 文件）已经过老板确认，要求产出前端代码时触发。

## 前置条件
1. `design/[功能名].pen` — 已确认的设计图
2. `docs/PRD-[功能名].md` — 已确认的 PRD（验收标准用于后续测试）
3. Pencil 桌面版运行中，MCP 可用
4. 项目已初始化（Next.js + TypeScript + Tailwind）

## 输入
- 设计图 .pen 文件（通过 Pencil MCP 读取）
- PRD 文档（数据模型、接口定义、边界情况）
- Design Tokens（颜色/字体/间距/圆角）

## 输出
- 可运行的 Next.js App Router 项目
- 移动端优先响应式代码
- Mock 数据层
- 全局样式（从 design tokens 生成）

---

## 执行步骤

### Step 1: 项目初始化（首次使用）

如果项目不存在，创建：
```bash
npx create-next-app@latest [功能名]-demo --typescript --tailwind --eslint --app --src-dir
cd [功能名]-demo
npx shadcn@latest init
```

项目结构：
```
src/
├── app/
│   ├── layout.tsx          ← 全局布局（移动端 viewport）
│   ├── page.tsx            ← 首页
│   └── [路由名]/
│       └── page.tsx        ← 各页面
├── components/
│   ├── ui/                 ← Shadcn/UI 基础组件
│   └── [功能名]/           ← 本功能专属组件
├── lib/
│   ├── mock-data.ts        ← Mock 数据
│   └── types.ts            ← TypeScript 类型定义
├── styles/
│   └── globals.css         ← 全局样式 + CSS 变量
└── hooks/                  ← 自定义 hooks
```

### Step 2: 从设计图提取并生成全局样式

通过 Pencil MCP 执行：
```
1. get_variables → 读取所有 design tokens
2. 映射为 CSS 变量写入 globals.css
3. 生成 tailwind.config.ts 的 extend 配置
```

globals.css 示例：
```css
:root {
  /* Brand Colors */
  --brand-primary: #2D7A4F;
  --brand-accent-pink: #FF6B8A;
  --brand-accent-orange: #F5A623;

  /* Background */
  --bg-mint: #E8F8EE;
  --bg-cream: #FFF8EB;
  --bg-surface: #FAFAFA;

  /* Text */
  --text-primary: #1A1A1A;
  --text-secondary: #333333;
  --text-tertiary: #888888;

  /* Spacing */
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 12px;
  --space-base: 16px;
  --space-lg: 20px;
  --space-xl: 24px;

  /* Radius */
  --radius-sm: 12px;
  --radius-md: 16px;
  --radius-lg: 20px;
  --radius-xl: 24px;
  --radius-full: 9999px;

  /* Shadow */
  --shadow-card: 0 2px 8px rgba(0,0,0,0.08);
  --shadow-widget: 0 4px 12px rgba(0,0,0,0.1);
}
```

tailwind.config.ts 追加：
```typescript
extend: {
  colors: {
    brand: {
      primary: 'var(--brand-primary)',
      'accent-pink': 'var(--brand-accent-pink)',
      'accent-orange': 'var(--brand-accent-orange)',
    },
    // ... 映射所有 CSS 变量
  },
  borderRadius: {
    sm: 'var(--radius-sm)',
    md: 'var(--radius-md)',
    lg: 'var(--radius-lg)',
    xl: 'var(--radius-xl)',
  },
  fontFamily: {
    rounded: ['"SF Pro Rounded"', '"Inter"', 'system-ui', 'sans-serif'],
  },
}
```

### Step 3: 生成类型定义和 Mock 数据

从 PRD §5（数据模型 + 接口）提取：

```typescript
// src/lib/types.ts
// 从 PRD 的数据模型直接映射

// src/lib/mock-data.ts
// 从 PRD 的 Mock JSON 示例直接复制
// 标注为 🔸 的接口全部用 Mock
```

Mock 数据策略（从 PRD §5.2 读取）：
- 🟢 真实接口 → fetch 调用
- 🔸 Mock 数据 → 从 mock-data.ts 导入
- 🔴 不做 → 不生成代码

### Step 4: 逐页面生成代码

对设计图中的每个 Frame，按顺序：

#### 4a. 读取设计结构
```
通过 Pencil MCP batch_get 读取 Frame 的：
- 节点树（层级结构）
- 每个节点的样式属性（颜色/字体/间距/圆角）
- 布局方式（Auto Layout → Flex）
- 组件实例（Component Instance → 复用组件）

通过 get_screenshot 获取 Frame 截图作为视觉参考
```

#### 4b. 映射设计节点到 React 组件
```
设计层级映射规则：
- Frame → div / section
- Auto Layout (horizontal) → flex-row
- Auto Layout (vertical) → flex-col
- Text → p / h1-h6 / span
- Rectangle → div with bg
- Component Instance → React Component
- Image placeholder → next/image with placeholder
```

#### 4c. 生成页面代码
```
对每个路由页面生成：
1. page.tsx — 页面主体（Server Component 优先）
2. 需要交互的部分抽成 Client Component
3. 引用 globals.css 的 CSS 变量（不写死颜色值）
4. 引用 Shadcn/UI 组件 + 自定义组件
```

#### 4d. 抽取公共组件
```
识别设计图中复用的元素，抽成组件：
- 出现 >= 2 次的 UI 模式 → 独立组件
- 组件文件放 src/components/[功能名]/
- 组件命名与设计图中的 Component 名一致
- 每个组件有 Props 类型定义
```

### Step 5: 移动端适配

#### 5a. 全局布局
```typescript
// src/app/layout.tsx
export const metadata = {
  viewport: {
    width: 'device-width',
    initialScale: 1,
    maximumScale: 1,
    userScalable: false,
  },
}

// 移动端容器
<div className="mx-auto max-w-[390px] min-h-screen bg-white">
  {children}
</div>
```

#### 5b. 状态页面
从 PRD §6 和设计图的状态变体 Frame 生成：
- 空状态组件
- 加载骨架屏组件
- 错误状态组件
- 无网络提示组件

#### 5c. 交互实现
从 PRD §4 的交互说明实现：
- 点击 → onClick handler / Link 跳转
- 滑动 → 使用 swipe 库或 touch 事件
- 长按 → onTouchStart + setTimeout
- 下拉刷新 → 自定义 hook 或库
- 弹窗 → Shadcn Dialog/Sheet

### Step 6: 代码自查

#### 视觉还原检查
```
1. npm run dev 启动项目
2. 用浏览器 DevTools 切到 iPhone 14 视口 (390x844)
3. 截图与设计图 Frame 逐页对比
4. 颜色/字体/间距/圆角是否一致
5. 不一致的地方调整代码
```

#### 代码质量检查
```
- [ ] 无 TypeScript 错误（npm run build 通过）
- [ ] 无 ESLint 警告
- [ ] 所有颜色使用 CSS 变量或 Tailwind token（无硬编码 HEX）
- [ ] 所有组件有 Props 类型定义
- [ ] Mock 数据集中管理（不散落在组件里）
- [ ] 关键交互元素有 data-testid（给 Playwright 用）
- [ ] 图片使用 next/image 组件
- [ ] 无 console.log 残留
```

#### PRD 对照检查
```
- [ ] 每个页面路由与 PRD 页面一一对应
- [ ] 每个交互说明在代码中有实现
- [ ] 每个状态（空/加载/错误）有对应 UI
- [ ] 边界情况处理与 PRD §6 一致
- [ ] data-testid 覆盖所有验收标准涉及的元素
```

### Step 7: 交付

1. 确保 `npm run dev` 正常运行
2. 确保 `npm run build` 无错误
3. 通知老板：Demo 可预览（localhost:3000）
4. 列出已实现的页面和功能清单
5. 列出 data-testid 清单（给下一步测试用）

---

## 关键编码规范

### 命名约定
- 文件名：kebab-case（widget-card.tsx）
- 组件名：PascalCase（WidgetCard）
- CSS 变量：kebab-case（--brand-primary）
- data-testid：kebab-case（miss-you-button）

### 组件结构
```typescript
// 标准组件模板
interface WidgetCardProps {
  title: string
  // ...
}

export function WidgetCard({ title }: WidgetCardProps) {
  return (
    <div
      className="rounded-xl p-4 shadow-card"
      data-testid="widget-card"
    >
      {/* ... */}
    </div>
  )
}
```

### 样式优先级
1. Tailwind utility classes（首选）
2. CSS 变量引用（颜色/间距/圆角）
3. Shadcn/UI 组件默认样式
4. 自定义 CSS（最后手段）

### data-testid 命名规则
```
[页面]-[元素]-[动作（可选）]
示例：
- home-pet-card
- detail-miss-you-button
- mood-jar-add-button
- settings-premium-banner
```
这些 testid 会被 Step 5（测试）的 Playwright 脚本直接引用。

---

## 与前后步骤的衔接

### 从 Step 3（设计图）接收
- .pen 文件中的 Frame 结构 → 页面路由
- Variables → CSS 变量
- Components → React 组件
- 截图 → 视觉还原参考

### 向 Step 5（测试）传递
- data-testid 清单 → Playwright 定位器
- PRD 验收标准 → 测试用例
- 页面路由清单 → 测试导航
- Mock 数据 → 测试数据
