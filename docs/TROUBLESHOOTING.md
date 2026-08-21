# DSH 移动端适配问题与解决方案汇总

## 问题 1：侧边栏"朦胧无内容"

### 现象
点击 🐳 按钮后，看到模糊 backdrop 但侧边栏空白。

### 根因
DSH 应用 CSS 设置 `will-change: transform`，导致 `position: fixed` 渲染异常。

### 解决方案
```css
[data-dsh-mobile-frame]:not([data-sidebar-collapsed]) [data-dsh-mobile-pane="sidebar"] {
  display: block !important;
}
```

---

## 问题 2：头部按钮顺序错误

### 现象
🐳 按钮出现在右侧而非左侧。

### 解决方案
改用 `prepend` 替代 `appendChild`。

---

## 问题 3：设置按钮位置错误

### 现象
⚙️ 设置在 🐳 旁边，而非挨着模式名称。

### 解决方案
将 headerSettingsButton 从 headerActions 拆出，单独插入到模式名称后。

---

## 问题 4：titleCluster 过宽

### 现象
⚙️ 和 📥 之间有约 67px 间隙。

### 解决方案
```css
.I58I4q_titleCluster {
  flex: 0 1 auto !important;
  max-width: 220px !important;
}
```

---

## 经验总结

1. **will-change + position:fixed 陷阱**
   - 需要 `display: block !important` 强制覆盖

2. **CSS 选择器范围**
   - 注意元素是否在 `[data-dsh-mobile-frame]` 内部

3. **Flexbox 布局控制**
   - `flex: 0 1 auto` 可限制元素扩展
