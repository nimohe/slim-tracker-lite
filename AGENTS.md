# SPA（减肥记录助手）

单文件 vanilla JS SPA，无需构建。

## 文件说明

单入口文件 `index.html`（Chart.js CDN 图表、PWA、JSON 导入导出、头像上传、滑动手势删除）

## 快速开始

浏览器直接打开 `index.html`，无需 dev server。

## 架构说明

- **单入口**：所有 HTML/CSS/JS 各自在一个文件中
- **纯 JS**（无框架/路由库）—— SPA 路由通过 hash 切换动态渲染 DOM 实现
- **Tailwind CSS** 通过 CDN 引入
- **Chart.js**：通过 CDN 引入 `chart.umd.min.js`
- **localStorage 持久化**，存储 key：`diet_tracker_spa_v2`
- **3 个视图**：首页（体重录入 + 历史）、分析（图表 + 运动分布）、我的（BMI + 设置）
- **UI 组件**：Toast、确认弹窗、运动打卡弹窗——均为 DOM 原生实现，无第三方 UI 库

## 数据模型

```
{
  user: {
    height: string,        // cm
    targetWeight: string,   // kg
    nickname: string,       // 默认 "减脂达人"
    avatar: string,         // base64 data URL
    gender: number          // 0=未设置 1=男 2=女
  },
  weights: [{ id, value, date, time, timestamp }],
  sports: [{ id, duration, types[], date, time, timestamp }]
}
```

## 功能清单

| 特性 | 说明 |
|---|---|
| 图表引擎 | Chart.js v4.4.1（折线图 + 堆叠柱状图）|
| 图表周期筛选 | ✅ 近一周/一月/全部 |
| 图表报表导出 | ✅ 导出 PNG |
| PWA（manifest + 安装） | ✅（manifest JS 动态生成）|
| 头像上传 | ✅ base64，限制 500KB |
| 昵称/性别 | ✅ |
| JSON 备份/恢复 | ✅ |
| 滑动手势删除 | ✅（左滑 45px 触发）|
| 更多加载 | ✅ 每次 30 条 |
| 历史筛选 | ✅ 全部/称重/运动三栏，日期分组折叠 |
| 运动类型管理 | ✅ localStorage 持久化模板，编辑模式增删 |
| 体重走势 | ✅ 每日多条时间点展示 |
| 运动分布 | ✅ 堆叠色块，按类型着色 |
| BMI 仪表盘 | ✅ 线性轨道卡片式 |
| 离线提示 | ✅ |

## 注意事项

- **`sw.js` 不存在**：引用了但仓库无此文件，`catch(()=>{})` 静默忽略错误——如需 SW 需自行创建
- **头像存储**：base64 data URL 直接存 localStorage，单张限制 500KB，头像过大会撑爆 localStorage（约 5MB 限额）
- **Google Fonts**：引用 DM Mono，首次加载需网络
- **图表 canvas** 切换视图后自动 `destroy()` 再重建，无需手动清理

## 约定

- **语言**：界面为中文（zh-CN）
- **移动优先**：布局使用 `max-w-md` 容器、safe-area 适配底部导航
- **无测试、无 linter、无代码生成**
