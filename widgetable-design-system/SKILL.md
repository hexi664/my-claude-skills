---
name: widgetable-design-system
description: Widgetable App 设计规范和 Design System。当使用 Pencil.dev 或 Claude Code 为 Widgetable 创建 UI 设计、小组件设计、页面原型时，必须使用此 skill 以确保设计风格一致。触发词包括：Widgetable 设计、小组件设计、widget 设计、创建 pen 文件、Widgetable 页面、Widgetable UI、桌面组件设计。当用户提到 Widgetable 产品相关的任何设计任务时都应触发此 skill。
---

# Widgetable Design System

本 skill 定义了 Widgetable App 的完整设计规范，供 Claude Code 在 Pencil.dev 中生成 .pen 文件时参照使用。

## 设计理念

Widgetable 是一款以**可爱、温暖、社交互动**为核心的 iOS 小组件 App。设计风格强调：
- **柔和圆润**：大圆角、柔和渐变、圆形头像
- **色彩丰富但不刺眼**：每个功能模块有独立主题色，但整体保持和谐
- **插画感强**：大量使用卡通角色、emoji、手绘风格图标
- **社交属性**：好友头像、互动状态、对比数据随处可见
- **轻游戏化**：等级、经验值、徽章、PRO 标签等游戏元素融入 UI

---

## 1. 颜色系统 (Colors)

### 1.1 品牌主色
- Brand Green: `#4CAF50` — 主按钮、PRO 标签、状态指示
- Brand Green Light: `#7BC67E` — 按钮渐变结束色
- Brand Green Gradient: `linear-gradient(180deg, #6ECF72 0%, #4CAF50 100%)` — CTA 按钮

### 1.2 功能模块主题色
每个功能模块有独立的背景色调：
- 宠物模块: `#FFF5E6` (暖黄米色) / 强调色 `#F5A623` (橙色)
- 频率模块: `#FFE4EC` (浅粉) / 强调色 `#FF6B8A` (粉红)
- 状态模块: `#E8E0F0` (淡紫) / 强调色 `#9B7DC8` (紫色)
- 睡眠模块: `#E0F5F5` (浅青) / 强调色 `#4ECDC4` (青绿)
- 花园模块: `#F0F7E6` (浅绿) / 强调色 `#8BC34A` (草绿)
- Pin it 模块: `#E8F8E8` (薄荷绿) / 强调色 `#66BB6A`
- 想你模块: `#FFE4EC` (浅粉) / 强调色 `#FF8A9B`

### 1.3 中性色
- Background: `#F5F5F5` — 页面背景
- Surface White: `#FFFFFF` — 卡片背景
- Surface Light: `#F8FAF8` — 次级背景
- Text Primary: `#1A1A1A` — 主标题
- Text Secondary: `#666666` — 副标题/描述文字
- Text Tertiary: `#999999` — 占位符/提示文字
- Divider: `#F0F0F0` — 分割线
- Tab Bar Inactive: `#BBBBBB` — 未选中 Tab 图标

### 1.4 语义色
- Success: `#4CAF50` (绿)
- Warning: `#FF9800` (橙)
- Error: `#F44336` (红)
- HOT Badge: `#FF4757` — HOT 标签背景
- PRO Badge: `#4CAF50` — PRO 标签背景

### 1.5 小组件专用色
桌面小组件通常使用半透明背景以适配系统壁纸：
- Widget Background: `rgba(255, 255, 255, 0.85)` — 浅色模式
- Widget Background Dark: `rgba(30, 30, 30, 0.85)` — 深色模式
- Widget Text: `#1A1A1A` / `#FFFFFF`
- Widget Accent Pink: `#FF6B8A` — 情侣/社交组件强调色

---

## 2. 字体系统 (Typography)

### 2.1 字体家族
- 中文: `PingFang SC` (iOS 系统字体)
- 英文: `SF Pro Display` / `SF Pro Text`
- 数字/数据: `SF Pro Rounded` (圆润风格，用于计数器、等级等)

### 2.2 字体层级
| 用途 | 字号 | 字重 | 行高 | 示例 |
|------|------|------|------|------|
| 页面大标题 | 28px | Bold (700) | 34px | "动态"、"朋友" |
| Banner 标题 | 24px | Bold (700) | 30px | "Play games and compare scores!" |
| 卡片标题 | 20px | Semibold (600) | 26px | "宠物"、"频率" |
| 卡片副标题 | 14px | Regular (400) | 20px | "和好友一起养只宠物吧" |
| 正文 | 16px | Regular (400) | 22px | 设置项文字 |
| 小标签 | 12px | Medium (500) | 16px | "HOT"、"PRO"、Tab 文字 |
| 数据数字 | 32px | Bold (700) | 38px | 计数器 "0"、天数 "506天" |
| 辅助信息 | 11px | Regular (400) | 14px | 时间戳、版本号 |

---

## 3. 间距系统 (Spacing)

