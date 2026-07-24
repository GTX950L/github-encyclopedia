# 第29章：GitHub Codespaces — 云开发环境 ☁️

> 想在浏览器里写代码？不想配置本地开发环境？GitHub Codespaces 让你在云端开发！

## 📋 目录

- [什么是 Codespaces？](#什么是-codespaces)
- [创建第一个 Codespace](#创建第一个-codespace)
- [开发环境配置](#开发环境配置)
- [实用技巧](#实用技巧)
- [常见问题](#常见问题)

---

## 什么是 Codespaces？

### 基本定义

**GitHub Codespaces** 是 GitHub 提供的**云端开发环境**。

简单说：
- 🖥️ 在浏览器里打开 VS Code
- 📦 环境已预装好依赖
- ⚡ 高性能云端机器
- 💾 代码保存在云端

### 比喻

```
传统开发：
你的电脑 → 装系统 → 装编辑器 → 装语言环境 → 装依赖 → 开始写代码
                                            ↑ 这一步可能花一天...

Codespaces：
点一下按钮 → 浏览器打开 VS Code → 开始写代码！
                                            ↑ 这一步只要 10 秒！
```

### 适合谁？

| 场景 | 适合？ | 原因 |
|------|--------|------|
| **学生/新手** | ✅ 强烈推荐 | 不用配置环境，直接开始写代码 |
| **多设备开发者** | ✅ 推荐 | 在公司电脑、个人电脑、平板上都能开发 |
| **团队协作** | ✅ 适合 | 所有人都用同一个环境，告别"我电脑上能运行" |
| **临时贡献开源** | ✅ 非常适合 | 不用 clone 到本地，直接在云端改代码 |
| **游戏开发** | ❌ 不推荐 | 需要 GPU 和大量存储 |

---

## 创建第一个 Codespace

### 方法1：从仓库创建（最常用）

```
1. 打开一个仓库（如 https://github.com/GTX950L/github-encyclopedia）
2. 点击绿色的 "Code" 按钮
3. 选择 "Codespaces" 标签
4. 点击 "Create codespace on main"

等待 10-30 秒...
浏览器里会出现 VS Code 界面！🎉
```

### 方法2：从模板创建

```
1. 访问 https://github.com/codespaces
2. 点击 "New codespace"
3. 选择模板（如：React、Python、Node.js）
4. 点击 "Create codespace"
```

### 第一次使用

**你会看到的界面**：

```
┌─────────────────────────────────────────┐
│  ［文件］［编辑］［选择］［查看］...     │
│ ┌──────────┬──────────────────────────┐ │
│ │ 📁 项目   │ 欢迎！                     │ │
│ │ ├── src   │ >>> print("Hello!")       │ │
│ │ ├── docs  │ Hello!                    │ │
│ │ └── tests │                           │ │
│ │           │ 终端在底部                  │ │
│ │           │ $ pip install flask        │ │
│ └──────────┴──────────────────────────┘ │
│ │ 终端 │ 输出 │ 问题 │                   │ │
└─────────────────────────────────────────┘
```

### 在 Codespace 中工作

```bash
# 打开终端（Ctrl+\`）
# 查看当前环境
python --version
node --version
git --version

# 安装依赖
pip install -r requirements.txt

# 运行代码
python main.py

# 提交代码
git add .
git commit -m "在 Codespace 中修改"
git push
```

---

## 开发环境配置

### 默认配置

Codespaces 默认提供了这些工具：
- 🐍 Python、Node.js、Go、Java、C++ 等常用语言
- 🐙 Git、GitHub CLI
- 🐳 Docker
- 📦 包管理器（pip、npm、yarn、apt）

### 自定义配置：devcontainer.json

如果你需要特定的环境配置，可以在仓库中添加 `devcontainer.json`：

```json
{
  "name": "Python Data Science",
  "image": "mcr.microsoft.com/devcontainers/python:3.12",

  // 安装扩展
  "extensions": [
    "ms-python.python",
    "ms-python.vscode-pylance",
    "ms-toolsai.jupyter"
  ],

  // 设置
  "settings": {
    "python.defaultInterpreterPath": "/usr/local/bin/python"
  },

  // 自动安装依赖
  "postCreateCommand": "pip install -r requirements.txt",

  // 暴露端口
  "forwardPorts": [5000, 8080]
}
```

### 常用 devcontainer 配置模板

| 项目类型 | 推荐配置 |
|---------|---------|
| **Python 项目** | `mcr.microsoft.com/devcontainers/python` |
| **Node.js 项目** | `mcr.microsoft.com/devcontainers/javascript-node` |
| **Go 项目** | `mcr.microsoft.com/devcontainers/go` |
| **Java 项目** | `mcr.microsoft.com/devcontainers/java` |
| **Rust 项目** | `mcr.microsoft.com/devcontainers/rust` |
| **Docker 项目** | `mcr.microsoft.com/devcontainers/docker-from-docker` |
| **通用开发** | `mcr.microsoft.com/devcontainers/universal` |

> 💡 文件位置：在仓库根目录创建 `.devcontainer/devcontainer.json`

---

### 🚀 高级 devcontainer 配置

#### 1️⃣ Dockerfile 自定义镜像

如果预置镜像不满足需求，可以用 Dockerfile 完全自定义环境：

```json
// .devcontainer/devcontainer.json
{
  "name": "Custom Python Environment",
  "build": {
    "dockerfile": "Dockerfile",
    "args": {
      "VARIANT": "3.12"
    }
  },
  "extensions": ["ms-python.python"],
  "postCreateCommand": "pip install -r requirements.txt"
}
```

配套的 Dockerfile：

```dockerfile
# .devcontainer/Dockerfile
ARG VARIANT=3.12
FROM mcr.microsoft.com/devcontainers/python:${VARIANT}

# 安装系统级依赖
RUN apt-get update && apt-get install -y \
    postgresql-client \
    redis-tools \
    && rm -rf /var/lib/apt/lists/*

# 安装全局 Python 工具
RUN pip install --upgrade pip poetry
```

#### 2️⃣ Docker Compose 多容器

前后端分离或需要数据库时，定义多容器环境：

```json
// .devcontainer/devcontainer.json
{
  "name": "Web App with Database",
  "dockerComposeFile": "docker-compose.yml",
  "service": "app",
  "workspaceFolder": "/workspace",
  "extensions": ["ms-python.python", "mtxr.sqltools"]
}
```

配套的 docker-compose.yml：

```yaml
# .devcontainer/docker-compose.yml
version: "3.8"
services:
  app:
    image: mcr.microsoft.com/devcontainers/python:3.12
    volumes:
      - ..:/workspace:cached
    command: sleep infinity
    depends_on:
      - db

  db:
    image: postgres:16
    environment:
      POSTGRES_USER: app
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: myapp
    ports:
      - "5432:5432"
```

#### 3️⃣ Devcontainer Features（开箱即用的功能）

Features 是预构建的功能模块，可以直接添加到 devcontainer 配置中：

```json
{
  "name": "Full Stack Dev",
  "image": "mcr.microsoft.com/devcontainers/universal:2",
  "features": {
    "ghcr.io/devcontainers/features/docker-in-docker:2": {},
    "ghcr.io/devcontainers/features/node:1": {
      "version": "22"
    },
    "ghcr.io/devcontainers/features/github-cli:1": {}
  }
}
```

**常用 Features**：
| Feature | 用途 |
|---------|------|
| `docker-in-docker` | 在容器内使用 Docker |
| `node` | 安装指定版本 Node.js |
| `github-cli` | 预装 gh 命令行 |
| `aws-cli` | AWS 命令行工具 |
| `terraform` | 基础设施即代码 |
| `sshd` | SSH 远程连接 |

#### 4️⃣ 生命周期钩子

不同阶段自动执行命令：

```json
{
  "name": "Auto Setup",
  "image": "mcr.microsoft.com/devcontainers/python:3.12",
  // 创建容器后执行（一次）
  "postCreateCommand": "pip install -r requirements.txt && pre-commit install",
  // 容器启动后执行（每次）
  "postStartCommand": "git pull --rebase",
  // 附加到容器后执行
  "postAttachCommand": "echo 'Ready!'"
}
```

> 💡 **原理**：`postCreateCommand` 只在首次创建时运行，`postStartCommand` 每次启动运行。

## 实用技巧

### 1. 端口转发

如果项目运行了一个 Web 服务（如 Flask/Django），Codespaces 会自动帮你转发端口：

```python
# Flask 应用
app.run(port=5000)
```

终端会显示：
```
➡️  你的应用运行在 http://localhost:5000
🌐 Codespaces 转发到 https://xxx-xxxx-xxxxx-5000.preview.app.github.dev
```

> 这样其他人也能通过公网 URL 访问你的应用！

### 2. 为开源项目贡献代码

Codespaces 让贡献开源变得极其简单：

```
1. 打开项目的 GitHub 仓库
2. 点击 "Code" → "Codespaces"
3. 按 `.` 键直接在浏览器中打开 VS Code
4. 修改代码
5. 在终端中创建 PR：
   git checkout -b fix/bug
   git add .
   git commit -m "fix: 解决登录 Bug"
   git push
6. 返回 GitHub 页面，创建 PR
```

### 3. 多设备同步

- ✅ 在公司用 Codespaces 开发
- ✅ 回到家用同一台 Codespace 继续开发
- ✅ 所有文件保存在云端，无需同步

### 4. 使用 VS Code 本地连接

如果你更喜欢本地 VS Code，也可以连接远程 Codespace：

```
1. 安装 "GitHub Codespaces" 扩展
2. 在 VS Code 中打开命令面板（Ctrl+Shift+P）
3. 输入 "Codespaces: Connect to..."
4. 选择你的 Codespace
5. VS Code 本地界面连接到云端环境！
```

### 5. 重启和恢复

Codespace 在闲置 30 分钟后会自动停止（节省配额）。
重新使用时：

```
1. 访问 https://github.com/codespaces
2. 找到你的 Codespace
3. 点击 "Connect"
4. 之前的修改还在！继续工作
```

---

## 常见问题

### Q1: Codespaces 免费吗？

**A**: 

| 账号类型 | 每月免费额度 | 超过后的价格 |
|---------|-------------|------------|
| **Free** | 120 小时（2 核）或 60 小时（4 核） | $0.18/小时（2 核） |
| **Pro** | 180 小时（2 核） | $0.18/小时（2 核） |
| **学生包** | 180 小时 + $100 信用额度 | 相当多免费时间 |

**节省额度的方法**：
- ✅ 不用时停止 Codespace（自动 30 分钟停止）
- ✅ 选择 2 核机器（足够日常开发）
- ✅ 删除不再使用的 Codespace

### Q2: 可以用手机/iPad 开发吗？

**A**: ✅ **可以！**

```
方法1：使用浏览器
├── 在 Codespace 页面点击连接
└── 在浏览器中打开 VS Code 界面

方法2：使用 iPad
├── 下载 VS Code for iPad
├── 安装 GitHub Codespaces 扩展
└── 连接你的 Codespace
```

### Q3: Codespace 的数据安全吗？

**A**: ✅ **安全**。

- 🔒 **加密存储**：所有数据在 GitHub 云端加密
- 🔒 **私有仓库**：只有你和协作者能访问
- 🔒 **自动备份**：代码每次 push 都保存在 GitHub 仓库

> ⚠️ 唯一的风险：如果你删除了 Codespace，云端的临时修改会丢失。**建议经常 git push**。

### Q4: Codespace 能跑 GPU 吗？

**A**: ⚠️ **目前不行**。

Codespaces 提供的都是 CPU 机器。如果需要 GPU（如训练 AI 模型），可以：
- 使用 GitHub Actions（有 GPU runner）
- 使用 Google Colab（免费 GPU）
- 使用自己的 GPU 服务器

---

## 📝 实战练习

### 练习1：创建你的第一个 Codespace

**任务**：
1. 打开你最喜欢的 GitHub 仓库
2. 点击 "Code" → "Codespaces" → 创建
3. 等待环境启动
4. 在终端运行项目

### 练习2：配置 devcontainer

**任务**：
1. 为你的项目创建 `.devcontainer/devcontainer.json`
2. 配置需要的扩展和依赖
3. 重新创建 Codespace，验证配置生效

### 练习3：在 Codespace 中贡献开源

**任务**：
1. 找一个你感兴趣的开源项目
2. 用 Codespace 打开它
3. 修复一个文档错别字
4. 提交 PR

---

## 📚 延伸阅读

- [第20章：GitHub Pages 搭建网站](chapter20-pages-website.md) - 部署你的网页应用
- [第19章：GitHub Actions 入门](chapter19-actions-intro.md) - CI/CD 自动化
- [Coespace 官方文档](https://docs.github.com/en/codespaces)
- [devcontainer 配置参考](https://containers.dev/)

---

## 📝 本章小结

你现在学会了：

- ✅ 什么是 Codespaces，为什么需要它
- ✅ 如何创建和使用 Codespace
- ✅ 如何自定义开发环境（devcontainer）
- ✅ 实用技巧（端口转发、多设备、开源贡献）
- ✅ 成本和配额管理

### 💡 核心要点

> **Codespaces 让开发环境配置成为历史。**
> 
> 告别"在我电脑上能运行"的问题，让团队所有人都使用相同的环境。

---

<p align="right">
  <a href="chapter28-best-practices.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter30-discussions.md">下一章 →</a>
</p>
