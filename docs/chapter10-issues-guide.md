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
name: Bug 报告
about: 创建一份报告，帮助我们改进项目
title: ''
labels: 'bug'
assignees: ''

---

**Bug 描述**
清晰简洁地描述这个 Bug。

**复现步骤**
复现该行为的步骤：
1. 前往 '...'
2. 点击 '...'
3. 向下滚动至 '...'
4. 看到错误

**预期行为**
清晰简洁地描述你期望发生什么。

**截图**
如果适用，添加截图以帮助说明问题。

**桌面端（请填写以下信息）：**
 - 操作系统：[如 Windows 11]
 - 浏览器：[如 Chrome, Edge]
 - 版本：[如 120]

**手机端（请填写以下信息）：**
 - 设备：[如 iPhone 15]
 - 操作系统：[如 iOS 17]
 - 浏览器：[如 自带浏览器, Safari]
 - 版本：[如 120]

**补充说明**
在此添加关于该问题的任何其他信息。
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

## 🎯 进阶技巧：Issue Forms（表单模板）

### 什么是 Issue Forms？

Issue Forms 是 **新一代的 Issue 模板**，使用 YAML 格式定义。相比传统的 Markdown 模板，它更加结构化，提交者填写起来也更方便。

### 与传统模板对比

| 特性 | Markdown 模板 | Issue Forms（YAML 表单） |
|------|---------------|------------------------|
| **填写方式** | 自由文本（容易格式混乱） | 表单输入（结构化） |
| **必填字段** | 无法强制 | ✅ 可以设置必填 |
| **下拉选择** | ❌ 不支持 | ✅ 支持 |
| **复选框** | 手写 Markdown | ✅ 原生支持 |
| **使用难度** | ⭐ 简单 | ⭐⭐ 中等 |

### 创建 Issue Forms

#### 文件位置

```
.github/ISSUE_TEMPLATE/
├── bug_report.yml     ← YAML 格式
├── feature_request.yml
└── config.yml
```

#### 示例：Bug Report 表单

```yaml
# .github/ISSUE_TEMPLATE/bug_report.yml
name: 🐛 Bug 报告
description: 提交 Bug 帮助我们改进
title: "[Bug]: "
labels: ["bug"]

body:
  - type: markdown
    attributes:
      value: |
        感谢你提交 Bug 报告！请尽量提供完整信息。

  - type: input
    id: version
    attributes:
      label: 版本号
      description: 你正在使用的版本是什么？
      placeholder: "v1.0.0"
    validations:
      required: true

  - type: textarea
    id: description
    attributes:
      label: Bug 描述
      description: 请清晰描述这个 Bug
      placeholder: 登录按钮点击后无反应...
    validations:
      required: true

  - type: textarea
    id: reproduction
    attributes:
      label: 复现步骤
      description: 如何复现这个 Bug？
      placeholder: |
        1. 打开网站
        2. 点击登录按钮
        3. 页面无响应
    validations:
      required: true

  - type: dropdown
    id: os
    attributes:
      label: 操作系统
      multiple: true
      options:
        - Windows 11
        - macOS Sonoma
        - Ubuntu 24.04
        - 其他

  - type: checkboxes
    id: checks
    attributes:
      label: 检查清单
      options:
        - label: 我已经搜索过已有的 Issues，没有重复
          required: true
        - label: 我正在使用最新版本
          required: false
```

### 效果

```
当你创建 Issue 时，会看到这样的页面：
┌─────────────────────────────────────┐
│ 🐛 Bug 报告                          │
│                                     │
│ 版本号 *                             │
│ [___________v1.0.0__________]       │
│                                     │
│ Bug 描述 *                           │
│ [描述这个 Bug...                   ] │
│                                     │
│ 复现步骤 *                           │
│ [1. 打开网站                       ] │
│                                     │
│ 操作系统                             │
│ [▼ Windows 11     ] [+]             │
│                                     │
│ ☑ 我已经搜索过已有的 Issues           │
└─────────────────────────────────────┘
```

> 💡 **YAML 表单的字段类型**：`input`（单行文本）、`textarea`（多行文本）、`dropdown`（下拉选择）、`checkboxes`（复选框）、`markdown`（说明文字）

---

## 🏗️ 进阶技巧：Sub-issues 与 Issue Types

### 什么是 Sub-issues？

**Sub-issues（子问题）** 是 2025 年 4 月正式发布的功能，让你可以在 Issue 内部创建子 Issue，形成父子层级关系。

**为什么需要 Sub-issues？**

```
以前的做法：                现在的做法：
┌───────────────────┐      ┌─ 父 Issue：添加登录功能 ────────┐
│ 一个 Issue 里写满     │      │ ├─ 🔲 前端登录页面 UI           │
│ 长长的待办列表：      │      │ ├─ 🔲 后端登录 API              │
│                      │      │ ├─ 🔲 数据库用户表               │
│ [ ] 前端 UI          │      │ └─ 🔲 测试用例                   │
│ [ ] 后端 API         │      └───────────────────────────────┘
│ [ ] 数据库设计       │      └ 每个子 Issue 可独立跟踪、指派、评论
│ [ ] 测试             │
└───────────────────┘
```

#### 核心特性

| 特性 | 详情 |
|------|------|
| **最大子 Issue 数** | 每个父 Issue 最多 **100 个** |
| **嵌套层级** | 最多 **8 层**（子 Issue 可再包含子 Issue） |
| **进度追踪** | 子 Issue 完成 → 父 Issue **自动更新进度条** |
| **Projects 集成** | 可按父 Issue 分组、筛选、查看进度 |
| **跨仓库** | 可以从不同仓库添加子 Issue |
| **CLI 支持** | `gh issue create --parent <issue>` |

#### 创建 Sub-issue

**方式一：创建新子 Issue**
```
1. 打开父 Issue → 滚动到底部
2. 点击 "Create sub-issue"
3. 输入标题 → 设置负责人/标签/里程碑
4. 勾选 "Create more sub-issues" 可连续创建
5. 点击 "Create"
```

**方式二：添加已有 Issue**
```
1. 打开父 Issue → 点击 "Create sub-issue" 旁边的 ▼
2. 选择 "Add existing issue"
3. 搜索并选择要设为子 Issue 的问题
4. 支持跨仓库添加
```

#### 使用场景

- 🎯 **大型功能拆解**：将一个大功能拆分为多个小任务，独立跟踪
- 🔗 **跨仓库协作**：前端仓库 + 后端仓库 + API 仓库的 Issue 可统一管理
- 📋 **项目管理**：Epic → Feature → Task 的分层结构
- 📊 **进度可视化**：在 Projects 中按父 Issue 分组，一目了然

### Issue Types（问题类型）

与 Sub-issues 同时 GA 的另一功能，为 Issue 赋予内置类型：

| 类型 | 用途 | 示例 |
|------|------|------|
| 🐛 **Bug** | 缺陷报告 | "登录按钮在 Safari 上无反应" |
| ✨ **Feature** | 新功能 | "添加深色模式支持" |
| 📋 **Task** | 普通任务 | "更新依赖版本" |
| 🏗️ **Epic** | 大型工作 | "用户系统重构（包含多个 Feature）" |

**与 Sub-issues 配合使用的最佳实践**：
```
Epic: 用户系统重构
├─ Feature: 统一登录接口
│  ├─ Task: 后端 API 开发
│  ├─ Bug: Safari 兼容性问题
│  └─ Task: 测试用例编写
└─ Feature: 权限管理
   └─ Task: 数据库设计
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