### 3.1 基础间距 (4px Grid)
- xs: `4px` — 图标与文字间距
- sm: `8px` — 紧凑元素间距
- md: `12px` — 列表项内部间距
- base: `16px` — 标准间距、卡片内边距
- lg: `20px` — 区块间距
- xl: `24px` — 大区块分隔
- 2xl: `32px` — 页面边距上下

### 3.2 页面边距
- 页面水平边距: `16px` (左右)
- 卡片内边距: `16px`
- 卡片之间间距: `12px`
- Tab Bar 高度: `83px` (含安全区)
- 状态栏高度: `54px`

---

## 4. 圆角系统 (Border Radius)

- 小元素 (按钮标签、Badge): `8px`
- 中元素 (输入框、小卡片): `12px`
- 大卡片: `16px`
- 功能区域卡片: `20px`
- Banner/Hero 卡片: `24px`
- 头像: `50%` (完整圆形)
- 底部操作按钮 (大圆按钮): `50%`
- iOS 小组件圆角:
  - 小组件 (Small): `22px`
  - 中组件 (Medium): `22px`
  - 大组件 (Large): `22px`

---

## 5. 阴影系统 (Shadows)

- 卡片阴影: `0 2px 8px rgba(0, 0, 0, 0.06)`
- 浮层阴影: `0 4px 16px rgba(0, 0, 0, 0.1)`
- 按钮阴影: `0 2px 4px rgba(76, 175, 80, 0.3)` (绿色按钮)
- 小组件阴影: 无 (iOS 系统自带)

---

## 6. 组件规范 (Components)

### 6.1 导航栏 (Navigation Bar)
- 高度: `44px` (不含状态栏)
- 品牌 Logo 居中显示 (绿底白字 "Widgetable" 胶囊)
- 返回按钮: `<` 箭头, 颜色跟随页面主题
- 右侧操作按钮: 组件选择器图标 / 汉堡菜单

### 6.2 底部 Tab Bar
- 4 个 Tab: 首页(宠物) / 好友 / 动态 / 我的
- 图标尺寸: `24x24px`
- 选中态: 实心图标 + 主题色
- 未选中态: 线性图标 + `#BBBBBB`
- 无文字标签 (纯图标 Tab Bar)

### 6.3 功能入口卡片 (Feature Card)
- 布局: 2 列网格, 间距 `12px`
- 尺寸: 等宽, 高度自适应 (约 180-200px)
- 圆角: `20px`
- 内容: 标题 (20px Bold) + 描述 (14px) + 预览图/插画
- 每个卡片有独立主题色背景

### 6.4 顶部 Banner / Carousel
- 全宽, 高度约 `200px`
- 圆角: `0` (贴边) 或 `24px` (带边距)
- 渐变背景 + 插画
- CTA 按钮: 圆角胶囊按钮, 绿色/橙色
- 分页指示器: 小圆点, 底部居中

### 6.5 快捷入口栏 (Quick Access Bar)
- 水平排列 5 个图标入口
- 图标尺寸: `48x48px` (插画风格)
- 文字标签: `12px`, 居中于图标下方
- 间距: 等分排列

### 6.6 CTA 按钮
- 主按钮: 绿色渐变, 全宽, 高度 `50px`, 圆角 `25px`
- 次要按钮: 白色背景 + 绿色描边
- 橙色按钮 (花园/宠物): `#FF9800` 渐变
- 文字: `16px Semibold`, 白色

### 6.7 用户头像
- 标准头像: `40px` 圆形, 2px 白色描边
- 好友头像组: 重叠排列, 偏移 `-8px`
- 大头像 (个人主页): `80px` 圆形

### 6.8 Badge / 标签
- HOT: 红色背景 `#FF4757`, 白色文字, 圆角 `8px`, 右上角偏移
- PRO: 绿色背景, 白色文字, 圆角 `6px`
- NEW: 橙色背景, 白色文字
- 等级标签: `LV.0` 绿色/橙色渐变底

### 6.9 设置列表
- 白色背景卡片, 圆角 `16px`
- 列表项高度: `52px`
- 左侧图标: `24px`, 灰色/品牌色
- 右侧箭头: `>`, `#CCCCCC`
- 分割线: `1px`, `#F0F0F0`, 左侧缩进 `56px`

---

## 7. 桌面小组件设计规范 (Widget Design)

### 7.1 iOS 小组件尺寸
| 类型 | 尺寸 (pt) | 用途 |
|------|----------|------|
| Small | 169×169 | 单一信息展示 (照片、天数) |
| Medium | 360×169 | 双人信息/地图/对比 |
| Large | 360×376 | 详细信息/多元素 |
| Lock Screen Circular | 76×76 | 锁屏圆形组件 |
| Lock Screen Rectangular | 172×76 | 锁屏矩形组件 |
| Lock Screen Inline | — | 锁屏行内文字 |

