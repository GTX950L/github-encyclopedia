# 第11章：Projects 与项目管理 📊

> GitHub Projects 是内置的项目管理工具，类似 Trello。本章教你如何使用它。

## 📋 目录

- [什么是 Projects？](#什么是-projects)
- [创建 Project](#创建-project)
- [使用看板（Kanban）](#使用看板kanban)
- [自动化工作流](#自动化工作流)
- [常见问题](#常见问题)

---

## 什么是 Projects？

### 基本定义

**Projects（项目）** 是用看板方式管理 Issues 和 PR 的工具。

### 比喻

```
传统的 Issue 列表：
│
├── Issue #1: 添加登录功能
├── Issue #2: 修复登录 Bug
├── Issue #3: 添加注册功能
└── Issue #4: 修复注册 Bug

→ 分不清哪些在做，哪些已完成...

使用 Projects 看板：
│
├── 📋 Todo（待办）
│   └── Issue #3: 添加注册功能
│
├── 🔄 In Progress（进行中）
│   └── Issue #1: 添加登录功能
│
├── 🔍 Review（审查中）
│   └── PR #5: 实现登录功能
│
└── ✅ Done（已完成）
    ├── Issue #2: 修复登录 Bug
    └── Issue #4: 修复注册 Bug

→ 一目了然！
```

---

## 创建 Project

### 步骤详解

#### 第1步：进入 Projects 页面

```
方法1：仓库页面 → Projects 标签 → New project
方法2：https://github.com/用户名/仓库名/projects/new
```

#### 第2步：选择模板

| 模板 | 适用场景 |
|------|----------|
| **Board（看板）** | 适合敏捷开发、任务跟踪 |
| **Table（表格）** | 适合需要筛选、排序的场景 |
| **Roadmap（路线图）** | 适合规划长期目标 |
| **Bug triage（Bug 分类）** | 适合管理 Bug 报告 |

**推荐**：选择 **Board（看板）** ✅

#### 第3步：配置 Project

```
填写信息：
│
├── Project name: My Project
├── Description: 管理我的项目任务
└── Template: Board（看板）
```

---

## 使用看板（Kanban）

### 基本操作

#### 1️⃣ 添加 Issue 到看板

```
方法1：在 Project 页面点击 "Add item"
│
├── 选择已有的 Issue/PR
└── 或创建新的 Issue

方法2：在 Issue 页面，点击右侧 "Projects"
│
└── 选择要添加的 Project
```

---

#### 2️⃣ 移动 Issue（拖拽）

```
在看板中，直接拖拽 Issue 卡片：
│
├── 从 Todo → In Progress（开始做）
├── 从 In Progress → Review（提交审查）
└── 从 Review → Done（完成！）
```

---

#### 3️⃣ 添加自定义字段

**为什么需要？**

默认的看板只有"状态"，你可能需要：
- 📅 **截止日期**（Due date）
- 🔥 **优先级**（Priority：High/Medium/Low）
- 💰 **工作量**（Effort：Small/Medium/Large）
- 🏷️ **类型**（Type：Bug/Feature/Docs）

**如何添加？**

```
1. 在 Project 页面，点击右上角 ⚙️（Settings）
2. 点击 "Add field"
3. 选择字段类型：
   ├── Text（文本）
   ├── Single select（单选）✅ 推荐
   ├── Number（数字）
   ├── Date（日期）
   └── Iteration（迭代）
4. 配置选项
5. 保存
```

**示例：添加"优先级"字段**

```
Field name: Priority
Field type: Single select
Options:
├── 🔴 High
├── 🟡 Medium
└── 🔵 Low
```

---

## 自动化工作流

### 为什么需要自动化？

**手动操作太麻烦！**

```
❌ 不用自动化：
│
├── 每次提交 PR，手动移动到 Review 列
├── 每次 PR 合并，手动移动到 Done 列
└── 重复劳动...

✅ 使用自动化：
│
├── 提交 PR → 自动移动到 Review 列
├── PR 合并 → 自动移动到 Done 列
└── 自动完成！
```

---

### 如何配置自动化？

#### 第1步：进入自动化设置

```
Project 页面 → ⚙️ Settings → Workflows → Add workflow
```

#### 第2步：选择触发器（Trigger）

| 触发器 | 说明 |
|----------|------|
| **Item added to project** | 当 Issue/PR 被添加到 Project |
| **Item reopened** | 当 Issue/PR 被重新打开 |
| **Pull request merged** | 当 PR 被合并 |
| **Pull request closed** | 当 PR 被关闭 |

#### 第3步：选择动作（Action）

| 动作 | 说明 |
|--------|------|
| **Set field value** | 设置字段值（如：状态 = Done） |
| **Remove from project** | 从 Project 中移除 |
| **Send Slack notification** | 发送 Slack 通知 |

---

### 示例：PR 合并后自动移动到 Done

```
Trigger: Pull request merged
│
Action: Set field value
│
├── Field: Status
└── Value: ✅ Done
```

---

## 常见问题

### Q1: Project 和 Milestone 有什么区别？

| 功能 | Project | Milestone |
|------|---------|-----------|
| **可视化** | ✅ 看板、表格 | ❌ 只有列表 |
| **自定义字段** | ✅ 支持 | ❌ 不支持 |
| **自动化** | ✅ 支持 | ❌ 不支持 |
| **适合** | 日常任务管理 | 版本发布规划 |

**建议**：
- 使用 **Milestone** 规划版本（v1.0.0、v2.0.0）
- 使用 **Project** 管理日常任务

---

### Q2: 可以多个仓库共用一个 Project 吗？

**A**: ✅ **可以！**

**如何创建组织级 Project？**

```
1. 访问组织页面（或你的个人页面）
2. 点击 Projects → New project
3. 在创建时选择 "Organization" 而非 "Repository"
```

这样，所有仓库的 Issue 和 PR 都可以添加到这个 Project。

---

### Q3: 如何导出 Project 数据？

**A**: GitHub 原生不支持导出，但可以使用浏览器扩展或脚本。

**方法1：使用 GitHub CLI**

```bash
# 导出 Project 为 JSON
gh project item-list 1 --format json > project.json
```

**方法2：使用浏览器扩展**

- [GitHub Project Exporter](https://chrome.google.com/webstore/)（Chrome 扩展）

---

## 📝 实战练习

### 练习1：创建你的第一个 Project

**任务**：
1. 在一个仓库中创建 Project（选择 Board 模板）
2. 添加 3 个 Issue 到看板
3. 创建 Todo、In Progress、Review、Done 四个列
4. 拖拽 Issue，模拟工作流

---

### 练习2：添加自定义字段

**任务**：
1. 在 Project 中添加"优先级"字段
2. 配置选项：High、Medium、Low
3. 为每个 Issue 设置优先级

---

### 练习3：配置自动化

**任务**：
1. 配置工作流：当 PR 被合并，自动设置状态为 Done
2. 测试：提交一个 PR 并合并
3. 查看 Project，确认自动化生效

---

## 📚 延伸阅读

- [第10章：Issues 使用指南](chapter10-issues-guide.md) - 创建和管理 Issues
- [第9章：Fork 与 Pull Request](chapter09-fork-pr.md) - 提交 PR
- [第28章：企业级最佳实践](chapter28-best-practices.md) - 大型项目的协作流程

---

## 📝 本章小结

你现在学会了：

- ✅ 什么是 Projects，以及它的作用
- ✅ 如何创建和配置 Project
- ✅ 如何使用看板管理任务
- ✅ 如何添加自定义字段
- ✅ 如何配置自动化工作流

### 💡 核心要点

> **Projects 让项目管理可视化。**
> 
> 使用看板，你可以一眼看出：
> - 哪些任务在做
> - 哪些任务完成了
> - 项目进度如何

---

<p align="right">
  <a href="chapter10-issues-guide.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter12-wiki-documentation.md">下一章 →</a>
</p>