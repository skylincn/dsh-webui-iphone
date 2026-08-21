# 设置面板修复报告 - 2026-08-21

## 问题描述

移动端竖屏模式下，点击会话页面顶部栏的设置按钮（⚙️）无反应。
点击鲸鱼图标（🐳）应该打开侧边栏，但意外打开了设置面板。

## 根因分析

### 问题1：z-index 层级冲突
- 设置面板 overlay z-index: 1000
- 移动端侧边栏 pane z-index: 900
- 当侧边栏包含 dialog 时，CSS 规则将侧边栏 z-index 提升到 1100
- 结果：侧边栏 (1100) 覆盖了设置面板 (1000)

### 问题2：设置面板未使用 createPortal（根本原因）
- 设置面板是侧边栏的普通子元素，不是 portal
- 在竖屏模式下，当侧边栏折叠时 `overflow: hidden` 会裁剪掉设置面板
- 设置面板被限制在侧边栏 DOM 内部，无法正确渲染

## 解决方案

### 修改 DSH 源代码

**文件1：`SettingsRoot.module.css`**
```css
/* 修改前 */
.overlay { z-index: 1000; }

/* 修改后 */
.overlay { z-index: 1200; }
```

**文件2：`SettingsRoot.tsx`**
```tsx
// 修改前
{open && (
  <SettingsPanel ... />
)}

// 修改后
{open && createPortal(
  <SettingsPanel ... />,
  document.body,
)}
```

### 效果
- 设置面板现在直接渲染到 `document.body`，不受侧边栏 overflow 影响
- z-index 1200 确保设置面板始终显示在侧边栏 (900/1100) 之上
- 鲸鱼图标点击现在正确打开/关闭侧边栏，不再意外打开设置面板

## 验证

- [x] 竖屏模式：点击设置按钮可以正常打开设置面板
- [x] 竖屏模式：点击鲸鱼图标正确打开/关闭侧边栏
- [x] 横屏模式：设置按钮正常工作
- [x] 设置面板可以正常关闭（点击遮罩、返回按钮、Escape 键）

## 相关提交

- DSH 主仓库：`packages/client/ui-settings-general/src/client/SettingsRoot.tsx`
- DSH 主仓库：`packages/client/ui-settings-general/src/client/SettingsRoot.module.css`
