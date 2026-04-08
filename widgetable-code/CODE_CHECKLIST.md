# 代码质量检查清单

## 必须通过

### 构建
- [ ] `npm run build` 零错误
- [ ] `npm run dev` 正常启动
- [ ] 无 TypeScript 类型错误
- [ ] 无 ESLint 错误

### 视觉还原
- [ ] 每个页面在 iPhone 14 视口 (390x844) 下布局正确
- [ ] 颜色全部使用 CSS 变量 / Tailwind token（无硬编码 HEX）
- [ ] 字体层级与设计图一致（H1/H2/Body/Caption）
- [ ] 间距遵循 token 系统
- [ ] 圆角与设计图一致

### 功能完整
- [ ] 每个 PRD 页面都有对应路由
- [ ] 每个交互说明都有实现（点击/滑动/弹窗）
- [ ] 空状态页面已实现
- [ ] 加载状态（骨架屏）已实现
- [ ] 错误状态已实现

### 代码规范
- [ ] 组件有 Props 类型定义
- [ ] Mock 数据集中在 mock-data.ts
- [ ] 公共组件抽到 components/ 目录
- [ ] 无 console.log 残留
- [ ] 图片使用 next/image

### 可测试性
- [ ] 所有可交互元素有 data-testid
- [ ] data-testid 命名遵循 [页面]-[元素]-[动作] 格式
- [ ] testid 清单已记录在 docs/TESTID.md

### 移动端
- [ ] viewport meta 配置正确（禁止缩放）
- [ ] 容器最大宽度 390px 居中
- [ ] 安全区域留白（顶部状态栏 + 底部 Home Indicator）
- [ ] 触摸目标 >= 44px（Apple HIG 标准）
- [ ] 无横向滚动

### 边界情况（PRD §6）
- [ ] 空输入处理
- [ ] 超长文本截断
- [ ] 重复提交防抖
- [ ] 网络错误友好提示

## 红线
- ❌ `npm run build` 有错误
- ❌ 颜色全部硬编码 HEX
- ❌ 没有 data-testid
- ❌ Mock 数据散落在各组件里
- ❌ 页面在移动端视口下布局崩塌
