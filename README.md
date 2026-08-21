# dsh-webui-iphone

DSH Web UI iPhone 移动端适配 Skill

## 功能特性

### 会话头部布局
- 🐳 鲸鱼按钮固定在左侧
- 会话名紧凑显示
- 模式名称可点击切换
- ⚙️ 设置按钮紧挨模式名称
- 📥 下载按钮保持原位
- 侧边栏展开/折叠按钮

### 侧边栏修复
- 修复"朦胧无内容"问题

### 设置面板修复 (2026-08-21)
- 修复竖屏模式下点击设置按钮无反应的问题
- 修复点击鲸鱼图标意外打开设置面板的问题
- 根因：设置面板未使用 createPortal，导致被侧边栏 overflow 裁剪
- 解决方案：将设置面板渲染到 document.body，并提升 z-index 到 1200

## 安装

```bash
dsh plugin --profile web add github:skylincn/dsh-webui-iphone#main
```

## 最终布局（390px 宽）

```
x=8:   🐳 Open sidebar (w=28)
x=40:  会话名+模式名称 (w=220)
x=226: ⚙️ 设置
x=264: 📥 Session log
x=352: 侧边栏展开/折叠
```

## 许可证

MIT
