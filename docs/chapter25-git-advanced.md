# 第25章：Git 高级命令 🔧

> 熟练使用 Git 高级命令，让你在复杂场景下也能游刃有余。

## 📋 目录

- [Rebase（变基）](#rebase变基)
- [Cherry-pick（摘取提交）](#cherry-pick摘取提交)
- [Bisect（二分查找）](#bisect二分查找)
- [Reflog（引用日志）](#reflog引用日志)
- [Submodule（子模块）](#submodule子模块)
- [常见问题](#常见问题)

---

## Rebase（变基）

### 什么是 Rebase？

**Rebase** 是把一个分支的修改"移动"到另一个分支的顶端。

### 比喻

```
变基前：
main:      A --- B
feature:         --- C --- D

变基后（把 feature 变基到 main）：
main:      A --- B
feature:             --- C' --- D'
```

> 💡 **Rebase vs Merge**：
> - Merge 会创建一个合并提交（历史有分叉）
> - Rebase 会移动提交（历史是线性的）

---

### 基本操作

```bash
# 第1步：切换到功能分支
git switch feature/login

# 第2步：变基到 main
git rebase main

# 如果有冲突，解决后：
git add .
git rebase --continue

# 第3步：切换回 main，合并
git switch main
git merge feature/login  # 现在会是快进合并
```

---

### ⚠️ Rebase 的黄金规则

> **不要对已经推送到远程的分支做 Rebase！**

```
❌ 错误做法：
│
├── 你推送了 feature 分支到远程
├── 你对 feature 做了 rebase
├── 你强制推送 git push --force
└── 同事的本地分支和远程不一致了！

✅ 正确做法：
│
├── 只对本地分支做 rebase
└── 已经推送的分支，用 merge 而不是 rebase
```

---

## Cherry-pick（摘取提交）🍒

### 什么是 Cherry-pick？

**Cherry-pick** 是把某个分支的单个提交，"摘取"到当前分支。

### 使用场景

```
场景：你在 feature/login 分支做了 3 个提交
│
├── 提交 A：修复登录 Bug（紧急！）
├── 提交 B：添加登录页面
└── 提交 C：重构登录逻辑

问题：提交 A 很紧急，需要立即合并到 main
解决：使用 cherry-pick 只摘取提交 A！
```

---

### 基本操作

```bash
# 第1步：查看要摘取的提交 ID
git log --oneline feature/login
# 输出：
# a1b2c3d (feature/login) 重构登录逻辑
# e4f5g6h 添加登录页面
# i7j8k9l 修复登录 Bug  ← 我们要这个

# 第2步：切换回 main
git switch main

# 第3步：摘取提交
git cherry-pick i7j8k9l

# 第4步：推送
git push origin main
```

---

## Bisect（二分查找）🔍

### 什么是 Bisect？

**Bisect** 是用二分法查找"哪个提交引入了 Bug"。

### 使用场景

```
场景：项目上周还能运行，今天突然炸了！
│
├── 有 100+ 个提交
├── 不知道是哪个提交引入的 Bug
└── 手动检查太慢了...

解决：使用 git bisect 自动二分查找！
```

---

### 基本操作

```bash
# 第1步：开始二分查找
git bisect start

# 第2步：标记当前提交为"有 Bug"
git bisect bad

# 第3步：标记一个已知"没 Bug"的提交
git bisect good v1.0.0

# Git 会自动跳到中间的提交
# 输出：Bisecting: 5 revisions left to test after this

# 第4步：测试当前提交
# 如果当前提交有 Bug：
git bisect bad

# 如果当前提交没 Bug：
git bisect good

# Git 会继续二分，直到找到第一个"有 Bug"的提交

# 第5步：查看结果
# 输出：
# i7j8k9l is the first bad commit
# Author: XXX
# Date: XXX
# 
# fix: 重构用户模块  ← 就是这个提交引入了 Bug！

# 第6步：结束二分查找
git bisect reset
```

---

### 自动化 Bisect ✅ 推荐！

```bash
# 写一个测试脚本 test.sh
#!/bin/bash
npm test  # 如果测试通过，返回 0；否则返回 1

# 让 Git 自动运行二分查找
git bisect run ./test.sh

# Git 会自动找到第一个"测试不通过"的提交
```

---

## Reflog（引用日志）📜

### 什么是 Reflog？

**Reflog** 是 Git 的"黑匣子"，记录所有分支指针的移动。

### 为什么需要？

```
场景：你不小心删除了一个分支...
│
├── git branch -D feature/login
└── 完了！分支没了...

解决：使用 git reflog 找回！
```

---

### 基本操作

```bash
# 查看引用日志
git reflog

# 输出：
# i7j8k9l HEAD@{0}: commit: fix: 修复登录 Bug
# a1b2c3d HEAD@{1}: commit: 添加登录页面
# e4f5g6h HEAD@{2}: checkout: moving from feature/login to main
# ...

# 找回删除的分支
git branch feature/login i7j8k9l

# 成功！分支恢复了
```

---

### 恢复错误的 Reset ✅

```bash
# 场景：你错误地 reset --hard 了
git reset --hard HEAD~3  # 删除了最近 3 个提交

# 别慌！用 reflog 找回
git reflog
# 找到 reset 前的提交 ID

git reset --hard i7j8k9l  # 恢复！
```

---

## Worktree（工作树）🌳

### 什么是 Worktree？

**Worktree** 允许你在同一个仓库中**同时检出多个分支**到不同的目录。

### 为什么需要？

```
场景：你正在 feature 分支上开发新功能
突然发现 main 分支有个紧急 Bug 要修...
│
├── ❌ 旧办法：
│   ├── 暂存当前修改（git stash）
│   ├── 切到 main 分支
│   ├── 修复 Bug
│   ├── 切回 feature 分支
│   └── 恢复暂存（git stash pop）
│
└── ✅ 用 Worktree：
    ├── git worktree add ../fix-bug main
    ├── 在另一个目录修复 Bug
    └── 修完后删除 worktree
    → 你的 feature 分支纹丝不动！
```

### 基本操作

```bash
# 创建一个 worktree（在另一目录检出 main 分支）
git worktree add ../项目名-fix-bug main

# 创建基于当前分支的 worktree
git worktree add ../项目名-feature feature/login

# 列出所有 worktree
git worktree list

# 输出：
# /path/main-project        (main)
# /path/main-project-fix    (abc1234) [fix/critical-bug]
# /path/main-project-feat   (def5678) [feature/login]

# 使用完后删除 worktree
git worktree remove ../项目名-fix-bug

# 清理已经删除的 worktree 记录
git worktree prune
```

> 💡 **最佳实践**：Worktree 名称建议用 `{项目名}-{分支名}` 的格式，一眼就能看出哪个 worktree 对应哪个分支。

---

## Submodule（子模块）📦

### 什么是 Submodule？

**Submodule** 允许你在一个仓库中嵌入另一个仓库。

### 使用场景

```
场景：你的项目依赖一个第三方库
│
├── ❌ 直接复制代码：无法同步上游更新
├── ✅ 使用 Submodule：可以同步更新
└── 适合：依赖的库也在开发中
```

---

### 基本操作

#### 添加 Submodule

```bash
# 添加子模块
git submodule add https://github.com/别人的库.git libs/别人的库

# 提交
git add .gitmodules libs/别人的库
git commit -m "Add: 添加子模块"
```

---

#### 克隆包含 Submodule 的仓库

```bash
# 方法1：克隆时自动初始化子模块
git clone --recurse-submodules https://github.com/你的项目.git

# 方法2：克隆后手动初始化
git clone https://github.com/你的项目.git
cd 你的项目
git submodule init
git submodule update
```

---

#### 更新 Submodule

```bash
# 进入子模块目录
cd libs/别人的库

# 拉取最新代码
git pull origin main

# 回到主仓库，提交子模块的更新
cd ../..
git add libs/别人的库
git commit -m "Update: 更新子模块"
```

---

## 常见问题

### Q1: Rebase 和 Merge 该用哪个？

**A**: 取决于你的团队规范：

| 场景 | 推荐 |
|------|----------|
| **个人项目** | Rebase（保持历史干净） |
| **团队项目** | Merge（保留完整历史） |
| **公开分支** | ❌ 不要用 Rebase |
| **本地分支** | ✅ 可以用 Rebase |

---

### Q2: Cherry-pick 会导致重复提交吗？

**A**: 不会。

```
Cherry-pick 会创建一个新的提交（有不同的 ID）
│
├── 原提交：i7j8k9l
└── 摘取后的提交：m1n2o3p（内容相同，但 ID 不同）
```

---

### Q3: Bisect 会修改历史吗？

**A**: ❌ **不会！**

Bisect 只是"检查"提交，不会修改任何东西。

---

## 📝 实战练习

### 练习1：使用 Rebase 保持历史干净

**任务**：
1. 创建 feature 分支，做几个提交
2. 对 main 分支做新提交
3. 对 feature 分支做 rebase
4. 查看历史，确认是线性的

---

### 练习2：使用 Cherry-pick 摘取提交

**任务**：
1. 在 feature 分支做一个紧急修复提交
2. 摘取到 main 分支
3. 确认 main 分支包含了这个修复

---

### 练习3：使用 Bisect 查找 Bug

**任务**：
1. 故意在某个提交引入 Bug
2. 使用 git bisect 查找
3. 确认找到了正确的提交

---

## 📚 延伸阅读

- [第7章：分支管理](chapter07-branch-management.md) - 分支的基本操作
- [第8章：合并代码](chapter08-merge-code.md) - Merge 详解
- [第28章：企业级最佳实践](chapter28-best-practices.md) - Git 工作流规范

---

## 📝 本章小结

你现在学会了：

- ✅ 什么是 Rebase，如何使用
- ✅ 什么是 Cherry-pick，如何摘取提交
- ✅ 什么是 Bisect，如何二分查找 Bug
- ✅ 什么是 Reflog，如何恢复丢失的分支
- ✅ 什么是 Submodule，如何管理依赖

### 💡 核心要点

> **Rebase 可以让历史更干净，但要小心使用。**
> 
> **永远不要对已经推送的分支做 Rebase！**

---

<p align="right">
  <a href="chapter24-key-management.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter26-gh-cli.md">下一章 →</a>
</p>
