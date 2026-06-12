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

## 🔗 更多资源

- [GitHub Pages 官方主题预览](https://pages.github.com/themes/)
- [Jekyll 主题市场](https://jekyllthemes.io/)
- [GitHub Pages 文档](https://docs.github.com/pages)

---

<p align="right">
  <a href="appendix-c-actions-marketplace.md">← 附录C：Actions 市场</a>
  &nbsp;|&nbsp;
  <a href="../README.md">返回首页 →</a>
</p>
