# CTFd-fpclose 主题文档

## 概述

**CTFd-fpclose** 是一个基于 CTFd 核心主题的定制化主题，具有以下特色：

- 🌊 **动态管道背景**（Pipeline Animation）
- ✨ **深邃星空背景**（Starry Night）
- 🔮 **毛玻璃导航栏**（Glassmorphism Navbar）
- 🎨 **深色主题适配**（Dark Theme Optimized）
- 📱 **响应式设计**（Responsive Design）

---

## 主要特性

### 1. Pipeline 动态管道背景

**效果**：在页面顶部 jumbotron 区域显示炫酷的动态管道动画
**技术**：HTML5 Canvas + Simplex Noise
**位置**：所有主要页面的顶部区域

**相关文件**：
- `/static/js/pipeline/pipeline-jumbotron.js` - 动画核心逻辑
- `/static/js/pipeline/util.js` - 工具函数
- `/static/js/pipeline/noise.min.js` - 噪声生成库
- `/static/css/pipeline/pipeline-jumbotron.css` - 样式定义

**配置参数**（在 `pipeline-jumbotron.js` 中）：
```javascript
const pipeCount = 30;           // 管道数量
const baseHue = 180;            // 基础色调（蓝绿色）
const rangeHue = 60;            // 色调范围
const backgroundColor = 'hsla(150,80%,1%,1)';  // 背景色
```

### 2. 星空背景效果

**效果**：页面背景显示深邃的星空夜景，包含50+颗星星和星云效果
**层级**：z-index: -2 和 -1（最底层，不影响页面交互）

**相关文件**：
- `/static/css/starry-background.css` - 星空效果主文件

**特性**：
- 深蓝色渐变背景（从 #1a2a4e 到 #000000）
- 50个不同大小和亮度的星星
- 星云效果叠加
- 闪烁动画（4秒循环）

### 3. 文字颜色适配

**深色背景页面**：
- 标题（h1-h6）：纯白色 `#ffffff`
- 普通文本：浅灰色 `#e0e0e0`
- 链接：亮蓝色 `#0ab5e4`
- 挑战卡片标题：白色
- 挑战分数：亮蓝色

**弹窗内容**：
- 所有文字：深色 `#212529`（保证可读性）
- 链接：标准蓝色 `#0d6efd`
- 按钮：保持原有颜色方案

**相关文件**：
- `/static/css/starry-background.css` - 全局文字颜色
- `/static/css/challenge-card-light.css` - 挑战卡片专用样式

### 4. 模态框（弹窗）优化

**问题修复**：
- ✅ 修复 backdrop 阻挡交互问题
- ✅ 修复弹窗被灰色遮罩覆盖问题
- ✅ 修复弹窗内文字不可见问题

**z-index 层级**：
```
backdrop: 1049 (半透明灰色遮罩，pointer-events: none)
modal: 1050 (弹窗容器)
challenge-window: 1051 (挑战弹窗，最高优先级)
```

### 5. Pipeline 虚化过渡效果

**效果**：Pipeline 背景底部 80px 区域渐变虚化，平滑过渡到内容区域
**实现**：使用 `::after` 伪元素创建渐变遮罩

---

## 文件结构

```
ctfd-fpclose/
├── static/
│   ├── css/
│   │   ├── pipeline/
│   │   │   └── pipeline-jumbotron.css        # Pipeline 样式
│   │   ├── starry-background.css             # 星空背景 + 全局样式
│   │   └── challenge-card-light.css          # 挑战卡片样式
│   ├── js/
│   │   └── pipeline/
│   │       ├── pipeline-jumbotron.js         # Pipeline 动画
│   │       ├── util.js                       # 工具函数
│   │       └── noise.min.js                  # Simplex Noise
│   ├── img/
│   │   └── favicon.ico                       # 网站图标
│   └── assets/                               # 编译后的资源文件
├── templates/
│   ├── base.html                             # 基础模板（加载背景和动画）
│   ├── challenges.html                       # 挑战页面
│   ├── scoreboard.html                       # 记分板
│   ├── teams/
│   │   └── teams.html                        # 团队列表
│   └── users/
│       └── users.html                        # 用户列表
└── THEME_DOCUMENTATION.md                    # 本文档
```

---

