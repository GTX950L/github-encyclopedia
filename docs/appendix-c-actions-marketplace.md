# 附录C：GitHub Actions 市场常用 Actions 🛒

> 不用重复造轮子！这些现成的 Actions 直接拿来用

---

## 什么是 Actions 市场？

GitHub Actions 市场（[Marketplace](https://github.com/marketplace?type=actions)）提供了数以千计的预构建 Actions，你只需要在 Workflow 中引用即可使用。

---

## 🏷️ 官方推荐 Actions

### 代码检出与工作流基础

| Action | 用途 | 引用方式 |
|--------|------|---------|
| `actions/checkout` | 检出代码到运行环境 | `uses: actions/checkout@v4` |
| `actions/setup-node` | 安装 Node.js 环境 | `uses: actions/setup-node@v4` |
| `actions/setup-python` | 安装 Python 环境 | `uses: actions/setup-python@v5` |
| `actions/setup-java` | 安装 Java 环境 | `uses: actions/setup-java@v4` |
| `actions/cache` | 缓存依赖，加速构建 | `uses: actions/cache@v4` |
| `actions/upload-artifact` | 上传构建产物 | `uses: actions/upload-artifact@v4` |
| `actions/download-artifact` | 下载构建产物 | `uses: actions/download-artifact@v4` |

---

## 🔐 安全相关

| Action | 用途 |
|--------|------|
| `github/codeql-action` | 代码安全扫描 |
| `aquasecurity/trivy-action` | 容器镜像漏洞扫描 |
| `actions/dependency-review-action` | 审查依赖安全性 |

---

## 🚀 部署相关

| Action | 用途 |
|--------|------|
| `peaceiris/actions-gh-pages` | 部署到 GitHub Pages |
| `docker/build-push-action` | 构建并推送 Docker 镜像 |
| `aws-actions/configure-aws-credentials` | 配置 AWS 凭证 |
| `azure/webapps-deploy` | 部署到 Azure Web App |
| `google-github-actions/deploy-cloud-functions` | 部署到 Google Cloud |

---

## 📦 发布相关

| Action | 用途 |
|--------|------|
| `softprops/action-gh-release` | 创建 GitHub Release |
| `changesets/action` | 管理版本和变更日志 |
| `marvinpinto/action-automatic-releases` | 自动发布 |
| `cycjimmy/semantic-release-action` | 语义化版本发布 |

---

## 🧪 测试与质量

| Action | 用途 |
|--------|------|
| `codecov/codecov-action` | 上传代码覆盖率报告 |
| `SonarSource/sonarqube-scan-action` | SonarQube 代码质量扫描 |
| `reviewdog/action-eslint` | ESLint 代码审查 |
| `stefanzweifel/git-auto-commit-action` | 自动提交格式化后的代码 |

---

## 🔔 通知

| Action | 用途 |
|--------|------|
| `slackapi/slack-github-action` | 发送 Slack 通知 |
| `8398a7/action-slack` | 更简单的 Slack 通知 |
| `appleboy/telegram-action` | 发送 Telegram 消息 |
| `peter-evans/create-or-update-comment` | 自动创建/更新 Issue 评论 |

---

## 📋 实用 Workflow 模板

### Python 项目自动测试

```yaml
name: Python Test

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      - run: pip install -r requirements.txt
      - run: pytest --cov=./
      - uses: codecov/codecov-action@v4
```

### 自动部署到 GitHub Pages

```yaml
name: Deploy Pages

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Build
        run: |
          mkdir public
          cp *.html public/
      - uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

### 定期任务（每6小时执行）

```yaml
name: Scheduled Task

on:
  schedule:
    - cron: '0 */6 * * *'  # 每6小时

jobs:
  run:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      - run: python scripts/daily_task.py
```

---

## 🔥 最受欢迎的 Actions（按使用量排名）

以下是在 GitHub Marketplace 上使用量最高的 Actions（截至 2026年）：

| 排名 | Action | 用途 | Star 数 |
|------|--------|------|---------|
| 🥇 | `actions/checkout` | 检出代码 | 6k+ |
| 🥈 | `actions/setup-node` | 安装 Node.js | 4k+ |
| 🥉 | `actions/setup-python` | 安装 Python | 3k+ |
| 4 | `actions/cache` | 缓存依赖加速构建 | 5k+ |
| 5 | `docker/build-push-action` | Docker 镜像构建推送 | 4k+ |
| 6 | `peaceiris/actions-gh-pages` | 部署 GitHub Pages | 5k+ |
| 7 | `github/codeql-action` | 代码安全扫描 | 3k+ |
| 8 | `softprops/action-gh-release` | 创建 Release | 3k+ |
| 9 | `codecov/codecov-action` | 上传覆盖率 | 2k+ |
| 10 | `stefanzweifel/git-auto-commit-action` | 自动提交 | 2k+ |

> 💡 **选择建议**：优先使用 Star 数高、更新活跃的官方或认证 Actions。

---

## 🔍 如何查找更多 Actions

1. 访问 [GitHub Actions Marketplace](https://github.com/marketplace?type=actions)
2. 搜索关键词（如 `docker`、`deploy`、`slack`）
3. 查看 Action 的：
   - ⭐ Star 数（越高越好）
   - 🔄 最近更新时间
   - 📖 使用文档
   - ✅ 是否为 **Verified Creator**（官方认证）

---

## 💡 使用技巧

### 1. 版本锁定

```yaml
# ✅ 推荐：锁定主版本
uses: actions/checkout@v4

# ✅ 更好：锁定具体 commit（最安全）
uses: actions/checkout@b4ffde65f46336ab88eb53be808477a3936bae11

# ❌ 避免：使用最新版
uses: actions/checkout@main  # 可能引入不兼容的更改
```

### 2. 善用 `with` 参数

```yaml
- uses: actions/setup-python@v5
  with:
    python-version: '3.12'
    cache: 'pip'  # 自动缓存 pip 依赖
```

### 3. 使用 `needs` 控制执行顺序

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps: [...]
  
  deploy:
    needs: test  # 只有 test 通过后才执行
    runs-on: ubuntu-latest
    steps: [...]
```

---

<p align="right">
  <a href="appendix-b-git-commands.md">← 附录B：Git 命令速查</a>
  &nbsp;|&nbsp;
  <a href="appendix-d-jekyll-themes.md">附录D：Jekyll 主题 →</a>
</p>
