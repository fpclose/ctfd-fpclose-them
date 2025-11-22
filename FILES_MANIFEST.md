# CTFd-fpclose 主题文件清单

## 📋 文件分类说明

- 🆕 **新增文件**：为主题特性新创建的文件
- ✏️ **修改文件**：基于原始主题修改的文件
- 📦 **原有文件**：继承自核心主题的文件

---

## 🆕 新增的核心文件

### CSS 样式文件

#### 1. Pipeline 动画样式
```
📁 static/css/pipeline/
└── 🆕 pipeline-jumbotron.css
```
**功能**：Pipeline 动态背景的样式定义
**大小**：~3KB
**关键样式**：
- `.pipeline-jumbotron` - 容器样式
- `.pipeline-jumbotron::after` - 底部虚化过渡
- 响应式断点（768px）

#### 2. 星空背景样式
```
📁 static/css/
└── 🆕 starry-background.css
```
**功能**：星空背景 + 全局样式 + 模态框优化
**大小**：~15KB
**包含**：
- `body::after` - 50个星星效果
- `body::before` - 星云效果
- 全局文字颜色（浅色）
- 模态框 z-index 和 pointer-events 修复
- 卡片和容器样式
- ECharts 图表文字颜色

#### 3. 挑战卡片样式
```
📁 static/css/
└── 🆕 challenge-card-light.css
```
**功能**：挑战卡片的浅色文字样式
**大小**：~2KB
**样式**：
- `.challenge-button` 文字颜色
- `.challenge-solved` 已解决样式
- `.category-header` 分类标题

### JavaScript 文件

#### 1. Pipeline 动画脚本
```
📁 static/js/pipeline/
├── 🆕 pipeline-jumbotron.js
├── 🆕 util.js
└── 🆕 noise.min.js
```

**pipeline-jumbotron.js**
- **功能**：Pipeline 动画核心逻辑
- **大小**：~12KB
- **依赖**：util.js, noise.min.js

**util.js**
- **功能**：工具函数库
- **大小**：~2KB
- **包含**：随机数生成、坐标转换等

**noise.min.js**
- **功能**：Simplex Noise 算法
- **大小**：~8KB（压缩后）
- **用途**：生成自然的噪声图案

---

## ✏️ 修改的模板文件

### 主题模板

#### 1. 基础模板
```
📁 templates/
└── ✏️ base.html
```
**修改内容**：
- 在 `</body>` 前添加 CSS/JS 引用
- 加载 Pipeline 相关资源
```html
<link rel="stylesheet" href="/themes/ctfd-fpclose/static/css/pipeline/pipeline-jumbotron.css">
<link rel="stylesheet" href="/themes/ctfd-fpclose/static/css/starry-background.css">
<link rel="stylesheet" href="/themes/ctfd-fpclose/static/css/challenge-card-light.css">
<script src="/themes/ctfd-fpclose/static/js/pipeline/noise.min.js"></script>
<script src="/themes/ctfd-fpclose/static/js/pipeline/util.js"></script>
<script src="/themes/ctfd-fpclose/static/js/pipeline/pipeline-jumbotron.js"></script>
```

### 插件模板覆盖

#### 2. 团队插件模板
```
📁 ../../plugins/ctfd_tuandui/templates/teams/
└── ✏️ teams.html
```
**修改**：`<div class="jumbotron">` → `<div class="jumbotron pipeline-jumbotron">`
**行号**：第4行

#### 3. 用户插件模板
```
📁 ../../plugins/ctfd_user/templates/users/
├── ✏️ users.html
└── ✏️ private.html

📁 ../../plugins/ctfd_user/templates/
└── ✏️ settings.html
```
**修改**：所有文件的 `jumbotron` div 添加 `pipeline-jumbotron` 类

#### 4. 排行榜插件模板
```
📁 ../../plugins/ctfd-paihangb/templates/scoreboard/
└── ✏️ custom_scoreboard.html
```
**修改**：`<div class="jumbotron">` → `<div class="jumbotron pipeline-jumbotron">`

---

## 📦 继承的核心文件

以下文件直接继承自 CTFd 核心主题，未作修改：

### 模板文件
```
templates/
├── 📦 challenges.html
├── 📦 scoreboard.html
├── 📦 login.html
├── 📦 register.html
├── 📦 setup.html
├── 📦 teams/
│   ├── 📦 new_team.html
│   ├── 📦 join_team.html
│   ├── 📦 teams.html
│   └── 📦 team.html
├── 📦 users/
│   ├── 📦 users.html
│   └── 📦 user.html
└── 📦 components/
    ├── 📦 navbar.html
    ├── 📦 errors.html
    └── 📦 ...
```

### 静态资源
```
static/
├── 📦 assets/           # 编译后的 JS/CSS
├── 📦 img/              # 图片资源
└── 📦 sounds/           # 音效文件
```

---

## 📊 文件统计

| 类别 | 数量 | 说明 |
|------|------|------|
| 🆕 新增 CSS | 3 个 | pipeline-jumbotron.css, starry-background.css, challenge-card-light.css |
| 🆕 新增 JS | 3 个 | pipeline-jumbotron.js, util.js, noise.min.js |
| ✏️ 修改模板 | 5 个 | base.html + 4个插件模板 |
| 📦 继承文件 | 50+ | 所有其他核心主题文件 |
| 📝 文档文件 | 3 个 | THEME_DOCUMENTATION.md, QUICK_START.md, FILES_MANIFEST.md |

---

## 🗂️ 完整目录结构

