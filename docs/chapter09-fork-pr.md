# 第9章：Fork 与 Pull Request 🔀

> 想给开源项目贡献代码？Fork 和 PR 是必经之路！本章手把手教你。

## 📋 目录

- [什么是 Fork？](#什么是-fork)
- [什么是 Pull Request？](#什么是-pull-request)
- [完整贡献流程](#完整贡献流程)
- [PR 的最佳实践](#pr-的最佳实践)
- [维护者的视角](#维护者的视角)
- [常见问题](#常见问题)

---

## 什么是 Fork？

### 基本定义

**Fork（复刻）** 就是把别人的仓库复制一份到你的账号下。

### 为什么需要 Fork？

```
场景：你想给别人的开源项目贡献代码
│
├── ❌ 你没有直接修改权限
├── ✅ 解决方案：Fork 到你的账号
├── ✅ 在你的副本上修改
└── ✅ 提交 Pull Request 给原作者
```

### Fork vs Clone

| 操作 | 作用 | 位置 |
|------|------|------|
| **Fork** | 复制仓库到你的 GitHub 账号 | GitHub 云端 |
| **Clone** | 下载仓库到你的本地电脑 | 本地电脑 |

**典型流程**：
```
1. Fork（在 GitHub 上复制）
   │
   ▼
2. Clone（下载到本地）
   │
   ▼
3. 修改代码（在本地）
   │
   ▼
4. Push（推送到你的 Fork）
   │
   ▼
5. 创建 PR（请求合并到原仓库）
```

---

## 什么是 Pull Request？

### 基本定义

**Pull Request（拉取请求，简称 PR）** 是你请求原仓库合并你的修改。

### 为什么叫"拉取请求"？

```
你：", 我把代码改好了，请【拉取】我的修改！"
原作者：", 让我审查一下..."
```

### PR 的生命周期

```
1. 创建 PR
   ↓
2. 代码审查（Code Review）
   ↓
3. 自动化测试（GitHub Actions）
   ↓
4. 修改（如果需要）
   ↓
5. 合并（Merge）或关闭（Close）
```

---

## 完整贡献流程

### 场景：给 `vuejs/core` 修复一个 Bug

#### 第1步：Fork 仓库

```
1. 访问 https://github.com/vuejs/core
2. 点击右上角的 Fork 按钮
3. 等待几秒，GitHub 会复制一份到你的账号
4. 现在你有了：github.com/你的用户名/core
```

---

#### 第2步：Clone 你的 Fork

```bash
# 克隆你的 Fork（不是原仓库！）
git clone https://github.com/你的用户名/core.git

# 进入项目目录
cd core
```

---

#### 第3步：添加原仓库为 upstream

```bash
# 添加原仓库为 upstream（上游）
git remote add upstream https://github.com/vuejs/core.git

# 查看远程仓库
git remote -v
# 输出：
# origin    https://github.com/你的用户名/core.git (fetch)
# origin    https://github.com/你的用户名/core.git (push)
# upstream  https://github.com/vuejs/core.git (fetch)
# upstream  https://github.com/vuejs/core.git (push)
```

> 💡 **为什么需要 upstream？**
> 如果原仓库有更新，你可以拉取最新代码，保持同步。

---

#### 第4步：创建分支并修改代码

```bash
# 从 main 分支创建新分支
git switch -c fix/typo-in-readme

# 修改代码...
# 例如：修复 README.md 中的错别字

# 提交修改
git add .
git commit -m "fix: 修复 README 中的错别字"

# 推送到你的 Fork
git push -u origin fix/typo-in-readme
```

---

#### 第5步：创建 Pull Request

```
1. 访问你的 Fork 页面
2. GitHub 会提示："Compare & pull request"
3. 点击，填写 PR 信息：
   │
   ├── 标题：fix: 修复 README 中的错别字
   ├── 描述：
   │   - 问题：README.md 第5行有错别字
   │   - 修改：将"登录"改为"登陆"
   │   - 测试：已检查，无其他错误
   └── 关联 Issue（如果有）：Closes #123
4. 点击 "Create pull request"
```

---

#### 第6步：等待审查

```
审查员可能会：
│
├── ✅ 批准（Approve）→ 可以合并
├── 💬 提出修改意见 → 你需要修改代码
└── ❌ 拒绝（Request changes）→ 需要大改
```

**如何修改 PR？**

```bash
# 不需要关闭 PR，直接修改代码，再推送！
# Git 会自动更新 PR。

# 修改代码...
git add .
git commit -m "fix: 根据审查意见修改"
git push origin fix/typo-in-readme
```

---

#### 第7步：合并 PR

```
审查通过后，维护者会合并你的 PR。
│
├── 你的代码成为项目的一部分！🎉
├── 你的名字会出现在贡献者列表中
└── 你可以删除你的功能分支（可选）
```

---

## PR 的最佳实践

### ✅ 好的 PR 应该...

| 特点 | 说明 |
|------|------|
| **小而专注** | 一个 PR 只做一件事 |
| **清晰的标题** | 一眼看出这个 PR 是做什么的 |
| **详细的描述** | 说明为什么需要这个修改 |
| **关联 Issue** | 使用 `Closes #123` 自动关闭 Issue |
| **通过测试** | 确保所有测试通过 |
| **代码风格一致** | 遵循项目的代码规范 |

---

### ❌ 不好的 PR...

| 特点 | 例子 |
|------|------|
| **太大** | 一个 PR 改了 50 个文件 |
| **标题模糊** | "fix"、"update"、"改了点东西" |
| **没有描述** | 只写了标题，没有说明 |
| **包含无关修改** | 既修复 Bug，又重构代码，又添加新功能 |
| **没有测试** | 代码有 Bug，测试没通过 |

---

### PR 标题规范

建议使用以下格式：

```
<类型>: <描述>

类型：
- feat: 新功能
- fix: 修复 Bug
- docs: 文档更新
- style: 代码格式（不影响功能）
- refactor: 重构
- test: 测试相关
- chore: 其他改动
```

**示例**：

```
✅ feat: 添加用户登录功能
✅ fix: 修复移动端显示异常的 Bug
✅ docs: 更新 API 文档

❌ update
❌ 改了点东西
❌ 修复了一个 bug
```

---

## 维护者的视角

### 如何审查 PR？

```
收到 PR → 审查代码 → 提出意见 → 批准或拒绝
```

#### 审查要点

| 检查项 | 说明 |
|--------|------|
| **代码质量** | 有没有明显的 Bug？ |
| **代码风格** | 是否符合项目规范？ |
| **测试** | 有没有写测试？测试通过了吗？ |
| **文档** | 需要更新文档吗？ |
| **性能** | 有没有性能问题？ |

---

### 如何合并 PR？

GitHub 提供三种合并方式：

| 方式 | 说明 | 适用场景 |
|------|------|----------|
| **Merge commit** | 保留完整历史 | 大型项目、需要追溯历史 |
| **Squash and merge** | 压缩成一个提交 | 小型 PR、保持历史干净 |
| **Rebase and merge** | 变基合并 | 需要线性历史 |

**推荐**：**Squash and merge** ✅

---

## 常见问题

### Q1: Fork 后的仓库会同步原仓库的更新吗？

**A**: ❌ **不会自动同步！**

**如何手动同步？**

```bash
# 1. 拉取 upstream 的更新
git fetch upstream

# 2. 合并到你的 main 分支
git switch main
git merge upstream/main

# 3. 推送到你的 Fork
git push origin main
```

---

### Q2: 可以 Fork 自己的仓库吗？

**A**: 技术上可以，但**通常不需要**。

Fork 主要用于：
- 给开源项目贡献代码
- 基于别人的项目做修改

如果你是自己项目的协作者，直接 **Clone** 即可，不需要 Fork。

---

### Q3: PR 被拒绝了怎么办？

**A**: 不要气馁！

**常见原因**：
1. 项目已经有类似功能了
2. 修改不符合项目的方向
3. 代码质量不够

**怎么办？**
1. 仔细阅读拒绝理由
2. 如果理由合理，感谢维护者
3. 如果理由不合理，礼貌地讨论
4. 改进代码，重新提交（如果需要）

---

### Q4: 我可以撤回 PR 吗？

**A**: ✅ **可以！**

```
方法1：在 PR 页面点击 "Close pull request"
方法2：推送一个新的提交，覆盖之前的修改
```

---

## 📝 实战练习

### 练习1：完成第一次贡献

**任务**：
1. 找一个你感兴趣的开源项目
2. Fork 它
3. 修复一个小 Bug 或改进文档
4. 提交你的第一个 PR！

---

### 练习2：同步原仓库的更新

**任务**：
1. Fork 一个活跃的项目
2. 一周后，原仓库有新的提交
3. 同步这些更新到你的 Fork

---

## 📚 延伸阅读

- [第7章：分支管理](chapter07-branch-management.md) - 理解分支的概念
- [第8章：合并代码](chapter08-merge-code.md) - 合并分支的详细讲解
- [第10章：Issues 使用指南](chapter10-issues-guide.md) - 如何报告 Bug、提出建议

---

## 📝 本章小结

你现在了解了：

- ✅ 什么是 Fork，为什么要 Fork
- ✅ 什么是 Pull Request，如何创建
- ✅ 完整的贡献流程（从 Fork 到合并）
- ✅ PR 的最佳实践
- ✅ 如何作为维护者审查 PR

### 💡 核心要点

> **开源贡献不仅仅是写代码！**
> 
> 修复文档错别字、翻译文档、改进 README，这些都是非常有价值的贡献。

---

<p align="right">
  <a href="chapter08-merge-code.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter10-issues-guide.md">下一章 →</a>
</p>