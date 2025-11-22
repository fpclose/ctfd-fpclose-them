# 自定义首页插件安装指南

## 概述

为了使用主题中的自定义 `index.html` 首页，需要安装配套的 ISCTF Home 插件。该插件会覆盖 CTFd 的默认首页路由。

## 插件功能

- ✅ 自动使用主题的 `index.html` 作为网站首页
- ✅ 显示 ISCTF 2025 赛事介绍页面
- ✅ 不影响其他 CTFd 功能
- ✅ 支持主题切换

## 快速安装

### 方法 1: 手动安装（推荐）

1. **创建插件目录**
   ```bash
   mkdir -p /path/to/CTFd/CTFd/plugins/isctf_home/assets
   ```

2. **创建插件文件**
   
   创建 `/path/to/CTFd/CTFd/plugins/isctf_home/__init__.py`：
   
   ```python
   """
   ISCTF Home Plugin
   Overrides the default homepage to use custom index.html template
   """
   
   from flask import render_template, Blueprint
   from CTFd.plugins import register_plugin_assets_directory
   
   
   def load(app):
       """
       Load the plugin and override the homepage route
       """
       # Create a blueprint for our custom routes
       isctf_bp = Blueprint(
           "isctf_home",
           __name__,
           template_folder="templates",
           static_folder="assets",
       )
       
       # Register plugin assets directory
       register_plugin_assets_directory(
           app, base_path="/plugins/isctf_home/assets/"
       )
       
       # Define custom index route with higher priority
       @isctf_bp.route("/", methods=["GET"])
       def custom_index():
           """
           Custom homepage using index.html template from theme
           """
           return render_template("index.html")
       
       # Register the blueprint with higher priority (url_prefix=None means root)
       # We register it before other blueprints to take precedence
       app.register_blueprint(isctf_bp)
       
       # Move our blueprint's rules to the beginning so they match first
       app.url_map._rules.insert(0, app.url_map._rules.pop())
   ```

3. **创建配置文件**
   
   创建 `/path/to/CTFd/CTFd/plugins/isctf_home/config.json`：
   
   ```json
   {
     "name": "ISCTF Home",
     "route": "/plugins/isctf_home",
     "version": "1.0.0",
     "description": "Custom ISCTF 2025 homepage with event information"
   }
   ```

4. **重启 CTFd**
   ```bash
   docker compose restart ctfd
   # 或其他重启方式
   ```

### 方法 2: 使用 Git 克隆（推荐给开发者）

```bash
# 假设你已经克隆了主题仓库
cd /path/to/CTFd/CTFd/plugins/

# 手动创建插件（因为插件不在主题仓库中）
mkdir -p isctf_home/assets

# 复制上面的代码到对应文件
```

## Docker 部署

如果使用 Docker，在 `docker-compose.yml` 中添加插件目录挂载：

```yaml
version: '3'

services:
  ctfd:
    image: ctfd/ctfd:3.8.1
    volumes:
      # 主题挂载
      - ./CTFd/themes/ctfd-fpclose:/opt/CTFd/CTFd/themes/ctfd-fpclose:ro
      # 插件挂载
      - ./CTFd/plugins/isctf_home:/opt/CTFd/CTFd/plugins/isctf_home:ro
      # ... 其他挂载
    environment:
      - CTFD_THEME=ctfd-fpclose
```

## 验证安装

1. **检查插件是否加载**
   - 查看 CTFd 日志
   - 应该能看到类似 "Loading plugin: isctf_home" 的消息

2. **访问首页**
   - 打开浏览器访问 `http://your-ctfd-url/`
   - 应该能看到 ISCTF 2025 赛事介绍页面
   - 页面应该有毛玻璃效果的白色卡片

3. **检查浏览器控制台**
   - 按 F12 打开开发者工具
   - 应该没有 404 或其他错误

## 故障排除

### 问题 1: 首页仍显示 CTFd 默认页面

**原因**：插件未正确加载或路由优先级不够

**解决方法**：
1. 检查插件目录结构是否正确
2. 确认 `__init__.py` 文件存在且代码正确
3. 查看 CTFd 日志是否有插件加载错误
4. 重启 CTFd 服务

### 问题 2: 出现 500 内部错误

**原因**：主题中缺少 `index.html` 文件

**解决方法**：
1. 确认主题已正确安装
2. 检查 `templates/index.html` 文件是否存在
3. 查看 CTFd 日志获取详细错误信息

### 问题 3: 插件未在管理面板显示

**说明**：这是正常的。该插件是路由插件，不会在插件管理页面显示。只要首页正确显示即表示插件工作正常。

### 问题 4: Docker 环境中插件不工作

**解决方法**：
1. 确认 `docker-compose.yml` 中正确挂载了插件目录
2. 检查文件权限（应该是 644 或 755）
3. 重新创建容器：
   ```bash
   docker compose down
   docker compose up -d
   ```

## 插件工作原理

1. **插件加载**：CTFd 启动时自动加载 `plugins/` 目录下的所有插件
2. **路由注册**：插件创建一个 Flask Blueprint 并注册到 `/` 路径
3. **优先级处理**：通过调整 URL 规则顺序，使插件路由优先于默认路由
4. **模板渲染**：当访问 `/` 时，渲染主题的 `index.html` 模板

## 卸载插件

如果需要恢复 CTFd 默认首页：

```bash
# 删除插件目录
rm -rf /path/to/CTFd/CTFd/plugins/isctf_home/

# 重启 CTFd
docker compose restart ctfd
```

## 注意事项

1. **插件与主题分离**：插件不包含在主题包中，需要单独安装
2. **兼容性**：插件仅在 CTFd 3.8.1+ 版本测试通过
3. **备份**：安装前建议备份原有配置
4. **权限**：确保 CTFd 进程有读取插件文件的权限

## 技术细节

### 文件结构

```
CTFd/
├── plugins/
│   └── isctf_home/
│       ├── __init__.py       # 插件主代码
│       ├── config.json       # 插件配置
│       ├── assets/           # 插件静态资源（空目录）
│       └── README.md         # 插件文档
└── themes/
    └── ctfd-fpclose/
        └── templates/
            └── index.html    # 自定义首页模板
```

### 路由优先级

插件通过以下方式确保路由优先：

1. 注册 Blueprint 到 Flask 应用
2. 使用 `app.url_map._rules.insert(0, ...)` 将规则移到最前
3. Flask 按顺序匹配路由，先匹配到的先执行

## 获取帮助

如果遇到问题：

1. **查看日志**：`docker logs ctfd` 或查看 CTFd 日志文件
2. **提交 Issue**：[GitHub Issues](https://github.com/fpclose/ctfd-fpclose-them/issues)
3. **查看文档**：阅读 README.md 和 INSTALL_CN.md

---

**祝你部署顺利！** 🎉
