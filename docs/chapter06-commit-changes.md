# 第6章：提交更改（Commit）💾

> Commit（提交）是 Git 的核心概念。每次提交就像是"存档"，让你可以随时回到过去。

## 📋 目录

- [什么是 Commit？](#什么是-commit)
- [Commit 的生命周期](#commit-的生命周期)
- [如何写一个好的 Commit Message](#如何写一个好的-commit-message)
- [修改上一次提交](#修改上一次提交)
- [查看提交历史](#查看提交历史)
- [撤销提交](#撤销提交)
- [常见问题](#常见问题)

---

## 什么是 Commit？

### 基本定义

**Commit（提交）** 就是把当前的代码状态"拍照"保存下来。

### 比喻

```
玩游戏的"存档"：
│
├── 存档1：通关第1关（10:00）
├── 存档2：获得宝剑（10:30）
├── 存档3：击败 BOSS（11:00）
└── 存档4：通关！（12:00）

如果失败了，可以读档重来...

Git Commit 也是一样：
│
├── Commit 1：创建项目（2026-06-01）
├── Commit 2：添加登录功能（2026-06-02）
├── Commit 3：修复登录 Bug（2026-06-03）
└── Commit 4：添加支付功能（2026-06-04）

如果代码改坏了，可以回退到某个 Commit...
```

---

## Commit 的生命周期

### 三步提交法

```
工作区（Working Directory）
    │
    │ git add（添加文件到暂存区）
    ▼
暂存区（Staging Area）
    │
    │ git commit（提交到仓库）
    ▼
本地仓库（Local Repository）
    │
    │ git push（推送到远程）
    ▼
远程仓库（Remote Repository，如 GitHub）
```

### 详细步骤

#### 第1步：修改文件

```bash
# 修改文件
echo "New feature" >> README.md

# 查看状态
git status
# 会显示：modified: README.md（红色，表示在工作区）
```

#### 第2步：添加到暂存区

```bash
# 添加单个文件
git add README.md

# 添加所有修改的文件
git add .

# 查看状态
git status
# 会显示：modified: README.md（绿色，表示在暂存区）
```

#### 第3步：提交

```bash
# 提交（必须写提交信息！）
git commit -m "Add: 添加新的功能说明到 README"

# 查看提交历史
git log
```

#### 第4步：推送到 GitHub

```bash
git push origin main
```

---

## 如何写一个好的 Commit Message

### ❌ 反面教材

```
update
修改
fix
asdf
改了一下
修复了一个 bug
```

### ✅ 好的 Commit Message 规范

#### 格式

```
<类型>: <主题>

<正文>（可选）
```

#### 类型（Type）

| 类型 | 说明 | 示例 |
|------|------|------|
| **feat** | 新功能 | `feat: 添加用户登录功能` |
| **fix** | 修复 Bug | `fix: 修复登录失败的问题` |
| **docs** | 文档更新 | `docs: 更新 README` |
| **style** | 代码格式（不影响功能） | `style: 格式化代码` |
| **refactor** | 重构（不是新功能也不是修复） | `refactor: 重构用户模块` |
| **test** | 测试相关 | `test: 添加登录功能的单元测试` |
| **chore** | 其他改动（依赖更新等） | `chore: 更新依赖` |

#### 示例

```bash
# ✅ 好的 Commit Message
git commit -m "feat: 添加用户注册功能"

git commit -m "fix: 修复移动端显示异常的 Bug"

git commit -m "docs: 更新 API 文档"

# ❌ 不好的 Commit Message
git commit -m "update"
git commit -m "改了点东西"
```

---

## 修改上一次提交

### 场景：提交后发现写错了

#### 方法1：补充提交（推荐）✅

```bash
# 修改文件
echo "补充内容" >> README.md

# 补充到上一次提交
git add .
git commit --amend --no-edit

# 会合并到上一次提交（提交信息不变）
```

#### 方法2：修改提交信息

```bash
# 修改上一次的提交信息
git commit --amend -m "新的提交信息"
```

> ⚠️ **警告**：`--amend` 会重写 Git 历史！如果已经推送到远程，不要使用（除非你确定知道自己在做什么）。

---

## 查看提交历史

### 基本命令

```bash
# 查看完整历史
git log

# 查看简洁版本（一行显示一个提交）
git log --oneline

# 查看最近 5 次提交
git log -5 --oneline

# 查看某个文件的提交历史
git log -- README.md
```

### 示例输出

```bash
$ git log --oneline

a1b2c3d (HEAD -> main) fix: 修复登录 Bug
e4f5g6h feat: 添加用户注册功能
i7j8k9l docs: 更新 README
m1n2o3p init: 初始化项目
```

---

## 撤销提交

### 场景1：想撤销本地提交（未推送）

```bash
# 方法1：保留修改，撤销提交
git reset --soft HEAD~1

# 方法2：不保留修改，直接删掉提交
git reset --hard HEAD~1
```

> ⚠️ **`--hard` 会删除修改！** 慎用！

---

### 场景2：想撤销已推送的提交

**不要直接 reset！** 会影响其他协作者。

#### 正确做法：使用 revert

```bash
# 创建一个"反向提交"，抵消之前的提交
git revert a1b2c3d

# 会打开编辑器，让你确认提交信息
# 保存后，会生成一个新的提交
```

---

## 常见问题

### Q1: 提交太频繁好不好？

**A**: ✅ **频繁提交是好习惯！**

**建议**：
- 每完成一个小功能，就提交一次
- 不要积累几百行代码才提交
- 提交信息要写清楚（不要写 "update"）

---

### Q2: 可以把敏感信息提交后再删除吗？

**A**: ❌ **不可以！**

**原因**：
- Git 历史中会保留敏感信息
- 即使你删除了，别人仍然可以从历史中找到

**正确做法**：
1. 立即撤销提交（如果还没推送）
2. 如果已经推送，使用 [BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/) 清理历史

---

### Q3: Commit 和 Push 有什么区别？

| 操作 | 作用 | 位置 |
|------|------|------|
| **Commit** | 保存到本地仓库 | 你的电脑 |
| **Push** | 上传到远程仓库 | GitHub 云端 |

**关系**：
```
Commit → Commit → Commit → Push
（本地存档）        （上传到云端）
```

---

## 📝 实战练习

### 练习1：第一次提交

**任务**：
1. 创建或修改一个文件
2. 使用 `git add` 添加到暂存区
3. 使用规范的 Commit Message 提交
4. 推送到 GitHub

---

### 练习2：修改上一次提交

**任务**：
1. 提交一个修改
2. 发现漏了文件，补充提交
3. 使用 `git commit --amend` 合并到上一次提交

---

### 练习3：撤销提交

**任务**：
1. 故意提交一个错误修改
2. 使用 `git revert` 撤销
3. 查看提交历史，确认撤销成功

---

## 📚 延伸阅读

- [第7章：分支管理](chapter07-branch-management.md) - 使用分支隔离开发
- [第8章：合并代码](chapter08-merge-code.md) - 合并分支
- [第9章：Fork 与 Pull Request](chapter09-fork-pr.md) - 贡献开源项目

---

## 📝 本章小结

你现在学会了：

- ✅ 什么是 Commit，以及它的生命周期
- ✅ 如何写规范的 Commit Message
- ✅ 如何修改和撤销提交
- ✅ 如何查看提交历史

### 下一步学习

- 想学习分支？去 → [第7章：分支管理](chapter07-branch-management.md)
- 想了解 GitHub 协作？去 → [第9章：Fork 与 PR](chapter09-fork-pr.md)

---

<p align="right">
  <a href="chapter05-download-code.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter07-branch-management.md">下一章 →</a>
</p>
