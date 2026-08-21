
## 更新：定期轮询机制（2026-08-21）

### 新增功能
- 添加 30 秒定期轮询机制
- Web 和桌面客户端自动刷新会话列表
- 与 BroadcastChannel 形成互补

### 工作原理
| 机制 | 范围 | 触发方式 |
|------|------|----------|
| BroadcastChannel | 同浏览器标签页 | 会话变更时立即广播 |
| storage 事件 | 同浏览器标签页 | localStorage 变化时触发 |
| window focus | 当前标签页 | 窗口获得焦点时刷新 |
| 定期轮询 | 所有客户端 | 每 30 秒自动刷新 |

### 代码变更
```typescript
// packages/client/runtime/src/client/sessions/manager.ts
private readonly _pollTimer: ReturnType<typeof setInterval> | null = null

private setupCrossTabSync(): void {
  // ... 原有代码 ...
  
  // 新增：定期轮询
  const POLL_INTERVAL_MS = 30000
  this._pollTimer = setInterval(() => {
    void this.refreshList()
  }, POLL_INTERVAL_MS)
}
```

### 效果
- ✅ 同浏览器多标签页实时同步
- ✅ Web ↔ 桌面客户端每 30 秒自动同步
- ✅ 窗口聚焦时立即刷新
- ✅ 会话变更时立即广播
