# 第20章：GitHub Pages 搭建网站 🌍

> 想免费搭建个人博客或项目官网？GitHub Pages 可以帮你！

## 📋 目录

- [什么是 GitHub Pages？](#什么是-github-pages)
- [创建第一个 Pages 网站](#创建第一个-pages-网站)
- [自定义域名](#自定义域名)
- [使用 Jekyll 生成静态网站](#使用-jekyll-生成静态网站)
- [常见问题](#常见问题)

---

## 什么是 GitHub Pages？

### 基本定义

**GitHub Pages** 是 GitHub 提供的**免费静态网站托管服务**。

### 可以做什么？

| 用途 | 示例 |
|------|------|
| **个人博客** | `https://你的用户名.github.io/` |
| **项目官网** | `https://你的用户名.github.io/项目名/` |
| **文档网站** | 展示项目文档 |
| **作品集** | 展示你的项目 |

### 为什么推荐？

| 优点 | 说明 |
|------|------|
| **免费** | 无限流量、无限带宽 |
| **简单** | 只需要推送 HTML 文件 |
| **集成** | 和 GitHub 仓库无缝集成 |
| **HTTPS** | 自动配置 SSL 证书 |

---

## 创建第一个 Pages 网站

### 方法1：使用主分支（最简单）⭐

#### 步骤：

**第1步：创建仓库**

```
仓库名：你的用户名.github.io
│
例如：GTX950L.github.io
│
⚠️ 注意：仓库名必须是 你的用户名.github.io！
```

**第2步：克隆到本地**

```bash
git clone https://github.com/GTX950L/GTX950L.github.io.git
cd GTX950L.github.io
```

**第3步：创建首页**

```bash
# 创建 index.html
echo '<!DOCTYPE html>
<html>
<head>
    <title>我的博客</title>
</head>
<body>
    <h1>欢迎来到我的博客！</h1>
</body>
</html>' > index.html

# 提交并推送
git add .
git commit -m "Initial commit"
git push origin main
```

**第4步：访问网站**

```
等待 1-2 分钟...
访问：https://gtx950l.github.io/
```

🎉 **你的网站上线了！**

---

### 方法2：使用 docs/ 目录

**适用场景**：为项目创建官网。

#### 步骤：

```
1. 在项目的 docs/ 目录创建网站文件
2. 进入仓库 Settings → Pages
3. Source 选择 "Deploy from a branch"
4. Branch 选择 "main /docs"
5. 点击 Save
```

---

## 自定义域名

### 为什么需要？

```
默认域名：https://gtx950l.github.io/
                ↓
自定义后：https://yourname.com/
```

### 配置步骤

#### 第1步：购买域名

推荐服务商：
- 国内：阿里云、腾讯云
- 国外：Namecheap、GoDaddy

#### 第2步：添加 CNAME 文件

```bash
# 在项目根目录创建 CNAME 文件
echo 'yourname.com' > CNAME

# 提交并推送
git add CNAME
git commit -m "Add custom domain"
git push origin main
```

#### 第3步：配置 DNS

在域名服务商添加记录：

| 类型 | 名称 | 值 |
|------|------|------|
| **A** | `@` | `185.199.108.153` |
| **A** | `@` | `185.199.109.153` |
| **A** | `@` | `185.199.110.153` |
| **A** | `@` | `185.199.111.153` |
| **CNAME** | `www` | `你的用户名.github.io.` |

#### 第4步：在 GitHub 上启用 HTTPS

```
Settings → Pages → Enforce HTTPS → 勾选
```

> ⚠️ **注意**：DNS 生效需要 24-48 小时。

---

## 使用 Jekyll 生成静态网站

### 什么是 Jekyll？

**Jekyll** 是 GitHub Pages 内置的**静态网站生成器**。

### 为什么使用 Jekyll？

| 需求 | 手写 HTML | 使用 Jekyll |
|------|------------|--------------|
| **写文章** | 手写 HTML | ✅ 写 Markdown 即可 |
| **主题** | 自己写 CSS | ✅ 使用现成主题 |
| **分页** | 自己实现 | ✅ 内置支持 |
| **分类/标签** | 自己实现 | ✅ 内置支持 |

---

### 快速开始

#### 第1步：创建 Jekyll 站点

```bash
# 安装 Jekyll（需要先安装 Ruby）
gem install jekyll

# 创建新站点
jekyll new my-blog

# 进入目录
cd my-blog

# 启动本地预览
bundle exec jekyll serve
```

访问 `http://localhost:4000/` 预览！

#### 第2步：推送到 GitHub

```bash
# 初始化 Git
git init
git add .
git commit -m "Initial commit"

# 添加远程仓库
git remote add origin https://github.com/GTX950L/GTX950L.github.io.git

# 推送
git push -u origin main
```

#### 第3步：选择主题

在 `_config.yml` 中配置：

```yaml
theme: jekyll-theme-minima

# 或直接使用远程主题
remote_theme: pages-themes/slate@v0.2.0
```

---

### 推荐主题

| 主题 | 风格 | 适用 |
|------|------|------|
| **Minima** | 简洁 | 个人博客 |
| **Slate** | 深色 | 技术文档 |
| **Cayman** | 现代 | 项目官网 |
| **Midnight** | 酷炫 | 作品集 |

---

## 常见问题

### Q1: GitHub Pages 可以搭建动态网站吗？

**A**: ❌ **不可以！**

GitHub Pages 只能托管**静态文件**（HTML、CSS、JS）。

**如果需要动态功能**（如用户登录、数据库）：
- ✅ 使用 **Vercel**（支持 Node.js）
- ✅ 使用 **Heroku**（支持多种语言）
- ✅ 使用 **自己的服务器**

---

### Q2: 网站更新后，为什么没有变化？

**A**: 可能是缓存问题。

**解决方法**：

```bash
# 强制刷新浏览器
Ctrl + F5  # Windows
Cmd + Shift + R  # macOS
```

或者等待 1-2 分钟（GitHub Pages 部署需要时间）。

---

### Q3: 可以使用 React/Vue 搭建 Pages 吗？

**A**: ✅ **可以！**

**步骤**：

```
1. 使用 create-react-app 或 Vue CLI 创建项目
2. 运行 npm run build 生成静态文件
3. 把 dist/ 或 build/ 目录推送到 GitHub
4. 在 Settings → Pages 中指定该目录
```

---

## 📝 实战练习

### 练习1：创建个人博客

**任务**：
1. 创建 `你的用户名.github.io` 仓库
2. 添加 `index.html`
3. 访问网站，确认上线

---

### 练习2：使用 Jekyll

**任务**：
1. 本地安装 Jekyll
2. 创建博客站点
3. 写第一篇博客（Markdown 文件）
4. 推送到 GitHub

---

### 练习3：自定义域名

**任务**：
1. 购买一个域名
2. 配置 CNAME 和 DNS
3. 启用 HTTPS

---

## 📚 延伸阅读

- [第19章：GitHub Actions](chapter19-actions-intro.md) - 自动部署网站
- [附录D：常用 Jekyll 主题](appendix-d-jekyll-themes.md) - 更多主题选择

---

## 📝 本章小结

你现在学会了：

- ✅ 什么是 GitHub Pages
- ✅ 如何创建第一个 Pages 网站
- ✅ 如何自定义域名
- ✅ 如何使用 Jekyll 生成静态网站

### 💡 核心要点

> **GitHub Pages 是搭建个人博客的最佳选择！**
> 
> 完全免费、无限流量、自动 HTTPS。
> 配合 Jekyll，你可以专注于写文章，而不用管服务器。

---

<p align="right">
  <a href="chapter19-actions-intro.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter21-security-issues.md">下一章 →</a>
</p>