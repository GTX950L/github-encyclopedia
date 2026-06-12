# 第4章：第一个仓库 📁

> 仓库（Repository）是 GitHub 的核心概念。本章手把手教你创建、配置和删除仓库。

## 📋 目录

- [什么是仓库？](#什么是仓库)
- [创建第一个仓库](#创建第一个仓库)
- [仓库的基本配置](#仓库的基本配置)
- [上传第一个文件](#上传第一个文件)
- [删除仓库](#删除仓库)
- [常见问题](#常见问题)

---

## 什么是仓库？

### 基本定义

**仓库（Repository）** 就是一个项目的文件夹，但带有"超能力"：

```
普通文件夹：
my-project/
├── index.html
├── style.css
└── script.js

GitHub 仓库：
my-project/          ← 普通文件夹
├── index.html
├── style.css
├── script.js
├── README.md        ← GitHub 会显示这个文件
├── LICENSE         ← 开源协议
└── .git/           ← Git 的"大脑"，记录所有历史
```

### 仓库的类型

| 类型 | 谁能看？ | 适合什么？ |
|------|---------|----------|
| **Public（公开）** | 所有人 | 开源项目、个人作品集 |
| **Private（私有）** | 只有你和你邀请的人 | 公司代码、未发布的项目 |

---

## 创建第一个仓库

### 方法1：在网页上创建（推荐新手）✅

#### 步骤详解

**第1步：进入创建页面**

```
方法1：点击右上角 + 号 → New repository
方法2：直接访问 https://github.com/new
```

**第2步：填写仓库信息**

```
┌─────────────────────────────────────────────┐
│ Repository name *                        │  ← 仓库名（必需）
│ my-first-repo                        │
├─────────────────────────────────────────────┤
│ Description                               │  ← 项目描述（可选）
│ 我的第一个 GitHub 仓库                   │
├─────────────────────────────────────────────┤
│ ○ Public    ● Private                    │  ← 选择公开或私有
├─────────────────────────────────────────────┤
│ ✅ Add a README file                    │  ← 强烈建议勾选！
│ ✅ Add .gitignore                      │  ← 选择语言模板
│ ✅ Choose a license                    │  ← 选择开源协议
└─────────────────────────────────────────────┘
```

#### 各字段详解

**Repository name（仓库名）**

| 要求 | 说明 |
|------|------|
| 必需 | 不能为空 |
| 唯一 | 不能和你的其他仓库重名 |
| 规则 | 只能包含字母、数字、横杠(-)、下划线(_) |
| 建议 | 简短、有意义、使用小写 |

> ✅ **好的仓库名**：
> - `my-blog`（我的博客）
> - `todo-app`（待办应用）
> - `leetcode-solutions`（力扣题解）
>
> ❌ **不好的仓库名**：
> - `asdfghjkl`（无意义）
> - `我的第一个项目`（包含中文，不推荐）
> - `a-very-long-repository-name-that-is-hard-to-remember`（太长）

**Description（描述）**

用一两句话说明项目是做什么的。

**示例**：
- `A simple todo app built with React`
- `我的个人博客，使用 Vue.js 和 Markdown`
- `LeetCode 刷题记录（Python 版）`

---

### 方法2：使用 GitHub CLI 创建

```bash
# 基本格式
gh repo create 仓库名 --public --source=. --push

# 示例：创建并推送本地项目
cd my-project
git init
git add .
git commit -m "Initial commit"
gh repo create my-project --public --source=. --push
```

---

## 仓库的基本配置

### 初始化文件详解

#### README.md

**作用**：项目的"门面"，是别人了解你项目的第一个窗口。

**GitHub 会自动显示 README.md 的内容在仓库首页**。

**基本结构**：

```markdown
# 项目名

简短的项目描述。

## 特性

- 特性1
- 特性2

## 快速开始

安装和使用的步骤。

## 贡献

欢迎贡献！

## 许可证

MIT License
```

> 📝 **详细写法**见 [第15章：README 编写艺术](chapter15-readme-art.md)

---

#### .gitignore

**作用**：告诉 Git 哪些文件不需要版本控制。

**为什么需要？**

```
❌ 不要提交的文件：
├── node_modules/     ← 依赖包，太大且可以重新安装
├── .env              ← 环境变量，可能包含密钥！
├── dist/ 或 build/   ← 编译产物，可以重新生成
└── *.log             ← 日志文件，没意义
```

**如何创建？**

**方法1：在创建仓库时勾选**

```
创建仓库页面 → Add .gitignore → 选择语言

例如：选择 Node.js
GitHub 会自动生成适合 Node.js 的 .gitignore
```

**方法2：手动创建**

在项目根目录创建 `.gitignore` 文件，写入：

```gitignore
# 依赖
node_modules/

# 编译产物
dist/
build/

# 环境变量
.env
.env.local

# 日志
*.log

# 系统文件
.DS_Store  # macOS
Thumbs.db  # Windows
```

---

#### LICENSE

**作用**：告诉别人可以如何使用你的代码。

**为什么需要？**

```
没有 LICENSE = 默认版权保护
→ 别人不能复制、修改、分发你的代码！
```

**如何选择协议？**

| 协议 | 允许商用 | 需要署名 | 要求开源修改 |
|------|----------|----------|----------------|
| **MIT** | ✅ | ✅ | ❌ |
| **Apache 2.0** | ✅ | ✅ | ❌ |
| **GPL v3** | ✅ | ✅ | ✅ |

> 📝 **详细说明**见 [第16章：开源协议选择](chapter16-license-choice.md)

---

## 上传第一个文件

### 方法1：在网页上直接上传（最简单）⭐

**适用场景**：快速上传一两个文件，不需要命令行。

#### 步骤：

```
1. 进入仓库页面
2. 点击 "Add file" → "Upload files"
3. 拖拽文件到页面，或点击 "choose your files"
4. 填写提交信息（Commit message）
5. 点击 "Commit changes"
```

**提交信息（Commit message）的格式**：

```
✅ 好的提交信息：
"Add README.md"
"Update index.html: fix login bug"
"Add: user authentication feature"

❌ 不好的提交信息：
"update"（太模糊）
"asdf"（无意义）
"改了一下"（使用中文，且模糊）
```

---

### 方法2：使用 Git 命令行上传（最常用）⭐⭐

#### 前置条件

需要先安装 Git：https://git-scm.com/

#### 步骤：

**第1步：克隆仓库到本地**

```bash
# 复制仓库地址（HTTPS 或 SSH）
git clone https://github.com/你的用户名/仓库名.git

# 进入项目目录
cd 仓库名
```

**第2步：添加文件**

```bash
# 创建或复制文件到目录
echo "# My Project" > README.md

# 查看状态
git status
# 会显示： Untracked files: README.md
```

**第3步：提交更改**

```bash
# 添加文件到暂存区
git add README.md

# 或者添加所有文件
git add .

# 提交
git commit -m "Initial commit"
```

**第4步：推送到 GitHub**

```bash
git push origin main
```

---

## 删除仓库

### ⚠️ 警告：删除是不可逆的！

删除仓库后：
- ❌ 所有代码都会消失
- ❌ 所有 Issue 和 PR 都会消失
- ❌ Star 和 Fork 数都会消失
- ❌ **无法恢复！**

### 删除步骤

```
1. 进入仓库页面
2. 点击 "Settings"
3. 滚动到最底部
4. 点击 "Delete this repository"
5. 输入仓库名确认
6. 点击 "I understand the consequences, delete this repository"
```

---

## 常见问题

### Q1: 仓库名可以改吗？

**A**: ✅ **可以！**

**如何修改？**

```
Settings → Repository name → 输入新名字 → Update
```

**注意**：
- 修改后，旧地址会自动重定向到新地址（暂时）
- 但建议在 README 中更新所有链接

---

### Q2: 公开仓库可以改成私有吗？

**A**: ✅ **可以！**

```
Settings → Danger Zone → Change visibility → Make private
```

**注意**：
- 改成私有后，别人就不能 Fork 或 Star 了
- 如果再改回公开，Star 数会保留，但 Fork 数会重置

---

### Q3: 我可以删除别人的仓库吗？

**A**: ❌ **不可以！**

你只能删除自己创建的仓库，或者你有权限管理的组织仓库。

---

### Q4: 一个账号可以创建多少个仓库？

**A**: **无限！**

GitHub 免费版允许创建无限个公开和私有仓库。

---

## 📝 实战练习

### 练习1：创建你的第一个仓库

**任务**：
1. 登录 GitHub
2. 创建一个公开仓库，名字叫 `my-first-repo`
3. 勾选 "Add a README file"
4. 在 README 中写一句欢迎语
5. 提交

---

### 练习2：上传文件

**任务**：
1. 在本地创建一个 `hello.py` 文件
2. 使用 `git clone` 下载仓库
3. 把 `hello.py` 放进去
4. 提交并推送

---

### 练习3：删除测试仓库

**任务**：
1. 创建一个测试仓库 `test-delete`
2. 确认删除流程
3. 删除它（小心别删错！）

---

## 📚 延伸阅读

- [第5章：下载代码](chapter05-download-code.md) - 如何下载别人的仓库
- [第6章：提交更改](chapter06-commit-changes.md) - Git 提交的详细讲解
- [第15章：README 编写艺术](chapter15-readme-art.md) - 如何写出吸引人的 README

---

## 📝 本章小结

你现在学会了：

- ✅ 什么是仓库，以及公开 vs 私有的区别
- ✅ 如何在网页上创建仓库
- ✅ 仓库的基本配置（README、.gitignore、LICENSE）
- ✅ 如何上传文件（网页和命令行两种方式）
- ✅ 如何删除仓库（以及警告）

### 下一步学习

- 想下载别人的代码？去 → [第5章：下载代码](chapter05-download-code.md)
- 想学习 Git 提交？去 → [第6章：提交更改](chapter06-commit-changes.md)

---

<p align="right">
  <a href="chapter03-interface-guide.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter05-download-code.md">下一章 →</a>
</p>