### 7.2 小组件设计原则
- **信息聚焦**: 每个组件只展示 1-2 个核心信息
- **大字号**: 关键数据用 `28-36px Bold`
- **高对比度**: 确保在各种壁纸上可读
- **圆角统一**: 内部元素圆角 = 外部圆角 - 内边距
- **无交互元素**: 小组件只能整体点击，不放按钮
- **照片组件**: 照片铺满 + 半透明渐变遮罩 + 底部文字

### 7.3 小组件常见布局
- **情侣地图组件 (Medium)**: 地图背景 + 两个圆形头像 + 中间心形 + 底部城市名
- **照片组件 (Small/Medium)**: 照片铺满 + 左下角小头像 + 右上角心形
- **天数计数器 (Medium)**: 照片背景 + 大号天数 "506天" + 描述文字
- **宠物组件**: 卡通宠物居中 + 状态信息
- **睡眠对比组件**: 两个环形进度 + VS + 分数对比

---

## 8. 页面模板 (Page Templates)

### 8.1 首页 (Home)
```
┌─────────────────────────┐
│ Status Bar              │
│ [Logo: Widgetable]      │
├─────────────────────────┤
│ ┌─────────────────────┐ │
│ │  Banner Carousel     │ │
│ │  (渐变背景+插画+CTA) │ │
│ └─────────────────────┘ │
│ ┌─┐ ┌─┐ ┌─┐ ┌─┐ ┌─┐   │
│ │商│ │P │ │宠│ │游│ │花│ │
│ │店│ │ro│ │物│ │戏│ │园│ │
│ └─┘ └─┘ └─┘ └─┘ └─┘   │
│ ┌──────┐ ┌──────┐      │
│ │ 宠物  │ │ 频率  │      │
│ │ Card │ │ Card │      │
│ └──────┘ └──────┘      │
│ ┌──────┐ ┌──────┐      │
│ │ 状态  │ │ 睡眠  │      │
│ │ Card │ │ Card │      │
│ └──────┘ └──────┘      │
├─────────────────────────┤
│ [Tab Bar: 4 icons]      │
└─────────────────────────┘
```

### 8.2 宠物详情页
```
┌─────────────────────────┐
│ ← Froggie          ··· │
├─────────────────────────┤
│ ┌─────────────────────┐ │
│ │   房间场景 (全宽)     │ │
│ │   宠物角色居中        │ │
│ │   对话气泡           │ │
│ │ [LV] [经验条]        │ │
│ └─────────────────────┘ │
│ [喂食] [清洁] [便便] [社交]│
├─────────────────────────┤
│ ┌──────┐┌──────┐┌────┐ │
│ │ 道具1 ││ 道具2 ││道具3│ │
│ │ 卡片  ││ 卡片  ││卡片 │ │
│ └──────┘└──────┘└────┘ │
└─────────────────────────┘
```

### 8.3 设置/个人页
```
┌─────────────────────────┐
│ [头像] [用户名]      >  │
│ [邀请码]                │
├─────────────────────────┤
│ [PRO 订阅卡片 - 绿色]    │
├─────────────────────────┤
│ 推荐                    │
│ ┌─┐ ┌─┐ ┌─┐ ┌─┐       │
│ │商│ │宠│ │花│ │礼│      │
│ └─┘ └─┘ └─┘ └─┘       │
├─────────────────────────┤
│ 设置                    │
│ [通知]              >   │
│ [单位]              >   │
│ [语言]              >   │
│ [授权管理]           >   │
└─────────────────────────┘
```

---

## 9. 在 Pencil.dev 中使用

### 9.1 创建 Variables
在 Pencil.dev 中使用此设计系统时，先通过 Cmd+K 创建 Variables：

```
Read the design tokens from this skill and create Pencil variables for:
- All colors (brand, module themes, neutrals, semantics)
- Spacing scale (xs through 2xl)
- Border radius scale
- Typography sizes
```

### 9.2 生成 App 页面
```
Create a new Widgetable screen for [功能名].
Use the Widgetable design system variables.
Follow the page template structure from the skill.
Use rounded, cute illustration style for decorative elements.
Background should use the [模块名] theme color.
```

### 9.3 生成桌面小组件
```
Create a [Small/Medium/Large] iOS widget design for Widgetable.
Widget type: [照片/计数器/宠物/睡眠/地图]
Use semi-transparent white background.
Follow iOS widget size: [具体尺寸]pt.
Corner radius: 22px.
Key data should be [28-36px Bold].
```

### 9.4 设计审查清单
生成设计后，检查以下项目：
- [ ] 颜色是否使用了 Variables 而非硬编码
- [ ] 圆角是否符合规范 (卡片 16-20px, 按钮 25px, 头像 50%)
- [ ] 字号层级是否正确 (标题 > 正文 > 辅助)
- [ ] 间距是否遵循 4px 网格
- [ ] 小组件尺寸是否符合 iOS 规范
- [ ] 是否保持了可爱/温暖的整体风格
- [ ] 功能模块是否使用了对应的主题色
