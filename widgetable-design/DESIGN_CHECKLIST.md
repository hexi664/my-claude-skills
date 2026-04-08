# 设计图质量检查清单

## 必须通过

### 结构完整性
- [ ] 每个 PRD 中的页面都有对应 Frame
- [ ] 每个页面的所有状态都有 Frame（默认/空/加载/错误）
- [ ] 弹窗和浮层有独立 Frame
- [ ] 有页面跳转流标注

### 视觉一致性
- [ ] 所有颜色引用 Variables，无写死 HEX
- [ ] 字体使用统一层级（H1/H2/Body/Caption）
- [ ] 间距遵循 spacing 变量系统
- [ ] 圆角同类元素一致
- [ ] 风格与 Widgetable 参考截图匹配（可爱/圆润/暖色）

### Design System 合规
- [ ] 新颜色已加入 Variables
- [ ] 新组件已加入 design-system.pen
- [ ] 所有按钮/卡片/输入框使用 Component Instance
- [ ] 没有脱离 Design System 的"野生"样式

### PRD 对照
- [ ] 每个 ASCII 原型的布局在设计中体现
- [ ] 每个交互说明有对应标注
- [ ] PRD §6 边界情况的状态都有设计
- [ ] 没有超出 PRD 范围的多余设计

### 可开发性
- [ ] Frame 尺寸 390x844（iPhone 14）
- [ ] 使用 Auto Layout 而非绝对定位
- [ ] 层级命名有意义（不是 Frame 42）
- [ ] 安全区域留白（状态栏 47px + Home Indicator 34px）
- [ ] 图片占位符有明确尺寸标注

## 加分项
- [ ] 有深色模式变体
- [ ] 关键动效有说明（过渡方式/时长）
- [ ] 复杂交互有多步骤分解
- [ ] 组件有 hover/pressed/disabled 状态

## 红线
- ❌ 有页面只有文字描述没有 Frame
- ❌ 颜色全部写死 HEX 没用 Variables
- ❌ 风格明显偏离 Widgetable（比如用了企业风/暗黑风）
- ❌ Frame 尺寸不是移动端尺寸
