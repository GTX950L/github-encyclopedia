# 第10章：Issues 使用指南 🐛

> Issues 是 GitHub 的"论坛"。报告 Bug、提出功能请求、讨论技术问题，都在这里。

## 📋 目录

- [什么是 Issues？](#什么是-issues)
- [创建 Issue](#创建-issue)
- [Issue 模板](#issue-模板)
- [管理 Issues](#管理-issues)
- [常见问题](#常见问题)

---

## 什么是 Issues？

### 基本定义

**Issue（问题）** 就是项目的"讨论区"。

### Issues 可以做什么？

| 用途 | 说明 | 示例 |
|------|------|------|
| **报告 Bug** | 告诉维护者项目有错误 | "登录按钮在 Safari 上无法点击" |
| **提出功能请求** | 建议新功能 | "希望能添加深色模式" |
| **提问** | 询问如何使用 | "如何使用 API 获取用户列表？" |
| **讨论** | 技术讨论 | "关于重构用户模块的思考" |

---

## 创建 Issue

### 步骤详解

#### 第1步：进入 Issues 页面

```
访问仓库页面 → 点击 "Issues" 标签
│
或直接访问：https://github.com/用户名/仓库名/issues
```

#### 第2步：点击 "New Issue"

```
点击绿色的 "New issue" 按钮
│
如果有模板，选择适合的类型：
├── 🐛 Bug Report（报告 Bug）
├── ✨ Feature Request（功能请求）
└── ❓ Question（提问）
```

#### 第3步：填写 Issue 信息

**标题（Title）**：

```
✅ 好的标题：
"Bug: 登录按钮在 Safari 上无法点击"
"Feature: 希望添加深色模式"
"Question: 如何使用 API 获取用户列表？"

❌ 不好的标题：
"帮帮我"
"有个问题"
"Bug"
```

**描述（Description）**：

使用模板（如果有），或包含以下信息：

**Bug Report 模板**：

```markdown
## 🐛 Bug 描述

简短描述这个 Bug。

## 🔄 复现步骤

1. 打开 '...'
2. 点击 '...'
3. 看到错误

## ✅ 预期行为

描述你期望发生的事情。

## 📷 截图

如果适用，添加截图。

## 💻 环境信息

- 操作系统：[如 Windows 11, macOS Sonoma]
- 浏览器：[如 Chrome 120, Safari 17]
- 版本：[如 v1.2.0]
```

**功能请求模板**：

```markdown
## ✨ 功能描述

简要描述这个新功能。

## 🎯 目标

这个功能是为了解决什么问题？

## 💡 建议的解决方案

描述你希望的解决方案。

## 🔄 替代方案

描述其他考虑过的方案。
```

---

## Issue 模板

### 为什么需要模板？

**模板可以引导报告者提供必要的信息**，避免：
- ❌ "项目有 Bug！"（没有提供任何细节）
- ❌ "希望添加新功能！"（没有说明是什么功能）

---

### 如何添加 Issue 模板？

#### 方法1：使用 GitHub 网页配置（推荐）✅

```
1. 进入仓库 Settings → Features → Issues
2. 点击 "Set up templates"
3. 选择模板类型：
   ├── Bug report
   ├── Feature request
   └── Custom（自定义）
4. 编辑模板内容
5. 提交
```

#### 方法2：手动创建模板文件

在仓库中创建 `.github/ISSUE_TEMPLATE/` 目录：

```
.my-repo/
└── .github/
    └── ISSUE_TEMPLATE/
        ├── bug_report.md
        ├── feature_request.md
        └── custom.md
```

**示例：`bug_report.md`**

```markdown
---
name: Bug Report
about: Create a report to help us improve
title: ''
labels: 'bug'
assignees: ''

---

**Describe the bug**
A clear and concise description of what the bug is.

**To Reproduce**
Steps to reproduce the behavior:
1. Go to '...'
2. Click on '...'
3. Scroll down to '...'
4. See error

**Expected behavior**
A clear and concise description of what you expected to happen.

**Screenshots**
If applicable, add screenshots to help explain your problem.

**Desktop (please complete the following information):**
 - OS: [e.g. iOS]
 - Browser [e.g. chrome, safari]
 - Version [e.g. 22]

**Smartphone (please complete the following information):**
 - Device: [e.g. iPhone6]
 - OS: [e.g. iOS8.1]
 - Browser [e.g. stock browser, safari]
 - Version [e.g. 22]

**Additional context**
Add any other context about the problem here.
```

---

## 管理 Issues

### 标签（Labels）

**作用**：分类 Issue。

**常用标签**：

| 标签 | 颜色 | 说明 |
|------|------|------|
| `bug` | 🔴 红色 | Bug 报告 |
| `enhancement` | 🟢 绿色 | 新功能 |
| `documentation` | 🔵 蓝色 | 文档相关 |
| `good first issue` | 🟣 紫色 | 适合新手 |
| `help wanted` | 🟠 橙色 | 需要帮助 |
| `question` | 🟡 黄色 | 提问 |

---

### 里程碑（Milestones）

**作用**：规划版本发布。

**示例**：

```
Milestone: v1.0.0
│
├── Issue #1: 添加登录功能 ✅
├── Issue #2: 添加注册功能 ✅
├── Issue #3: 修复登录 Bug 🔄
└── Issue #4: 添加支付功能 ⏳

进度：3/4 (75%)
```

---

### 指派（Assignees）

**作用**：指定谁来负责这个 Issue。

```
Assignee: @GTX950L
│
→ 这个人会收到通知
→ 其他人知道谁是负责人
```

---

## 常见问题

### Q1: 可以删除自己的 Issue 吗？

**A**: ✅ **可以，但只能在关闭状态下删除。**

```
步骤：
1. 关闭 Issue（Close issue）
2. 滚动到最底部
3. 点击 "Delete issue"
```

> ⚠️ **注意**：删除后无法恢复！

---

### Q2: 如何搜索 Issue？

**A**: 使用搜索框，支持多种筛选条件。

**搜索语法**：

```
# 搜索标题或描述中包含关键词的 Issue
is:issue is:open 登录

# 搜索带有某个标签的 Issue
label:bug

# 搜索指派给某个人的 Issue
assignee:GTX950L

# 搜索你创建的 Issue
author:你的用户名

# 组合搜索
is:issue is:open label:bug author:GTX950L
```

---

### Q3: 如何获得 Issue 的通知？

**A**: 订阅（Subscribe）Issue。

```
打开 Issue 页面 → 点击 "Subscribe" 按钮
│
你会收到以下通知：
├── 有人评论了这个 Issue
├── Issue 被关闭
└── Issue 被重新打开
```

---

## 📝 实战练习

### 练习1：创建第一个 Issue

**任务**：
1. 在你的仓库创建一个 Issue
2. 使用 Bug Report 模板
3. 填写完整信息
4. 添加 `bug` 标签

---

### 练习2：配置 Issue 模板

**任务**：
1. 在仓库中添加 Issue 模板
2. 至少添加 Bug Report 和 Feature Request 两个模板
3. 测试：创建 Issue，看是否显示模板

---

### 练习3：管理 Issues

**任务**：
1. 创建 3 个 Issue
2. 给它们添加不同的标签
3. 创建一个 Milestone，把这 3 个 Issue 加进去
4. 关闭其中一个 Issue

---

## 📚 延伸阅读

- [第9章：Fork 与 Pull Request](chapter09-fork-pr.md) - 提交代码修复 Issue
- [第11章：Projects 与项目管理](chapter11-projects-management.md) - 使用看板管理 Issues
- [第12章：Wiki 与文档](chapter12-wiki-documentation.md) - 编写项目文档

---

## 📝 本章小结

你现在学会了：

- ✅ 什么是 Issues，以及它的用途
- ✅ 如何创建好的 Issue（标题、描述、模板）
- ✅ 如何管理 Issues（标签、里程碑、指派）
- ✅ 如何搜索和订阅 Issues

### 💡 核心要点

> **好的 Issue 应该让维护者一眼看出问题。**
> 
> 提供详细的信息（复现步骤、环境、截图），能大大提高 Issue 被解决的速度。

---

<p align="right">
  <a href="chapter09-fork-pr.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter11-projects-management.md">下一章 →</a>
</p>