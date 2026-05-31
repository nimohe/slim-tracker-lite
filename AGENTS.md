# SPA（减肥记录助手）

单文件 vanilla JS SPA，浏览器直接打开 `index.html`，无需构建/dev server。

## 架构

- **单入口**：所有 HTML/CSS/JS 在 `index.html`（~1200 行）
- **路由**：`location.hash` → `hashchange` 事件驱动渲染。默认路由为 `#profile`（非首页）
- **事件委托**：所有 `click`/`change`/`blur` 通过 `#app` 元素委托处理（`blur` 使用 capture phase）
- **CDN 依赖**：Tailwind CSS（inline config 仅扩展字体）、Chart.js v4.4.1 UMD、Google Fonts DM Mono——首次加载需网络
- **移动优先**：`max-w-md` 容器 + `pb-safe`/`viewport-fit=cover` 适配刘海屏
- **UI 组件**：Toast、确认弹窗、运动打卡弹窗——均为 DOM 原生实现
- **版本**：UI 底部显示 `ver 1.0.0`

## 持久化（localStorage）

| Key | 内容 |
|---|---|
| `diet_tracker_spa_v2` | 主数据：user / weights / sports |
| `diet_tracker_sport_types_v2` | 运动类型模板数组，默认 `['壶铃','自由训练','散步','跑步','爬楼','有氧操']` |
| `diet_tracker_sport_colors_v1` | 运动类型→颜色索引映射 |

数据模型：
```
{
  user: { height, targetWeight, nickname, avatar, gender },
  weights: [{ id, value, date, time, timestamp }],
  sports: [{ id, duration, types[], date, time, timestamp }]
}
```

## 功能要点

- **3 个视图**：首页（体重录入 + 历史列表）、分析（折线图 + 堆叠柱状图）、我的（BMI 仪表盘 + 设置）
- **历史列表**：全部/称重/运动三栏筛选，日期分组折叠，每次加载 30 条（`displayLimit += 30`），左滑 45px 触发删除
- **图表**：Chart.js，周期筛选（近一周/一月/全部），切换视图自动 `destroy()` 再重建，导出为 800×1200 合成 PNG
- **运动打卡弹窗**：预设时长 10/15/30/45/60 分 + 自定义，编辑模式可增删类型（`DEFAULT_TYPES` 不可删除）
- **数据管理**：JSON 导出/导入（覆盖式）、清除数据为**嵌套两步确认**（警告 → 最终确认）
- **PWA**：manifest 由 JS 动态生成 canvas 图标，`sw.js` 引用但不存在（`catch(()=>{})` 静默忽略）
- **离线提示**：`navigator.onLine` 事件驱动顶部横幅

## 注意事项

- `db.clear()` 额外清理 LiveServer 的 `IsThisFirstTime_Log_From_LiveServer` key
- 头像存 base64 data URL，单张限制 500KB
- 无测试、无 linter、无代码生成