```
ctfd-fpclose/
│
├── 📝 README.md                           # 主题说明（原有）
├── 📝 THEME_DOCUMENTATION.md              # 🆕 完整文档
├── 📝 QUICK_START.md                      # 🆕 快速开始
├── 📝 FILES_MANIFEST.md                   # 🆕 本文件
├── 📝 LICENSE                             # 许可证
├── 📝 package.json                        # NPM 配置
├── 📝 yarn.lock                           # 依赖锁定
│
├── 📁 static/
│   ├── 📁 css/
│   │   ├── 📁 pipeline/
│   │   │   └── 🆕 pipeline-jumbotron.css
│   │   ├── 🆕 starry-background.css
│   │   └── 🆕 challenge-card-light.css
│   │
│   ├── 📁 js/
│   │   └── 📁 pipeline/
│   │       ├── 🆕 pipeline-jumbotron.js
│   │       ├── 🆕 util.js
│   │       └── 🆕 noise.min.js
│   │
│   ├── 📁 assets/                         # 📦 编译后的资源
│   ├── 📁 img/                            # 📦 图片
│   └── 📁 sounds/                         # 📦 音效
│
├── 📁 templates/
│   ├── ✏️ base.html                       # 修改：添加资源引用
│   ├── 📦 challenges.html
│   ├── 📦 scoreboard.html
│   ├── 📦 login.html
│   ├── 📦 register.html
│   ├── 📦 setup.html
│   ├── 📁 teams/
│   │   └── 📦 ...
│   ├── 📁 users/
│   │   └── 📦 ...
│   └── 📁 components/
│       └── 📦 ...
│
└── 📁 assets/                             # 📦 源文件（编译前）
    └── ...
```

---

## 🔗 文件依赖关系

### CSS 加载顺序
```
1. main.css (CTFd 核心样式)
   ↓
2. pipeline-jumbotron.css (Pipeline 样式)
   ↓
3. starry-background.css (星空 + 全局样式)
   ↓
4. challenge-card-light.css (挑战卡片样式)
```

### JavaScript 加载顺序
```
1. CTFd 核心 JS
   ↓
2. noise.min.js (Simplex Noise)
   ↓
3. util.js (工具函数)
   ↓
4. pipeline-jumbotron.js (Pipeline 动画)
```

### 模板继承关系
```
base.html
  ├── challenges.html
  ├── scoreboard.html
  ├── teams/teams.html
  └── users/users.html
```

---

## 🔍 关键代码位置

### Pipeline 初始化
**文件**：`static/js/pipeline/pipeline-jumbotron.js`
**位置**：第 ~200 行
```javascript
window.addEventListener('load', function() {
  var jumbotronElements = document.querySelectorAll('.pipeline-jumbotron');
  jumbotronElements.forEach(function(el) {
    initializePipelineAnimation(el);
  });
});
```

### 星空效果定义
**文件**：`static/css/starry-background.css`
**位置**：第 19-94 行
```css
body::after {
  content: '';
  position: fixed;
  background-image: 
    radial-gradient(...),  /* 50个星星 */
    ...
}
```

### 模态框修复
**文件**：`static/css/starry-background.css`
**位置**：第 134-267 行
```css
.modal-backdrop { z-index: 1049; }
.modal { z-index: 1050; }
#challenge-window { z-index: 1051; }
```

### 虚化过渡
**文件**：`static/css/pipeline/pipeline-jumbotron.css`
**位置**：第 22-38 行
```css
.pipeline-jumbotron::after {
  content: '';
  height: 80px;
  background: linear-gradient(...);
}
```

---

## 📝 修改记录

### base.html 修改位置
**行号**：72-78（文件末尾）
**添加内容**：6行 CSS 和 JS 引用

### 插件模板修改汇总
| 文件 | 修改行 | 修改内容 |
|------|--------|----------|
| ctfd_tuandui/teams/teams.html | 4 | 添加 `pipeline-jumbotron` 类 |
| ctfd_user/users/users.html | 4 | 添加 `pipeline-jumbotron` 类 |
| ctfd_user/users/private.html | 4 | 添加 `pipeline-jumbotron` 类 |
| ctfd_user/settings.html | 5 | 添加 `pipeline-jumbotron` 类 |
| ctfd-paihangb/scoreboard/custom_scoreboard.html | 4 | 添加 `pipeline-jumbotron` 类 |

---

## 🎯 核心功能映射

| 功能 | CSS 文件 | JS 文件 | 模板文件 |
|------|----------|---------|----------|
| Pipeline 动画 | pipeline-jumbotron.css | pipeline-jumbotron.js, util.js, noise.min.js | base.html |
| 星空背景 | starry-background.css | - | base.html |
| 虚化过渡 | pipeline-jumbotron.css | - | - |
| 文字颜色 | starry-background.css, challenge-card-light.css | - | - |
| 模态框修复 | starry-background.css | - | - |

---

## 💾 备份建议

在修改主题前，建议备份以下文件：

```bash
# 备份整个主题目录
cp -r /opt/CTFd/CTFd/themes/ctfd-fpclose /opt/CTFd/CTFd/themes/ctfd-fpclose.backup

# 或仅备份关键文件
cd /opt/CTFd/CTFd/themes/ctfd-fpclose
tar -czf ctfd-fpclose-backup-$(date +%Y%m%d).tar.gz \
  static/css/pipeline/ \
  static/css/starry-background.css \
  static/css/challenge-card-light.css \
  static/js/pipeline/ \
  templates/base.html
```

---

**版本**：v1.0.0  
**生成日期**：2025-11-22  
**文件总数**：65+ (包括继承的核心文件)