## 插件模板覆盖

由于某些页面被插件覆盖，需要在插件模板中添加 `pipeline-jumbotron` 类：

### 已修改的插件模板：

1. **ctfd_tuandui（团队插件）**
   - `/plugins/ctfd_tuandui/templates/teams/teams.html`
   - 修改：`<div class="jumbotron pipeline-jumbotron">`

2. **ctfd_user（用户插件）**
   - `/plugins/ctfd_user/templates/users/users.html`
   - `/plugins/ctfd_user/templates/users/private.html`
   - `/plugins/ctfd_user/templates/settings.html`
   - 修改：`<div class="jumbotron pipeline-jumbotron">`

3. **ctfd-paihangb（排行榜插件）**
   - `/plugins/ctfd-paihangb/templates/scoreboard/custom_scoreboard.html`
   - 修改：`<div class="jumbotron pipeline-jumbotron">`

---

## 主题配置

### 在 base.html 中的加载顺序

```html
<!-- Pipeline Background Animation for Jumbotron -->
<link rel="stylesheet" href="/themes/ctfd-fpclose/static/css/pipeline/pipeline-jumbotron.css">
<link rel="stylesheet" href="/themes/ctfd-fpclose/static/css/starry-background.css">
<link rel="stylesheet" href="/themes/ctfd-fpclose/static/css/challenge-card-light.css">
<script src="/themes/ctfd-fpclose/static/js/pipeline/noise.min.js"></script>
<script src="/themes/ctfd-fpclose/static/js/pipeline/util.js"></script>
<script src="/themes/ctfd-fpclose/static/js/pipeline/pipeline-jumbotron.js"></script>
```

**注意**：
1. CSS 必须在 `</head>` 之前或 `<body>` 开始处加载
2. JavaScript 必须在 `</body>` 之前加载
3. 加载顺序：`noise.min.js` → `util.js` → `pipeline-jumbotron.js`

---

## 自定义配置

### 修改星空密度

编辑 `/static/css/starry-background.css`：

```css
body::after {
  /* 调整 background-size 来改变星星密度 */
  background-size: 
    400px 400px,  /* 减小数值 = 更密集 */
    450px 450px,  /* 增大数值 = 更稀疏 */
    ...
}
```

### 修改 Pipeline 颜色

编辑 `/static/js/pipeline/pipeline-jumbotron.js`：

```javascript
const baseHue = 180;  // 基础色调：0-360
                      // 0=红, 120=绿, 180=青, 240=蓝, 300=紫
const rangeHue = 60;  // 色调变化范围
```

### 修改虚化过渡高度

编辑 `/static/css/pipeline/pipeline-jumbotron.css`：

```css
.pipeline-jumbotron::after {
  height: 80px;  /* 调整虚化区域高度 */
  background: linear-gradient(to bottom, 
    rgba(0, 0, 0, 0) 0%,
    rgba(13, 27, 42, 0.3) 30%,   /* 调整渐变节点 */
    rgba(13, 27, 42, 0.6) 60%,
    rgba(13, 27, 42, 1) 100%
  );
}
```

### 修改文字颜色

编辑 `/static/css/starry-background.css`：

```css
body {
  color: #e0e0e0 !important;  /* 全局文字颜色 */
}

h1, h2, h3, h4, h5, h6 {
  color: #fff !important;     /* 标题颜色 */
}

a {
  color: #0ab5e4 !important;  /* 链接颜色 */
}
```

---

## 响应式设计

### 断点设置

- **小屏幕**（≤768px）：`@media (max-width: 768px)`
  - Pipeline 高度：306px (250px + 56px navbar)
  - 容器内边距：1rem
  - 底部间距：2rem

- **中大屏幕**（≥769px）：`@media (min-width: 769px)`
  - Pipeline 高度：406px (350px + 56px navbar)
  - 容器内边距：2rem
  - 底部间距：3rem

---

## 性能优化

### CSS 性能

1. **使用 fixed 定位**减少重绘：
   ```css
   body::after {
     position: fixed;  /* 固定定位，不随滚动重绘 */
   }
   ```

2. **pointer-events: none**避免事件捕获：
   ```css
   body::after, body::before {
     pointer-events: none;  /* 背景不捕获鼠标事件 */
   }
   ```

