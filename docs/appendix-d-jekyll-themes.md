# 附录D：GitHub Pages 常用 Jekyll 主题 🎨

> 一键切换网站外观，让你的 GitHub Pages 更专业

---

## 什么是 Jekyll 主题？

GitHub Pages 内置支持 Jekyll 静态网站生成器，你可以通过简单的配置切换网站主题。

---

## 🎯 官方推荐主题

GitHub Pages 支持以下主题，只需在 `_config.yml` 中设置即可：

| 主题名 | 风格 | 适合场景 | 配置值 |
|--------|------|---------|--------|
| **Minima** | 简洁白 | 博客、个人主页 | `theme: minima` |
| **Cayman** | 现代蓝 | 项目文档 | `theme: jekyll-theme-cayman` |
| **Architect** | 大气 | 产品介绍 | `theme: jekyll-theme-architect` |
| **Dinky** | 小巧 | 简单项目 | `theme: jekyll-theme-dinky` |
| **Hacker** | 暗黑风 | 技术博客 | `theme: jekyll-theme-hacker` |
| **Leap Day** | 活力橙 | 个人主页 | `theme: jekyll-theme-leap-day` |
| **Merlot** | 商务灰 | 专业文档 | `theme: jekyll-theme-merlot` |
| **Midnight** | 深夜蓝 | 教程 | `theme: jekyll-theme-midnight` |
| **Minimal** | 极简 | 简历、Portfolio | `theme: jekyll-theme-minimal` |
| **Slate** | 石板灰 | 技术项目 | `theme: jekyll-theme-slate` |
| **Time Machine** | 复古 | 回忆录 | `theme: jekyll-theme-time-machine` |
| **Tactile** | 触感 | API 文档 | `theme: jekyll-theme-tactile` |

---

## 🚀 快速上手

### 第1步：编辑 `_config.yml`

```yaml
# 选择主题
theme: jekyll-theme-cayman

# 网站标题
title: 我的技术博客
description: 记录学习与成长

# 作者信息
author:
  name: GTX950L
  email: your@email.com
```

### 第2步：提交到 GitHub

```bash
git add _config.yml
git commit -m "设置 Cayman 主题"
git push
```

GitHub Pages 会自动重新构建，几分钟后刷新就能看到新主题。

---

## 🌈 第三方热门主题

如果官方主题不够用，可以尝试：

| 主题 | 特点 | 安装方式 |
|------|------|---------|
| **Chirpy** | 功能丰富的博客主题 | `gem "jekyll-theme-chirpy"` |
| **Minimal Mistakes** | 最受欢迎的 Jekyll 主题 | `remote_theme: "mmistakes/minimal-mistakes"` |
| **Jekyll Now** | 零配置快速搭建 | Fork [jekyll-now](https://github.com/barryclark/jekyll-now) |
| **TeXt** | 中文友好 | `remote_theme: "kitian616/jekyll-TeXt-theme"` |

### 使用远程主题

```yaml
# _config.yml
remote_theme: mmistakes/minimal-mistakes
plugins:
  - jekyll-remote-theme
```

---

## 💡 主题选择建议

| 你的需求 | 推荐主题 |
|---------|---------|
| 💼 技术博客 | **Hacker** 或 **Cayman** |
| 📖 项目文档 | **Architect** 或 **Minimal** |
| 👤 个人主页/简历 | **Minima** 或 **Minimal** |
| 🎮 游戏相关 | **Hacker**（暗黑风） |
| 📊 数据分析博客 | **Slate** |

---

## 🔄 Jekyll 的替代方案

除了 Jekyll，GitHub Pages 还支持其他静态网站生成器。如果你的需求超出了 Jekyll 的能力，可以考虑这些方案：

### 1. Hugo（Go 语言）⚡

**优点**：
- 🚀 **速度极快**（比 Jekyll 快 10-100 倍）
- 🌍 **社区活跃**，主题丰富
- 💪 **功能强大**（内置 SEO、多语言、分页等）
- 📝 **无需 Ruby 环境**，一个二进制文件搞定

**缺点**：
- 📚 学习曲线比 Jekyll 陡峭
- 🧩 模板语法（Go Templates）需要额外学习

**适合**：大型文档网站、博客、企业官网

```bash
# 安装 Hugo
# macOS: brew install hugo
# Windows: winget install Hugo.Hugo

# 创建新站点
hugo new site my-blog
cd my-blog

# 添加主题
git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
echo "theme = 'ananke'" >> hugo.toml

# 创建第一篇文章
hugo new content posts/my-first-post.md

# 本地预览
hugo server -D
```

### 2. MkDocs（Python）📘

**优点**：
- 📝 **Markdown 原生支持**（比 Jekyll 更自然）
- 🎨 **Material for MkDocs 主题**（非常漂亮，中文友好）
- 🔍 **内置搜索功能**
- 🐍 **Python 用户友好**

**缺点**：
- ⚙️ 功能不如 Hugo 丰富
- 📦 依赖 Python 生态

**适合**：项目文档、API 文档、技术手册

```bash
# 安装 MkDocs
pip install mkdocs mkdocs-material

# 创建新站点
mkdocs new my-docs
cd my-docs

# 编辑 mkdocs.yml
echo "site_name: 我的文档" > mkdocs.yml
echo "theme:" >> mkdocs.yml
echo "  name: material" >> mkdocs.yml

# 本地预览
mkdocs serve

# 构建
mkdocs build
```

### 3. VuePress / VitePress（JavaScript）💚

**优点**：
- 💚 **Vue 生态**（VuePress）或 **Vite 生态**（VitePress）
- ⚡ **极快的热更新**
- 🧩 **强大的插件系统**
- 🎯 **适合前端开发者**

**缺点**：
- 📦 需要 Node.js 环境
- 👥 社区相对 Jekyll 小

**适合**：前端项目文档、组件库文档

```bash
# VitePress（推荐 VuePress 的新项目用这个）
npm create vitepress@latest my-docs
cd my-docs
npm run docs:dev
```

### 如何选择？

| 你的情况 | 推荐方案 |
|---------|---------|
| **零配置，GitHub 原生支持** | Jekyll ✅ |
| **大型文档网站（上千页面）** | Hugo ⚡ |
| **项目文档，追求外观** | MkDocs + Material 🎨 |
| **前端开发者** | VitePress 💚 |
| **Python 用户** | MkDocs 🐍 |
| **Go 语言用户** | Hugo 🚀 |

> 💡 所有这些生成器都可以通过 GitHub Actions 自动部署到 Pages。详见 [第20章：Pages 搭建网站](../docs/chapter20-pages-website.md)。

---

## 🔗 更多资源

- [GitHub Pages 官方主题预览](https://pages.github.com/themes/)
- [Jekyll 主题市场](https://jekyllthemes.io/)
- [GitHub Pages 文档](https://docs.github.com/pages)

---

<p align="right">
  <a href="appendix-c-actions-marketplace.md">← 附录C：Actions 市场</a>
  &nbsp;|&nbsp;
  <a href="appendix-e-conventional-commits.md">附录E：Conventional Commits →</a>
</p>
