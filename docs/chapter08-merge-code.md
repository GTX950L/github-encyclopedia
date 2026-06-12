# 第8章：合并代码（Merge）🔀

> 合并（Merge）是把多个分支的代码整合到一起的操作。本章详解各种合并场景和冲突处理。

## 📋 目录

- [什么是合并？](#什么是合并)
- [基本合并操作](#基本合并操作)
- [合并冲突（Conflict）](#合并冲突conflict)
- [合并策略](#合并策略)
- [常见问题](#常见问题)

---

## 什么是合并？

### 基本定义

**合并（Merge）** 就是把分支 B 的修改整合到分支 A。

### 比喻

```
把分支 B 的代码"复制"到分支 A：
│
├── 如果分支 A 和 B 修改的不是同一个文件 → ✅ 自动合并
└── 如果修改了同一个文件的同一行 → ❌ 冲突（需要手动解决）
```

---

## 基本合并操作

### 场景：把功能分支合并到 main

#### 步骤：

```bash
# 第1步：切换到目标分支（通常是 main）
git switch main

# 第2步：拉取最新代码（避免冲突）
git pull origin main

# 第3步：合并功能分支
git merge feature/login

# 第4步：推送到远程
git push origin main
```

---

### 使用 GitHub Pull Request 合并（推荐！）✅

**为什么推荐？**
- ✅ 代码审查（Code Review）
- ✅ 自动化测试（GitHub Actions）
- ✅ 留下合并记录

**步骤：**

```
1. 在 GitHub 上创建 Pull Request
   │
   ├── 进入仓库页面 → Pull requests → New pull request
   ├── Base: main ← Compare: feature/login
   └── 填写 PR 标题和描述 → Create pull request

2. 等待审查（或自己审查）

3. 合并 PR
   │
   ├── Merge pull request（普通合并）
   ├── Squash and merge（压缩成一个提交）
   └── Rebase and merge（变基合并）
```

---

## 合并冲突（Conflict）⚠️

### 什么是冲突？

**当 Git 不知道该怎么合并时，就会发生冲突。**

### 冲突示例

#### 场景：你和同事都修改了同一个文件的同一行

```
你的修改（feature/login 分支）：
│
└── README.md 第5行：
    "项目作者：张三"

同事的修改（main 分支）：
│
└── README.md 第5行：
    "项目作者：李四"
```

**Git 不知道该用谁的版本！**

---

### 解决冲突的步骤

#### 第1步：发生冲突

```bash
$ git merge feature/login
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

#### 第2步：打开冲突文件

Git 会在冲突文件中标记：

```markdown
<<<<<<< HEAD
项目作者：李四
=======
项目作者：张三
>>>>>>> feature/login
```

| 标记 | 含义 |
|------|------|
| `<<<<<<< HEAD` | 当前分支（main）的内容 |
| `=======` | 分隔线 |
| `>>>>>>> feature/login` | 要合并的分支的内容 |

#### 第3步：手动解决冲突

**方法1：保留其中一方**

```markdown
# 删除 Git 的标记，只保留你要的版本
项目作者：张三
```

**方法2：保留双方（手动合并）**

```markdown
项目作者：张三、李四
```

#### 第4步：提交解决后的文件

```bash
# 标记冲突已解决
git add README.md

# 完成合并
git commit -m "merge: 合并 feature/login，解决冲突"

# 推送到远程
git push origin main
```

---

### 使用工具解决冲突（推荐！）✅

#### VS Code（图形化解决冲突）

```
1. 用 VS Code 打开冲突文件
2. VS Code 会自动识别冲突
3. 上方会出现按钮：
   ├── "Accept Current Change"（接受当前分支）
   ├── "Accept Incoming Change"（接受合并分支）
   ├── "Accept Both Changes"（都保留）
   └── "Compare Changes"（对比）
4. 点击按钮，自动解决
```

#### GitKraken / Sourcetree（图形化 Git 工具）

- 可视化分支历史
- 拖拽合并
- 自动解决简单冲突

---

## 合并策略

### 1️⃣ Merge Commit（普通合并）

```
feature/login ────┐
                     ├─→ main
main         ─────────┘
```

**特点**：
- ✅ 保留完整的历史记录
- ✅ 可以看到分支的创建和合并时间
- ❌ 历史记录比较"乱"

---

### 2️⃣ Squash and Merge（压缩合并）✅ 推荐

```
feature/login 的所有提交：
├── fix: 修复登录 Bug
├── feat: 添加登录页面
└── refactor: 重构登录逻辑

压缩成一个提交：
└── feat: 添加登录功能 (#123)
```

**特点**：
- ✅ 保持 main 分支历史干净
- ✅ 适合功能分支（多个小提交 → 一个大提交）
- ✅ GitHub 默认推荐

---

### 3️⃣ Rebase and Merge（变基合并）

```
feature/login 的提交"移动"到 main 的顶端：
│
main:      A ── B ── C
feature:              ── D ── E

Rebase 后：
main:      A ── B ── C ── D' ── E'
```

**特点**：
- ✅ 历史记录是线性的（没有分叉）
- ✅ 看起来像是一直在 main 上开发
- ❌ 会重写提交历史（不要对公开分支使用！）

---

## 常见问题

### Q1: 合并后发现改坏了，怎么办？

**A**: 使用 `git revert` 撤销合并：

```bash
# 查看合并提交的 ID
git log --oneline

# 撤销合并（会生成一个新的"反向提交"）
git revert -m 1 <合并提交的 ID>
```

---

### Q2: 可以取消正在进行的合并吗？

**A**: ✅ 可以！

```bash
# 取消合并，回到合并前的状态
git merge --abort
```

---

### Q3: 什么叫"快进合并"（Fast-forward）？

**A**: 当 main 分支没有新的提交时，Git 可以直接"快进"，不需要创建合并提交。

```
合并前：
main:     A ── B
feature:         ── C ── D

快进合并后（直接移动 main 指针）：
main:    A ── B ── C ── D
```

**特点**：历史记录是线性的，没有分叉。

---

## 📝 实战练习

### 练习1：模拟冲突并解决

**任务**：
1. 创建两个分支，都修改 README.md 的同一行
2. 尝试合并，触发冲突
3. 使用 VS Code 解决冲突
4. 完成合并

---

### 练习2：使用 Squash 合并

**任务**：
1. 在功能分支上做 3 个小提交
2. 创建 PR
3. 使用 "Squash and merge" 合并
4. 查看 main 分支的历史，确认只有一个提交

---

## 📚 延伸阅读

- [第7章：分支管理](chapter07-branch-management.md) - 分支的基本操作
- [第9章：Fork 与 Pull Request](chapter09-fork-pr.md) - 使用 PR 合并代码
- [第25章：Git 高级命令](chapter25-git-advanced.md) - Rebase 详解

---

## 📝 本章小结

你现在学会了：

- ✅ 什么是合并，以及如何合并分支
- ✅ 如何解决合并冲突
- ✅ 三种合并策略的区别
- ✅ 如何使用 GitHub PR 合并

### 下一步学习

- 想给开源项目贡献代码？去 → [第9章：Fork 与 PR](chapter09-fork-pr.md)
- 想了解 GitHub 的协作功能？去 → [第10章：Issues 使用指南](chapter10-issues-guide.md)

---

<p align="right">
  <a href="chapter07-branch-management.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter09-fork-pr.md">下一章 →</a>
</p>