3. **will-change 优化**（可选）：
   ```css
   .pipeline-jumbotron canvas {
     will-change: transform;  /* 提示浏览器优化动画 */
   }
   ```

### JavaScript 性能

1. Pipeline 使用 `requestAnimationFrame` 实现流畅动画
2. Canvas 尺寸自适应，避免不必要的重绘
3. 使用 Simplex Noise 算法生成高性能噪声

---

## 浏览器兼容性

### 支持的浏览器

- ✅ Chrome 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ Edge 90+
- ⚠️ IE 11（不支持，建议升级）

### 所需特性

- CSS Grid
- CSS Flexbox
- HTML5 Canvas
- ES6+ JavaScript
- CSS Custom Properties (变量)
- CSS `backdrop-filter`（毛玻璃效果）

---

## 故障排除

### Pipeline 背景不显示

**检查**：
1. 确认 JavaScript 文件加载顺序正确
2. 检查浏览器控制台是否有错误
3. 确认 `jumbotron` 元素有 `pipeline-jumbotron` 类

**解决**：
```html
<div class="jumbotron pipeline-jumbotron">
  <!-- 内容 -->
</div>
```

### 星空背景不显示

**检查**：
1. 确认 `starry-background.css` 已加载
2. 检查 z-index 是否被其他元素覆盖
3. 强制刷新浏览器（Ctrl+Shift+R）

### 弹窗文字看不清

**检查**：
1. 确认 modal 相关样式已加载
2. 检查是否有其他插件样式冲突
3. 检查浏览器开发工具中的 computed styles

**解决**：
在 `starry-background.css` 中已有完整的 modal 样式覆盖

### 导航栏与 Pipeline 有空白

**检查**：
1. 确认 `body` 和 `main` 的 `padding-top` 为 0
2. 检查 `.pipeline-jumbotron` 的 `padding-top` 是否等于导航栏高度

**解决**：
```css
body, main {
  padding-top: 0 !important;
}

.pipeline-jumbotron {
  padding-top: 56px;  /* 导航栏高度 */
}
```

---

## 开发者指南

### 添加新页面的 Pipeline 背景

1. 在页面模板中找到 `jumbotron` div
2. 添加 `pipeline-jumbotron` 类：
   ```html
   <div class="jumbotron pipeline-jumbotron">
     <div class="container">
       <h1>页面标题</h1>
     </div>
   </div>
   ```

3. 如果页面被插件覆盖，修改插件模板

### 修改动画效果

编辑 `/static/js/pipeline/pipeline-jumbotron.js`：

```javascript
// 调整管道参数
const pipeCount = 30;        // 管道数量（更多 = 更密集）
const turnChanceRange = 0.1; // 转弯概率（更大 = 更多转弯）
const pipePropsTurnChanceMult = 0.05; // 转弯倍率

// 调整颜色
const baseHue = 180;         // 基础色调
const rangeHue = 60;         // 色调范围
const baseLightness = 50;    // 基础亮度
const rangeLightness = 25;   // 亮度范围
```

### 创建新的背景效果

1. 创建新的 CSS 文件：`/static/css/my-background.css`
2. 使用伪元素添加新层：
   ```css
   body::before {
     content: '';
     position: fixed;
     z-index: -3;  /* 在星空下方 */
     /* 你的样式 */
   }
   ```
3. 在 `base.html` 中引入新样式

---

## 致谢

- **Pipeline Animation** 原始效果来自 Ambient Canvas Backgrounds
- **CTFd** 核心主题 - CTFd 官方团队
- **Simplex Noise** - Stefan Gustavson

---

## 许可证

本主题基于 CTFd 核心主题修改，遵循 CTFd 的开源许可证。

---

## 版本历史

### v1.0.0 (2025-11-22)
- ✨ 添加 Pipeline 动态管道背景
- ✨ 添加深邃星空背景效果
- ✨ 实现毛玻璃导航栏
- 🎨 优化深色主题文字颜色
- 🐛 修复模态框交互问题
- 🐛 修复 Pipeline 底部虚化过渡
- 📱 优化响应式设计
- 📝 覆盖插件模板以支持 Pipeline 背景

---

**维护者**：CTFd-fpclose 主题团队  
**最后更新**：2025-11-22
