# CTFd-FPClose 主题安装指南

## 目录

- [系统要求](#系统要求)
- [安装方法](#安装方法)
  - [方法 1: 直接安装（推荐）](#方法-1-直接安装推荐)
  - [方法 2: Docker 安装](#方法-2-docker-安装)
  - [方法 3: 手动安装](#方法-3-手动安装)
- [激活主题](#激活主题)
- [验证安装](#验证安装)
- [故障排除](#故障排除)

---

## 系统要求

在安装之前，请确保你的系统满足以下要求：

- **CTFd 版本**: 3.8.1 或更高版本
- **Python**: 3.9+
- **浏览器**: 支持 HTML5 Canvas 的现代浏览器
  - Chrome/Edge 90+
  - Firefox 88+
  - Safari 14+
  - 移动浏览器（iOS Safari, Chrome Mobile）

---

## 安装方法

### 方法 1: 直接安装（推荐）

这是最简单快捷的安装方法，适合大多数用户。

#### 步骤 1: 克隆主题仓库

```bash
# 进入 CTFd 主题目录
cd /path/to/your/CTFd/CTFd/themes/

# 克隆主题仓库
git clone https://github.com/fpclose/ctfd-fpclose-theme.git ctfd-fpclose
```

**注意**: 确保克隆后的文件夹名称为 `ctfd-fpclose`，这是 CTFd 识别主题的关键。

#### 步骤 2: 重启 CTFd

```bash
# 如果使用 Docker
docker compose restart ctfd

# 如果使用 systemd
sudo systemctl restart ctfd

# 如果直接运行
# 按 Ctrl+C 停止，然后重新运行启动命令
```

#### 步骤 3: 激活主题

参见 [激活主题](#激活主题) 部分。

---

### 方法 2: Docker 安装

如果你的 CTFd 运行在 Docker 容器中，推荐使用这种方法。

#### 步骤 1: 下载主题

```bash
# 在 CTFd 项目根目录
cd /path/to/your/CTFd-project/

# 创建主题目录（如果不存在）
mkdir -p CTFd/themes

# 克隆主题
cd CTFd/themes
git clone https://github.com/fpclose/ctfd-fpclose-theme.git ctfd-fpclose
```

#### 步骤 2: 修改 docker-compose.yml

在你的 `docker-compose.yml` 文件中，找到 `ctfd` 服务部分，添加卷挂载和环境变量：

```yaml
version: '3'

services:
  ctfd:
    image: ctfd/ctfd:3.8.1
    restart: always
    ports:
      - "8000:8000"
    volumes:
      # 挂载主题目录
      - ./CTFd/themes/ctfd-fpclose:/opt/CTFd/CTFd/themes/ctfd-fpclose:ro
      # ... 其他挂载
    environment:
      # 设置默认主题（可选）
      - CTFD_THEME=ctfd-fpclose
      # ... 其他环境变量
    depends_on:
      - db
      - cache
    networks:
      - default

  db:
    # 数据库配置...

  cache:
    # 缓存配置...
```

#### 步骤 3: 重启容器

```bash
docker compose down
docker compose up -d
```

---

### 方法 3: 手动安装

如果你需要更精细的控制，可以手动安装主题。

#### 步骤 1: 下载主题文件

**选项 A: 使用 Git**
```bash
git clone https://github.com/fpclose/ctfd-fpclose-theme.git
```

**选项 B: 下载 ZIP**
1. 访问 [GitHub Release 页面](https://github.com/fpclose/ctfd-fpclose-theme/releases)
2. 下载最新版本的 ZIP 文件
3. 解压缩文件

#### 步骤 2: 复制主题文件

```bash
# 假设你下载到了 ~/Downloads/ctfd-fpclose-theme
cp -r ~/Downloads/ctfd-fpclose-theme /path/to/your/CTFd/CTFd/themes/ctfd-fpclose
```

#### 步骤 3: 设置权限

```bash
# 确保 CTFd 进程有读取权限
cd /path/to/your/CTFd/CTFd/themes/
chown -R ctfd:ctfd ctfd-fpclose  # 根据你的用户和组调整
chmod -R 755 ctfd-fpclose
```

#### 步骤 4: 重启 CTFd

```bash
# 根据你的部署方式重启 CTFd
```

---

## 激活主题

安装完成后，需要在 CTFd 管理面板中激活主题。

### 通过管理面板激活（推荐）

1. **登录 CTFd**
   - 使用管理员账号登录 CTFd

2. **进入配置页面**
   - 点击顶部导航栏的 **Admin Panel**
   - 在左侧菜单中选择 **Config**
   - 选择 **Appearance** 标签页

3. **选择主题**
   - 在 **Theme** 下拉菜单中找到 **ctfd-fpclose**
   - 选择它

4. **保存更改**
   - 点击页面底部的 **Update** 按钮
   - 等待页面刷新

5. **验证**
   - 打开一个新标签页
   - 访问挑战页面（Challenges）
   - 应该能看到新主题已经生效

### 通过环境变量激活

如果你想让主题在启动时就默认激活：

**Docker 方式**:
```yaml
# docker-compose.yml
services:
  ctfd:
    environment:
      - CTFD_THEME=ctfd-fpclose
```

**传统部署方式**:
```bash
export CTFD_THEME=ctfd-fpclose
# 然后启动 CTFd
```

**配置文件方式**:
```python
# CTFd/config.py 或 CTFd/config.ini
THEME = "ctfd-fpclose"
```

---

## 验证安装

安装并激活主题后，请进行以下验证：

### 1. 检查主题是否激活

1. 访问 CTFd 首页
2. 检查页面背景是否显示星空效果
3. 访问挑战页面（Challenges）
4. 检查页面顶部是否有 Pipeline 动画背景

### 2. 检查浏览器控制台

1. 按 F12 打开浏览器开发者工具
2. 切换到 **Console** 标签页
3. 刷新页面
4. 确认没有错误信息
   - 如果看到 `Pipeline: .pipeline-jumbotron container not found` 警告，这在非挑战页面是正常的

### 3. 检查文件加载

1. 在浏览器开发者工具中切换到 **Network** 标签页
2. 刷新页面
3. 筛选 CSS 和 JS 文件
4. 确认以下文件已成功加载（状态码 200）：
   - `/themes/ctfd-fpclose/static/css/pipeline/pipeline-jumbotron.css`
   - `/themes/ctfd-fpclose/static/css/starry-background.css`
   - `/themes/ctfd-fpclose/static/js/pipeline/pipeline-jumbotron.js`
   - `/themes/ctfd-fpclose/static/js/pipeline/util.js`
   - `/themes/ctfd-fpclose/static/js/pipeline/noise.min.js`

### 4. 测试响应式设计

1. 使用浏览器的响应式设计模式（F12 → 切换设备模拟器）
2. 测试不同屏幕尺寸：
   - 移动设备（375px）
   - 平板设备（768px）
   - 桌面设备（1920px）
3. 确认布局正常显示

---

## 故障排除

### 问题 1: 主题不在下拉菜单中

**症状**: 在管理面板的主题下拉菜单中看不到 `ctfd-fpclose`

**解决方法**:
1. 检查主题文件夹名称是否为 `ctfd-fpclose`
2. 检查文件夹位置是否正确：`/path/to/CTFd/CTFd/themes/ctfd-fpclose`
3. 确认 `templates/base.html` 文件存在
4. 重启 CTFd 服务
5. 清除浏览器缓存并重新登录

### 问题 2: Pipeline 动画不显示

**症状**: 挑战页面顶部没有动画效果

**解决方法**:
1. **检查浏览器控制台**
   ```
   F12 → Console 标签页
   查看是否有 JavaScript 错误
   ```

2. **验证文件加载**
   ```
   F12 → Network 标签页
   确认以下文件已加载：
   - pipeline-jumbotron.js
   - util.js
   - noise.min.js
   ```

3. **检查 HTML 结构**
   ```
   F12 → Elements 标签页
   找到 jumbotron 元素
   确认它有 class="jumbotron pipeline-jumbotron"
   ```

4. **验证 Canvas 元素**
   ```javascript
   // 在控制台中运行
   document.querySelector('.pipeline-jumbotron canvas')
   // 应该返回 canvas 元素，而不是 null
   ```

### 问题 3: 星空背景不显示

**症状**: 页面背景是纯色，没有星空效果

**解决方法**:
1. 检查 `starry-background.css` 是否加载
2. 检查浏览器是否支持 CSS animations
3. 查看浏览器控制台是否有 CSS 错误
4. 尝试硬刷新页面（Ctrl+Shift+R 或 Cmd+Shift+R）

### 问题 4: 动画性能差

**症状**: 动画卡顿或帧率低

**解决方法**:

**临时方案 - 调整性能参数**:

编辑 `/path/to/CTFd/CTFd/themes/ctfd-fpclose/static/js/pipeline/pipeline-jumbotron.js`：

```javascript
// 减少管道数量
const pipeCount = 15;  // 默认是 30

// 降低速度
const baseSpeed = 0.3;  // 默认是 0.5

// 禁用模糊效果（在 render() 函数中）
// 注释掉这行：
// ctx.b.filter = 'blur(12px)'
```

**永久方案**:
1. 升级硬件或使用性能更好的设备
2. 关闭其他占用 GPU 的程序
3. 更新显卡驱动

### 问题 5: 文件权限错误

**症状**: CTFd 日志中出现权限相关错误

**解决方法**:
```bash
# 设置正确的所有者
sudo chown -R ctfd:ctfd /path/to/CTFd/CTFd/themes/ctfd-fpclose

# 设置正确的权限
sudo chmod -R 755 /path/to/CTFd/CTFd/themes/ctfd-fpclose

# 对于 Docker 部署
docker exec ctfd chown -R ctfd:ctfd /opt/CTFd/CTFd/themes/ctfd-fpclose
```

### 问题 6: Docker 挂载问题

**症状**: Docker 容器中看不到主题文件

**解决方法**:
1. 确认主题文件在宿主机上存在
2. 检查 `docker-compose.yml` 中的挂载路径是否正确
3. 重新创建容器：
   ```bash
   docker compose down
   docker compose up -d
   ```
4. 验证挂载：
   ```bash
   docker exec ctfd ls -la /opt/CTFd/CTFd/themes/ctfd-fpclose
   ```

### 问题 7: 移动端显示异常

**症状**: 在手机或平板上显示不正常

**解决方法**:
1. 清除移动设备浏览器缓存
2. 确认 viewport meta 标签存在
3. 检查响应式 CSS 是否正确加载
4. 使用浏览器的移动设备模拟器进行调试

---

## 获取帮助

如果以上方法都无法解决你的问题，可以：

1. **查看完整文档**: [THEME_DOCUMENTATION.md](THEME_DOCUMENTATION.md)
2. **提交 Issue**: [GitHub Issues](https://github.com/fpclose/ctfd-fpclose-theme/issues)
3. **查看 CTFd 官方文档**: [CTFd Documentation](https://docs.ctfd.io)

提交 Issue 时，请包含以下信息：
- CTFd 版本
- 浏览器类型和版本
- 操作系统
- 详细的错误信息和截图
- 浏览器控制台输出

---

## 卸载主题

如果需要卸载主题：

1. **切换到其他主题**
   - 在管理面板中选择其他主题（如 core）
   - 点击 Update 保存

2. **删除主题文件**
   ```bash
   rm -rf /path/to/CTFd/CTFd/themes/ctfd-fpclose
   ```

3. **重启 CTFd**
   ```bash
   docker compose restart ctfd
   # 或其他重启方式
   ```

---

**祝你使用愉快！** 🎉
