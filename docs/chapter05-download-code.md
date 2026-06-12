# 第5章：下载代码（Clone & Download）📥

> 看到喜欢的开源项目，当然要下载来研究！本章介绍三种下载代码的方式。

## 📋 目录

- [三种下载方式对比](#三种下载方式对比)
- [方式1：Download ZIP（最简单）](#方式1download-zip最简单)
- [方式2：Git Clone（最常用）](#方式2git-clone最常用)
- [方式3：GitHub CLI（最强大）](#方式3github-cli最强大)
- [如何选择合适的下载方式](#如何选择合适的下载方式)
- [下载后的操作](#下载后的操作)
- [常见问题](#常见问题)

---

## 三种下载方式对比

| 方式 | 难度 | 能否同步更新 | 适用场景 |
|------|------|-------------|----------|
| **Download ZIP** | ⭐ 最简单 | ❌ 不能 | 快速查看代码，不需要修改 |
| **Git Clone** | ⭐⭐ 中等 | ✅ 能 | 长期学习、修改代码、贡献开源 |
| **GitHub CLI** | ⭐⭐⭐ 进阶 | ✅ 能 | 频繁操作 GitHub，需要自动化 |

---

## 方式1：Download ZIP（最简单）⭐

### 适用场景

- ✅ 只想快速查看代码
- ✅ 不需要修改和提交
- ✅ 不熟悉 Git 命令

### 操作步骤

**第1步**：进入仓库主页

```
打开浏览器 → 访问 https://github.com/用户名/仓库名
```

**第2步**：点击绿色的 **Code** 按钮

```
┌─────────────────────┐
│ 📁 Code  ▼          │  ← 点击这个按钮
└─────────────────────┘
```

**第3步**：选择 **Download ZIP**

```
弹出的菜单：
┌────────────────────────────────────┐
│ 🔗 Clone with HTTPS                │
│ 🔑 Clone with SSH                  │
│ 📱 Open with GitHub Desktop        │
│ 📦 Download ZIP  💾               │  ← 点击这个
│ 💻 Open with GitHub Codespaces    │
└────────────────────────────────────┘
```

**第4步**：解压 ZIP 文件

```bash
# 下载完成后，解压文件
unzip 仓库名-main.zip
cd 仓库名-main
```

### 优缺点

| 优点 | 缺点 |
|------|------|
| ✅ 不需要安装 Git | ❌ 不能同步原仓库的更新 |
| ✅ 操作简单 | ❌ 不能提交修改 |
| ✅ 适合新手 | ❌ 浪费存储空间（重复下载） |

---

## 方式2：Git Clone（最常用）⭐⭐

### 前置条件

需要先安装 Git：

```bash
# 检查是否安装了 Git
git --version

# 如果没有，去官网下载安装
# https://git-scm.com/
```

### 操作步骤

#### 步骤1：复制仓库地址

**方法A：HTTPS 地址（推荐新手）**

```
1. 点击绿色的 Code 按钮
2. 选择 Clone with HTTPS
3. 点击复制按钮（📋）复制地址

地址格式：
https://github.com/用户名/仓库名.git
```

**方法B：SSH 地址（推荐熟手）**

```
1. 点击绿色的 Code 按钮
2. 选择 Clone with SSH
3. 点击复制按钮

地址格式：
git@github.com:用户名/仓库名.git
```

> 💡 **区别**：
> - **HTTPS**：每次操作需要输入用户名和密码（或 Token）
> - **SSH**：配置一次 SSH Key 后，无需再输入密码（更安全、更方便）

#### 步骤2：执行 clone 命令

打开终端（命令行），执行：

```bash
# 基本格式
git clone 仓库地址

# 示例：克隆一个开源项目
git clone https://github.com/torvalds/linux.git

# 克隆到指定目录
git clone https://github.com/用户名/仓库名.git 我的目录名
```

#### 步骤3：进入项目目录

```bash
cd 仓库名

# 查看项目结构
ls -la  # macOS/Linux
dir      # Windows
```

### 同步原仓库的更新

如果你 Fork 了一个项目，或者原仓库有更新，可以同步：

```bash
# 1. 查看远程仓库
git remote -v

# 2. 添加原仓库为 upstream（只需执行一次）
git remote add upstream https://github.com/原作者/仓库名.git

# 3. 拉取原仓库的更新
git fetch upstream

# 4. 合并更新到你的分支
git merge upstream/main
```

---

## 方式3：GitHub CLI（最强大）⭐⭐⭐

### 什么是 GitHub CLI？

GitHub CLI（`gh`）是 GitHub 官方提供的命令行工具，让你可以：

- ✅ 克隆仓库
- ✅ 创建 Issue 和 PR
- ✅ 审查和合并 PR
- ✅ 管理 GitHub Actions

### 安装 GitHub CLI

#### macOS

```bash
# 使用 Homebrew
brew install gh

# 验证安装
gh --version
```

#### Windows

```bash
# 使用 Winget
winget install GitHub.cli

# 或使用 Chocolatey
choco install gh
```

#### Linux

```bash
# Ubuntu/Debian
sudo apt install gh

# Fedora
sudo dnf install gh
```

### 首次使用：登录 GitHub

```bash
# 登录命令
gh auth login

# 会问你一系列问题：
# 1. 要登录 GitHub.com 还是 GitHub Enterprise？→ 选择 GitHub.com
# 2. 想用 HTTPS 还是 SSH？→ 推荐 SSH（更安全）
# 3. 想用浏览器登录还是 Token？→ 推荐浏览器（最简单）
```

### 使用 gh 克隆仓库

```bash
# 基本格式
gh repo clone 用户名/仓库名

# 示例
gh repo clone torvalds/linux

# 克隆到当前目录
gh repo clone 用户名/仓库名 --  # 注意末尾的 --

# 只克隆最新版本（不克隆历史记录，速度更快）
gh repo clone 用户名/仓库名 -- --depth 1
```

### gh 的其他强大功能

```bash
# 创建 Issue
gh issue create --title "Bug报告" --body "详细描述"

# 查看 PR 列表
gh pr list

# 创建 PR
gh pr create --title "新功能" --body "说明"

# 克隆你自己的所有仓库
gh repo list --limit 1000 | while read -r repo _; do
    gh repo clone "$repo"
done
```

---

## 如何选择合适的下载方式

### 决策树

```
你想下载代码吗？
│
├─ 只想快速看看代码，不需要修改？
│   └─ 使用 Download ZIP ✅
│
├─ 想学习、修改代码，并可能贡献开源？
│   └─ 使用 Git Clone ✅
│
└─ 频繁操作 GitHub，需要自动化？
    └─ 使用 GitHub CLI ✅
```

### 实际场景推荐

| 场景 | 推荐方式 | 理由 |
|------|----------|------|
| 上课学习某个开源项目 | Download ZIP | 快速，不需要 Git |
| 准备给开源项目提交 PR | Git Clone | 需要提交修改 |
| 管理自己的多个仓库 | GitHub CLI | 批量操作更方便 |
| 在公司内网环境 | Git Clone (HTTPS) | 通常 SSH 端口被封 |

---

## 下载后的操作

### 1️⃣ 查看项目结构

```bash
# 查看文件列表
ls -la

# 查看项目说明文件（很重要！）
cat README.md

# 查看项目许可证
cat LICENSE

# 查看依赖文件（如果是 Node.js 项目）
cat package.json
```

### 2️⃣ 安装依赖

不同语言的项目有不同的依赖管理：

#### Python 项目

```bash
# 查看是否有 requirements.txt
cat requirements.txt

# 安装依赖
pip install -r requirements.txt

# 或者使用虚拟环境（推荐）
python -m venv venv
source venv/bin/activate  # macOS/Linux
venv\Scripts\activate     # Windows
pip install -r requirements.txt
```

#### Node.js 项目

```bash
# 查看 package.json
cat package.json

# 安装依赖
npm install

# 或者使用 yarn
yarn install
```

#### Java 项目

```bash
# 如果使用 Maven
mvn install

# 如果使用 Gradle
gradle build
```

### 3️⃣ 运行项目

```bash
# 查看 README.md，通常会有运行说明

# 示例：运行一个 Python 项目
python main.py

# 示例：运行一个 Node.js 项目
npm start

# 示例：运行一个 Java 项目
java -jar target/项目名.jar
```

---

## 常见问题

### Q1: clone 很慢怎么办？

**A**: 有几种方法可以加速：

**方法1：使用国内镜像**

```bash
# 将 github.com 替换为镜像地址
git clone https://github.com/用户名/仓库名.git
# 改为：
git clone https://hub.fastgit.xyz/用户名/仓库名.git
```

> ⚠️ **注意**：镜像站点可能不是官方运营，请勿用于敏感项目。

**方法2：只克隆最新版本**

```bash
# 不克隆完整历史记录，只克隆最新版本
git clone --depth 1 https://github.com/用户名/仓库名.git
```

**方法3：使用代理**

```bash
# 配置 Git 使用代理
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```

### Q2: 下载的代码如何更新到最新版本？

**A**: 如果你是用 `git clone` 下载的，可以：

```bash
# 进入项目目录
cd 仓库名

# 拉取最新代码
git pull origin main
```

如果你是用 Download ZIP 下载的，只能重新下载。

### Q3: 我可以下载私有仓库吗？

**A**: 可以，但需要权限：

- **方法1**：所有者给你添加为协作者
- **方法2**：组织给你分配访问权限
- **方法3**：使用 Token（需要有 `repo` 权限）

```bash
# 使用 Token 克隆私有仓库
git clone https://你的用户名:你的token@github.com/用户名/仓库名.git
```

### Q4: fork 和 clone 有什么区别？

| 操作 | 作用 | 位置 |
|------|------|------|
| **Fork** | 复制别人的仓库到你的 GitHub 账号 | GitHub 云端 |
| **Clone** | 下载仓库到你的本地电脑 | 本地电脑 |

**典型流程**：
1. Fork 别人的仓库（在 GitHub 上复制一份）
2. Clone 你 Fork 的仓库（下载到本地）
3. 修改代码
4. Push 到你的 Fork
5. 提交 PR 到原仓库

### Q5: 下载的代码可以商用吗？

**A**: 取决于项目的开源协议（LICENSE 文件）：

| 协议 | 可以商用吗？ | 需要注明作者吗？ |
|------|-------------|----------------|
| MIT | ✅ 可以 | ✅ 需要 |
| GPL | ✅ 可以 | ✅ 需要，且你的代码也要开源 |
| Apache 2.0 | ✅ 可以 | ✅ 需要 |
| 没有 LICENSE | ❌ 不可以（默认版权保护） | - |

> 💡 **建议**：使用前一定要查看 LICENSE 文件！

---

## 📝 实战练习

### 练习1：下载你的第一个开源项目

**任务**：
1. 找一个你感兴趣的开源项目（比如 [Vue.js](https://github.com/vuejs/core)）
2. 用三种方式分别下载
3. 对比文件大小和下载时间

### 练习2：同步原仓库的更新

**任务**：
1. Fork 一个项目
2. Clone 你的 Fork 到本地
3. 配置 upstream
4. 拉取原仓库的更新

### 练习3：使用 GitHub CLI

**任务**：
1. 安装 GitHub CLI
2. 登录你的账号
3. 用 `gh` 命令克隆一个仓库
4. 用 `gh` 命令查看你的所有仓库

---

## 📚 延伸阅读

- [第7章：分支管理](chapter07-branch-management.md) - 下载后如何创建分支修改代码
- [第9章：Fork 与 Pull Request](chapter09-fork-pr.md) - 如何给开源项目贡献代码
- [第16章：开源协议选择](chapter16-license-choice.md) - 了解下载的代码可以怎么用

---

## 📝 本章小结

你现在学会了：

- ✅ 三种下载代码的方式及其适用场景
- ✅ 如何使用 Git Clone 下载代码
- ✅ 如何同步原仓库的更新
- ✅ 如何使用 GitHub CLI 提高效率
- ✅ 下载后如何安装依赖和运行项目

### 下一步学习

- 想修改下载的代码？去 [第6章：提交更改](chapter06-commit-changes.md)
- 想给开源项目贡献代码？去 [第9章：Fork 与 PR](chapter09-fork-pr.md)
- 想了解开源协议？去 [第16章：开源协议](chapter16-license-choice.md)

---

<p align="right">
  <a href="chapter04-first-repository.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter06-commit-changes.md">下一章 →</a>
</p>