# 第7章：分支管理（Branch）🌿

> 分支是 Git 最强大的功能之一。它让你可以同时开发多个功能，互不干扰。

## 📋 目录

- [什么是分支？](#什么是分支)
- [为什么需要分支？](#为什么需要分支)
- [基本操作](#基本操作)
- [分支策略](#分支策略)
- [常见问题](#常见问题)

---

## 什么是分支？

### 基本定义

**分支（Branch）** 就是一条独立的开发线。

### 比喻

```
主干道（main 分支）：
│
├── 已发布的功能 A
├── 已发布的功能 B
└── 已发布的功能 C

分支（feature/login 分支）：
│
├── 正在开发的登录功能（不影响主干道）
├── 正在开发的注册功能（不影响主干道）
└── 可以随时合并回主干道
```

### 图示

```
main 分支（生产环境）
│
├── o── 初始提交
├── o── 功能 A
├── o── 功能 B
└── o── 功能 C

feature/login 分支（开发分支）
│
├── o── 初始提交
├── o── 功能 A
├── o── 功能 B
├── o── 功能 C
├── o── 登录：创建登录页面
└── o── 登录：添加表单验证
```

---

## 为什么需要分支？

### 场景1：同时开发多个功能

```
❌ 不用分支（危险！）：
│
├── 你在开发登录功能
├── 同事在开发支付功能
├── 你们都在 main 分支上修改
├── 你的代码和同事的代码冲突了！
└── 项目炸了...

✅ 使用分支（安全！）：
│
├── 你创建 feature/login 分支 → 在自己的分支上开发
├── 同事创建 feature/payment 分支 → 在自己的分支上开发
├── 你们互不干扰
└── 开发完成后，再合并到 main 分支
```

### 场景2：修复线上 Bug

```
✅ 正确的做法：
│
├── 从 main 分支创建一个 hotfix 分支
├── 在 hotfix 分支上修复 Bug
├── 测试通过后，合并到 main 分支
└── 同时合并到开发分支（保持同步）
```

---

## 基本操作

### 1️⃣ 查看分支

```bash
# 查看本地分支
git branch
# 输出：
# * main  ← 当前所在分支（带 * 号）

# 查看所有分支（包括远程）
git branch -a
```

---

### 2️⃣ 创建分支

```bash
# 方法1：创建分支（不切换）
git branch feature/login

# 方法2：创建并切换到新分支（推荐）
git checkout -b feature/login

# 方法3：使用新版命令（推荐）
git switch -c feature/login
```

---

### 3️⃣ 切换分支

```bash
# 方法1：使用 checkout（旧版）
git checkout main

# 方法2：使用 switch（新版，更直观）
git switch main
```

> ⚠️ **切换分支前，确保当前分支的修改已提交！**
> 否则，未提交的修改会被带到新分支。

---

### 4️⃣ 删除分支

```bash
# 删除已合并的分支
git branch -d feature/login

# 强制删除未合并的分支（⚠️ 会丢失修改！）
git branch -D feature/login

# 删除远程分支
git push origin --delete feature/login
```

---

### 5️⃣ 推送分支到远程

```bash
# 第一次推送分支
git push -u origin feature/login
# -u 参数会把本地分支和远程分支关联起来
# 以后只需要 git push 即可

# 查看分支关联
git branch -vv
```

---

## 分支策略

### Git Flow（经典分支模型）

```
main（生产环境）
│
├── release/1.0.0（发布分支）
├── develop（开发分支）
│   │
│   ├── feature/login（功能分支）
│   ├── feature/payment（功能分支）
│   └── feature/xxx（功能分支）
│
└── hotfix/critical-bug（紧急修复分支）
```

| 分支类型 | 作用 | 从哪来？ | 合并到哪？ |
|----------|------|----------|------------|
| **main** | 生产环境 | - | - |
| **develop** | 开发主线 | main | - |
| **feature/xxx** | 新功能 | develop | develop |
| **release/xxx** | 发布准备 | develop | main + develop |
| **hotfix/xxx** | 紧急修复 | main | main + develop |

---

### GitHub Flow（简单分支模型，推荐！）✅

```
main（默认分支，始终可发布）
│
└── feature/xxx（功能分支）
    │
    └── 开发完成后，创建 Pull Request → 审查 → 合并到 main
```

**为什么推荐？**
- ✅ 简单，容易理解
- ✅ 适合持续部署（CD）
- ✅ GitHub 官方推荐

---

## 常见问题

### Q1: 分支名有什么规范吗？

**A**: 建议使用以下格式：

| 类型 | 格式 | 示例 |
|------|------|------|
| 新功能 | `feature/功能名` | `feature/login` |
| 修复 Bug | `fix/bug描述` | `fix/login-fail` |
| 紧急修复 | `hotfix/问题` | `hotfix/critical-error` |
| 文档 | `docs/内容` | `docs/api-guide` |
| 重构 | `refactor/模块` | `refactor/user-module` |

---

### Q2: 可以删除 main 分支吗？

**A**: ❌ **不可以！**

`main` 分支是默认分支，删除后会导致：
- ❌ 项目的默认分支丢失
- ❌ Pull Request 无法合并
- ❌ 很多功能会失效

**如果一定要改默认分支**：
```
Settings → Branches → Default branch → 切换成其他分支
```

---

### Q3: 切换分支时，当前的修改会丢失吗？

**A**: 取决于修改是否已提交：

| 状态 | 切换分支后？ |
|------|----------------|
| **已提交** | ✅ 安全，修改保存在当前分支 |
| **未提交，但不在冲突文件** | ✅ 会带到新分支 |
| **未提交，且在冲突文件** | ❌ 拒绝切换，先提交或暂存 |

**如何暂存修改？**

```bash
# 暂存当前修改
git stash

# 切换分支，做其他事情...

# 回到原分支，恢复暂存的修改
git stash pop
```

---

## 📝 实战练习

### 练习1：创建并使用分支

**任务**：
1. 创建 `feature/hello` 分支
2. 在新分支上修改 README.md
3. 提交修改
4. 切换回 main 分支
5. 查看 README.md，确认修改不在 main 分支

---

### 练习2：删除已合并的分支

**任务**：
1. 创建分支，做一些修改，合并到 main
2. 删除本地分支
3. 删除远程分支

---

## 📚 延伸阅读

- [第8章：合并代码](chapter08-merge-code.md) - 如何合并分支
- [第9章：Fork 与 Pull Request](chapter09-fork-pr.md) - 使用分支贡献开源项目

---

## 📝 本章小结

你现在学会了：

- ✅ 什么是分支，为什么需要它
- ✅ 如何创建、切换、删除分支
- ✅ 分支策略（Git Flow vs GitHub Flow）
- ✅ 如何推送分支到远程

### 下一步学习

- 想合并分支？去 → [第8章：合并代码](chapter08-merge-code.md)
- 想了解 Pull Request？去 → [第9章：Fork 与 PR](chapter09-fork-pr.md)

---

<p align="right">
  <a href="chapter06-commit-changes.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter08-merge-code.md">下一章 →</a>
</p>
