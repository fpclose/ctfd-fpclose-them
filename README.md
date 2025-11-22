# CTFd-FPClose 主题

<div align="center">

![CTFd Version](https://img.shields.io/badge/CTFd-v3.8.1-blue)
![License](https://img.shields.io/badge/license-MIT-green)

一个功能强大、视觉震撼的 CTFd 自定义主题，具有动态 Pipeline 背景动画和星空效果。

[English](README.md) | 简体中文

</div>

## ✨ 特性

### 🌊 动态 Pipeline 背景
- 基于 HTML5 Canvas 的流畅动画效果
- 在挑战页面顶部显示炫酷的管道流动动画
- 使用 Simplex Noise 算法实现自然的运动轨迹
- 支持硬件加速，性能优化

### ⭐ 深邃星空背景
- 50+ 个不同大小和亮度的星星
- 星云效果叠加
- 自然的闪烁动画
- 深蓝色渐变背景营造沉浸式体验

### 🎨 现代化设计
- 深色主题优化
- 毛玻璃效果导航栏
- 响应式设计，完美支持移动端
- 优雅的颜色配色方案

### 📱 完全响应式
- 适配所有主流设备尺寸
- 移动端优化布局
- 触摸友好的交互设计

## 🚀 快速开始

### 方法 1: 直接下载安装（推荐）

1. **下载主题**
   ```bash
   cd /path/to/your/CTFd/CTFd/themes/
   git clone https://github.com/fpclose/ctfd-fpclose-theme.git ctfd-fpclose
   ```

2. **激活主题**
   - 访问 CTFd 管理面板
   - 进入 **Admin Panel** → **Config** → **Appearance**
   - 在 **Theme** 下拉菜单中选择 **ctfd-fpclose**
   - 点击 **Update** 保存更改

3. **验证安装**
   - 访问挑战页面，应该能看到 Pipeline 背景动画
   - 检查页面背景是否有星空效果

### 方法 2: Docker 环境安装

1. **修改 docker-compose.yml**
   ```yaml
   services:
     ctfd:
       image: ctfd/ctfd:3.8.1
       volumes:
         - ./CTFd/themes/ctfd-fpclose:/opt/CTFd/CTFd/themes/ctfd-fpclose
       environment:
         - CTFD_THEME=ctfd-fpclose
   ```

2. **重启容器**
   ```bash
   docker compose restart ctfd
   ```

### 方法 3: 通过环境变量

```bash
export CTFD_THEME=ctfd-fpclose
# 然后重启 CTFd 服务
```

## 📦 主题结构

```
ctfd-fpclose/
├── assets/                    # 源文件（未编译）
│   ├── js/                   # JavaScript 源文件
│   └── scss/                 # SCSS 样式源文件
├── static/                    # 编译后的静态资源
│   ├── css/                  
│   │   ├── pipeline/         # Pipeline 动画样式
│   │   ├── starry-background.css  # 星空背景
│   │   └── challenge-card-light.css  # 挑战卡片样式
│   ├── js/
│   │   └── pipeline/         # Pipeline 动画脚本
│   │       ├── pipeline-jumbotron.js  # 主逻辑
│   │       ├── util.js              # 工具函数
│   │       └── noise.min.js         # Simplex Noise 库
│   ├── img/                  # 图片资源
│   ├── sounds/               # 音效文件
│   └── webfonts/             # 字体文件
├── templates/                 # Jinja2 模板文件
│   ├── base.html             # 基础模板
│   ├── challenges.html       # 挑战页面
│   └── ...                   # 其他页面模板
├── package.json              # Node.js 依赖配置
├── vite.config.js            # Vite 构建配置
├── LICENSE                   # 许可证
└── README_CN.md              # 中文说明文档
```

## 🎨 自定义配置

### 修改 Pipeline 动画参数

编辑 `static/js/pipeline/pipeline-jumbotron.js`：

```javascript
// 管道数量（更多 = 更密集的动画）
const pipeCount = 30;

// 速度设置
const baseSpeed = 0.5;    // 基础速度
const rangeSpeed = 1;     // 速度范围

// 颜色设置
const baseHue = 180;      // 基础色相（180 = 青色）
const rangeHue = 60;      // 色相范围

// 背景颜色
const backgroundColor = 'hsla(150,80%,1%,1)';

// 线条粗细
const baseWidth = 2;      // 基础宽度
const rangeWidth = 4;     // 宽度范围
```

### 修改星空效果

编辑 `static/css/starry-background.css` 中的 CSS 变量来调整星空效果。

### 应用到其他页面

要在其他页面也显示 Pipeline 背景，只需在相应模板中添加 `pipeline-jumbotron` class：

```html
<div class="jumbotron pipeline-jumbotron">
  <div class="container">
    <h1>你的标题</h1>
  </div>
</div>
```

## 🔧 开发

如果你想修改主题源代码：

### 环境要求

- Node.js 16+
- Yarn 1.x

### 安装依赖

```bash
cd ctfd-fpclose
yarn install
```

### 开发模式

```bash
# 监听文件变化并自动编译
yarn dev
```

### 生产构建

```bash
# 构建优化后的生产版本
yarn build
```

### 代码格式化

```bash
# 格式化代码
yarn format

# 检查代码格式
yarn lint
```

## 🌐 浏览器兼容性

- ✅ Chrome/Edge 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ 移动浏览器（iOS Safari, Chrome Mobile）

## 📋 系统要求

- **CTFd 版本**: 3.8.1 或更高
- **Python**: 3.9+
- **浏览器**: 支持 HTML5 Canvas 的现代浏览器

## ❓ 常见问题

### Pipeline 动画不显示？

1. **检查浏览器控制台**是否有错误
2. **验证文件加载**：打开开发者工具 → Network 标签页，确认 JS 和 CSS 文件已成功加载
3. **检查 HTML 结构**：确保 jumbotron 元素同时具有 `jumbotron` 和 `pipeline-jumbotron` 两个 class

### 动画性能问题？

如果在低性能设备上运行：

1. **降低管道数量**：修改 `pipeCount` 为 15-20
2. **降低速度**：减小 `baseSpeed` 值
3. **禁用模糊效果**：注释掉 `pipeline-jumbotron.js` 中的 `ctx.b.filter = 'blur(12px)'`

### 主题无法激活？

1. 确认主题文件夹名称为 `ctfd-fpclose`
2. 检查 CTFd 版本是否为 3.8.1+
3. 查看 CTFd 日志是否有错误信息
4. 尝试重启 CTFd 服务

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

### 贡献指南

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 提交 Pull Request

## 📜 许可证

本项目基于 MIT 许可证开源。详见 [LICENSE](LICENSE) 文件。

## 🙏 致谢

- **CTFd Team** - 优秀的 CTF 平台框架
- **Sean Free** - [Ambient Canvas Backgrounds](https://github.com/crnacura/AmbientCanvasBackgrounds) 项目作者，Pipeline 动画的原作者
- **jwagner** - Simplex Noise 库作者

## 📞 联系方式

- **GitHub Issues**: [提交问题](https://github.com/fpclose/ctfd-fpclose-theme/issues)
- **GitHub Repository**: [项目主页](https://github.com/fpclose/ctfd-fpclose-theme)

## 📸 截图预览

### 挑战页面
![Challenges Page](screenshots/challenges.png)

### Pipeline 动画效果
![Pipeline Animation](screenshots/pipeline-animation.gif)

### 星空背景
![Starry Background](screenshots/starry-background.png)

---

<div align="center">

**⭐ 如果这个主题对你有帮助，请给个 Star！**

Made with ❤️ by FPClose

</div>
