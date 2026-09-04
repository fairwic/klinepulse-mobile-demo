# KlinePulse 移动端 H5 设计规范

> 本规范从 `index.html` 原型中提取，所有配色均已通过 WCAG AA 对比度验证。
> 后续生成新页面/组件时，直接复用本文档的 token、类名和交互约定，即可保持风格统一。

产品定位：面向加密量化交易用户的 operator-grade 控制台。视觉基调「暗金操作台」——碳灰黑底 + 稀缺琥珀金强调（**金色越少越贵**，只用在最关键的动作、焦点、数字上）。

---

## 目录

1. [配色 Token](#1-配色-token)
2. [排版规范](#2-排版规范)
3. [间距与圆角](#3-间距与圆角)
4. [组件库](#4-组件库)
5. [交互系统](#5-交互系统)
6. [涨跌与状态语义](#6-涨跌与状态语义)
7. [命名与代码约定](#7-命名与代码约定)
8. [无障碍红线](#8-无障碍红线)

## 1. 配色 Token

所有颜色**必须走 CSS 变量**，不允许在组件里写死 hex（除非该元素背景恒定与主题无关，如 toast 深底、彩色徽章上的文字）。深浅两套主题通过 `<html data-theme="dark|light">` 切换。

### 深色（默认 `:root`）

| 变量 | 值 | 用途 |
|---|---|---|
| `--bg-page` | `#060606` | 页面底 |
| `--bg-surface` | `#121212` | 卡片面 |
| `--bg-subtle` | `#171614` | 次级块底（KPI/输入框） |
| `--bg-elevated` | `#1c1b18` | 抬升块 |
| `--text-primary` | `#f6f4f0` | 主文本 |
| `--text-secondary` | `#c9c3b8` | 次文本 |
| `--text-muted` | `#a19b90` | 弱化文本/说明 |
| `--border-subtle` | `rgba(246,244,240,.10)` | 常规描边 |
| `--border-strong` | `rgba(232,180,92,.42)` | 强调描边 |
| `--accent` | `#e8b45c` | 琥珀金（唯一稀缺强调） |
| `--accent-strong` | `#f2c879` | 亮金（焦点/高亮文字） |
| `--accent-dim` | `rgba(232,180,92,.14)` | 金色底纹 |
| `--status-success` | `#63d69a` | 涨/多/就绪 |
| `--status-warning` | `#e8b45c` | 注意/待处理 |
| `--status-danger` | `#f0645b` | 跌/空/阻塞 |
| `--status-info` | `#7fb2c9` | 冷青/中性信息 |
| `--btn-primary-bg` | `#e8b45c` | 主按钮底 |
| `--btn-primary-text` | `#1a1206` | 主按钮字（深字） |

### 浅色（`html[data-theme="light"]`，仅覆盖配色 token）

| 变量 | 值 | 说明 |
|---|---|---|
| `--bg-page` | `#f5f3ee` | 米白底 |
| `--bg-surface` | `#ffffff` | 卡片面 |
| `--bg-subtle` | `#f0ede6` / `--bg-elevated` `#eae6dc` | |
| `--text-primary` | `#26221c` | 15.8:1 |
| `--text-secondary` | `#5c5648` | 7.3:1 |
| `--text-muted` | `#6a6353` | 5.96:1（**不可再浅**，浅了不达标） |
| `--accent` | `#9a6a12` | 深金，浅底上作正文仍达 4.73:1 |
| `--accent-strong` | `#7d5410` | |
| `--status-success` | `#1a8f57` | |
| `--status-danger` | `#d23f38` | |
| `--status-info` | `#2f7d9e` | |
| `--btn-primary-bg` | `#e8b45c`（不变） | 金底深字，两主题一致 |

> 新增颜色前先跑对比度：正文/图标 ≥ 4.5:1，大字/非文本 ≥ 3:1。达不到就调，不要将就。

---

## 2. 排版规范

字体栈：`'Manrope', 'PingFang SC', 'Hiragino Sans GB', system-ui, sans-serif`。基准 `14px / line-height 1.5`。

| 场景 | 字号 | 字重 | 类/写法 |
|---|---|---|---|
| 超大数字（资产/价格） | 22–32px | 800 | `.num` + inline size |
| 页面/详情标题 | 16–19px | 800 | `.push__title` / inline |
| 区块标题 | 13px | 700 | `.sec__title` |
| 卡片主标题 | 13.5–14.5px | 700–800 | inline |
| 正文 | 13px | 600 | 默认 |
| 次要说明 | 11–12px | 400–600 | `.tiny` + `.muted` |
| KPI 数值 | 15–17px | 800 | `.kpi__value` |
| KPI 标签 | 10.5px | 600 | `.kpi__label` |

**数字必须加 `.num`**（`font-variant-numeric: tabular-nums`）——价格、盈亏、百分比、时间等等宽对齐，避免跳动。

弱化文本组合用 `.tiny.muted`。金色高亮文字用 `color: var(--accent-strong)`。

---

## 3. 间距与圆角

| Token | 值 | 用途 |
|---|---|---|
| `--radius-card` | `16px` | 卡片/详情容器 |
| `--radius-pill` | `999px` | 徽章/chip/圆形按钮 |
| 按钮圆角 | `12px`（`.btn--sm` 为 `10px`） | |
| `--tabbar-h` | `64px` | 底部导航高 |
| `--topbar-h` | `52px` | 顶栏高 |
| `--safe-bottom` | `env(safe-area-inset-bottom)` | iOS 安全区 |

节奏约定：屏内 padding `14px`；卡片内 padding `14px`；区块 `.sec` 间距 `16px`；卡片之间 `10px`；行内元素 gap `8–12px`。**触控目标最小高度 44px**（`.btn` 默认 44、`.btn--sm` 36、`.icon-btn` 34）。

阴影只用 `--shadow-card`；金色焦点用 `--glow-gold`。不叠加多层阴影、不用彩色投影。

## 4. 组件库

> 所有组件已在原型中定义好样式，新页面直接套类名即可，不要重复写 CSS。

### 4.1 卡片 `.card`

信息块的基座。变体：
- `.card--tap` — 可点击（按压缩放 + 描边高亮），配 `data-push`/`data-sheet` 使用
- `.card--focus` — 焦点卡（金色顶边高光 + 辉光），用于 Hero / 重点事件，**一屏最多一个**

```html
<div class="card">…</div>
<div class="card card--tap" data-push="combo-detail" data-ctx='{"name":"…"}'>…</div>
<div class="card card--focus">…</div>
```

### 4.2 区块标题 `.sec`

```html
<div class="sec">
  <div class="sec__head"><span class="sec__title">标题</span><span class="sec__link" data-goto="positions">查看全部 →</span></div>
  <div class="stack">…</div>
</div>
```
`.sec__hint`（右侧灰色说明）、`.sec__link`（右侧金色可点链接）二选一。

### 4.3 状态徽章 `.badge`

**关键无障碍原则：状态不靠纯色**——徽章用「圆点 + 文字 + 形状」共同表达。

| 类 | 语义 |
|---|---|
| `.badge--ok` | 成功/就绪/生效（绿） |
| `.badge--warn` | 注意/待处理（金） |
| `.badge--danger` | 阻塞/失败（红） |
| `.badge--info` | 中性信息（冷青） |
| `.badge--muted` | 默认/次要（灰） |
| `+ .badge--square` | 方形变体，用于「阻塞/需操作」，形状差异强化语义 |

```html
<span class="badge badge--ok">已就绪</span>
<span class="badge badge--danger badge--square">已阻塞</span>
```

### 4.4 按钮 `.btn`

| 类 | 用途 |
|---|---|
| `.btn--primary` | 主操作（金底深字 + 投影），一屏最多一个 |
| `.btn--secondary` | 次操作 |
| `.btn--ghost` | 幽灵/取消/返回 |
| `.btn--danger` | 危险操作（红） |
| `+ .btn--block` | 占满整行 |
| `+ .btn--sm` | 小号（36px） |

```html
<button class="btn btn--primary btn--block" data-sheet="checkout" data-ctx='{"tier":"Pro"}'>选择 Pro</button>
```

### 4.5 数据行 `.row` / `.rows`

左标签右数值的高频布局：
```html
<div class="rows">
  <div class="row"><span class="row__k">24h 成交额</span><span class="row__v num">2.14 亿</span></div>
</div>
```

### 4.6 KPI 网格 `.kpi-grid` / `.kpi`

```html
<div class="kpi-grid">  <!-- 默认 3 列，可 inline 改 repeat(2|4,1fr) -->
  <div class="kpi"><div class="kpi__label">未实现盈亏</div><div class="kpi__value num pnl-pos">+96.4</div></div>
  <div class="kpi kpi--primary">…</div>  <!-- 金色重点态，一组最多一个 -->
</div>
```

### 4.7 chips 横向筛选 `.chips` / `.chip`

横向可滚动，单选高亮（JS 已自动接管 `.is-active` 切换）：
```html
<div class="chips"><button class="chip is-active">全部</button><button class="chip">新币榜</button></div>
```

### 4.8 分段控件 `.seg` / `.seg__btn` + `.seg-panel`

一屏内切换多组内容（如发现屏 信号/情报/策略）。`data-seg` 与 `data-panel` 值对应，JS 自动切换：
```html
<div class="seg" id="xxxSeg">
  <button class="seg__btn is-active" data-seg="a">信号</button>
  <button class="seg__btn" data-seg="b">情报</button>
</div>
<div class="seg-panel is-active" data-panel="a">…</div>
<div class="seg-panel" data-panel="b">…</div>
```

### 4.9 设置行 `.setrow` + 开关 `.sw`

```html
<div class="setrow">
  <div><div class="setrow__k">信号触发</div><div class="setrow__d">说明文字</div></div>
  <label class="sw"><input type="checkbox" checked><span class="sw__t"></span></label>
</div>
```

### 4.10 其他

- **列表项** `.item`（图标 `.item__ico` + `.item__body` + `.item__right`）
- **时间线**：`padding:9px 0` + 左侧圆点 `9px`，圆点色用 status token 表状态
- **迷你走势** `.spark`（窄屏用轻量柱状替代完整 K 线）
- **方向色** `.dir-long`/`.dir-short`（多空）、`.pnl-pos`/`.pnl-neg`（盈亏），带 `↑↓▲▼` 符号前缀
- **空状态** `.empty`（`.empty__ic` + `.empty__title` + `.empty__desc`），无数据时诚实展示，不留白屏

## 5. 交互系统

全站交互靠**声明式 data 属性 + 事件委托**，不给每个元素单独绑 JS。给元素加对应属性即可，无需写事件代码。

| 属性 | 作用 | 示例 |
|---|---|---|
| `data-push="<id>"` | 打开全屏详情页（右滑入） | `data-push="combo-detail"` |
| `data-sheet="<id>"` | 打开底部 Sheet（上滑起） | `data-sheet="checkout"` |
| `data-goto="<屏>"` | 跨底部 tab 跳转 | `data-goto="positions"` |
| `data-toast="<文字>"` | 操作反馈提示 | `data-toast="已保存" data-toast-kind="info"` |
| `data-ctx='{…}'` | 传给页面的 JSON 上下文 | `data-ctx='{"sym":"BTC-USDT"}'` |
| `data-push-back` | 返回上一层 | 详情页返回按钮 |
| `data-sheet-close` | 关闭 Sheet | Sheet 内取消按钮 |

**优先级规则（重要）**：事件委托取「离点击点最近的祖先」分发。所以卡片整体 `data-push`、卡内某个 span 单独 `data-toast`，点 span 只触发 toast、不误触发卡片跳转。

### 新增详情页 / Sheet 的写法

在脚本区用注册表，不写 DOM 拼接以外的逻辑：
```js
PUSH.register('my-page', function (ctx) {
  return { title: '标题', body: '<div class="card">…</div>',
           onMount: function (el) { /* 可选：挂内部事件 */ } };
});
SHEET.register('my-sheet', function (ctx) {
  return { body: '<div class="sheet__title">…</div>…' };
});
```
注册后任意元素加 `data-push="my-page"` 即可打开。**注册的 id 和引用必须一一对应**，否则点击无反应（这是最常见的低级错误）。

### 深链接

`?push=<id>` / `?sheet=<id>` / `#<屏名>` 可直接定位，用于分享和验证。

---

## 6. 涨跌与状态语义（全站唯一口径，不可混用）

- **绿涨红跌**（国际惯例）：涨/多 = `--status-success` 绿，跌/空 = `--status-danger` 红。全站统一，**禁止**某些页面红涨绿跌。
- 盈亏数字：`.pnl-pos`（绿）/`.pnl-neg`（红），带 `+/-` 符号。
- 准备度三态：可交易/就绪 = 绿，需确认/待处理 = 金，已阻塞 = 红方形徽章。
- 金色是**稀缺强调**：只给主操作、焦点卡、一个重点 KPI、logo。滥用金色会让"重要"失去意义。

### operator 语气（文案红线）

- 用「可交易 / 需验证 / 已阻塞 / 待执行 / 执行失败可重试」这类交易决策语言，少用裸技术状态名。
- 安全前提必须显式写明：如「未设止损不下单」「支付确认前不发放权益」「已持仓不会被自动平掉」。
- 不确定/服务不可用时诚实展示"待确认/状态未知"，**绝不把未确认显示成已生效**。

---

## 7. 命名与代码约定

- 类名用 BEM 式短横线：`.block__element`、`.block--modifier`（如 `.sec__title`、`.btn--primary`）。
- 单文件原型，所有样式在 `<style>`、所有逻辑在单个 `<script>`。新增组件样式加在对应注释分区下。
- 列表类内容用 JS 数据数组 + `.map()` 渲染，不手写重复 DOM。
- 图标用内联 SVG sprite（`<symbol id="i-xxx">` + `<use href="#i-xxx">`），**禁止 emoji**（跨机型不一致、显廉价）。新图标统一 24 viewBox、`stroke=currentColor`、`stroke-width 1.7`。
- 金额精度：CNY 用 `¥` 2 位小数；USDT/盈亏带符号 + 千分位。

---

## 8. 无障碍红线（每次新增必须过）

1. **对比度**：正文/图标 ≥ 4.5:1，大字/非文本 ≥ 3:1。深浅两主题都要过。
2. **状态不靠纯色**：颜色 + 图标/形状/文字至少两者结合（徽章圆点、方形变体、符号前缀）。
3. **触控目标 ≥ 44px**。
4. **主题无关元素固定色**：背景恒定的元素（toast 深底、彩色徽章上的字）文字写死，不用会随主题变的 token——否则切主题就看不清。
5. `prefers-reduced-motion` 下关闭动画（已全局处理）。
6. **不留死交互**：可点的元素（光标 pointer / 按钮样式）必须有真实反馈（跳转/Sheet/toast），点了没反应等于 bug。

---

## 附：快速新建一个页面的清单

1. 复制一个同类 `PUSH.register`，改 id 和 body。
2. body 用 `.card` / `.sec` / `.row` / `.kpi` / `.badge` / `.btn` 拼装，套 token 变量。
3. 入口元素加 `data-push="新id"`（+ 可选 `data-ctx`）。
4. 涉及颜色跑一次对比度；涉及状态用徽章+形状。
5. 用 `?push=新id` 深链接在浏览器打开自检；确认深浅主题都正常。



