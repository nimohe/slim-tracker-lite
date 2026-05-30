# SPA（减肥记录助手）

单文件 vanilla JS SPA，无需构建。

## 文件说明

本仓库包含 **两个独立入口**，功能逐步增强：

- `spa_html.html`（v1）—— 手写 SVG 图表、可拖动 FAB、功能简洁
- `code-mimo.html`（v2）—— Chart.js CDN 图表、PWA、JSON 导入导出、头像上传、滑动手势删除

两者共享相同的架构模式但数据独立存储。

## 快速开始

浏览器直接打开任意 `.html` 文件，无需 dev server。

## 架构说明

- **单入口**：所有 HTML/CSS/JS 各自在一个文件中
- **纯 JS**（无框架/路由库）—— SPA 路由通过显示/隐藏 `<div>` 实现
- **Tailwind CSS** 通过 CDN 引入（`<script src="https://cdn.tailwindcss.com">`）
- **Chart.js**（仅 `code-mimo.html`）：通过 CDN 引入 `chart.umd.min.js`
- **localStorage 持久化**，存储 key：
  - `spa_html.html` → `diet_tracker_vanilla_v1`
  - `code-mimo.html` → `diet_tracker_spa_v2`
- **3 个视图**：首页（体重录入 + 历史）、分析（图表）、我的（BMI + 设置）
- **UI 组件**：Toast、确认弹窗、运动打卡弹窗——均为 DOM 原生实现，无第三方 UI 库

## 数据模型

```
{
  user: {
    height: string,        // cm
    targetWeight: string,   // kg
    // ↓ code-mimo.html 独有 ↓
    nickname: string,       // 默认 "减脂达人"
    avatar: string,         // base64 data URL
    gender: number          // 0=未设置 1=男 2=女
  },
  weights: [{ id, value, date, timestamp }],
  sports: [{ id, duration, types[], date, timestamp }]
}
```

## 版本差异速查

| 特性 | `spa_html.html` | `code-mimo.html` |
|---|---|---|
| 图表引擎 | 手写 SVG | Chart.js v4.4.1（双轴柱线图）|
| 图表周期筛选 | ❌ | ✅ 近一周/一月/全部 |
| 图表报表导出 | ❌ | ✅ 导出 PNG |
| PWA（manifest + 安装） | ❌ | ✅（manifest JS 动态生成）|
| 头像上传 | ❌ | ✅ base64，限制 500KB |
| 昵称/性别 | ❌ | ✅ |
| JSON 备份/恢复 | ❌ | ✅ |
| 滑动手势删除 | ❌ | ✅（左滑 45px 触发）|
| 更多加载 | ❌ | ✅ 每次 30 条 |
| 运动 FAB | 独立可拖动悬浮 | 固定在输入栏旁 |
| 运动项目 | 7 种 | 8 种（含 HIIT）|
| 清空确认 | 单次 | 双层确认 |
| 离线提示 | ❌ | ✅ |
| sw.js | ❌ | 引用了但不存在（静默失败）|

## 注意事项

- **`sw.js` 不存在**：`code-mimo.html` 中注册了 service worker 但仓库无此文件，`catch(()=>{})` 静默忽略错误——如需 SW 需自行创建
- **头像存储**：base64 data URL 直接存 localStorage，单张限制 500KB（代码层限制），头像过大会撑爆 localStorage（约 5MB 限额）
- **v1 与 v2 数据隔离**：两者 localStorage key 不同，切换文件不会读到对方数据
- `code-mimo.html` 引用 Google Fonts（DM Mono），首次加载需网络
- **图表 canvas** 切换视图后自动 `destroy()` 再重建，无需手动清理

## 约定

- **语言**：界面为中文（zh-CN）
- **移动优先**：布局使用 `max-w-md` 容器、safe-area 适配底部导航
- **无测试、无 linter、无代码生成**
