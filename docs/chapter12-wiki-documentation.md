# 第12章：Wiki 与文档 📖

> 好的文档能让项目更容易被使用。本章教你如何使用 GitHub Wiki 和编写项目文档。

## 📋 目录

- [什么是 Wiki？](#什么是-wiki)
- [启用和编辑 Wiki](#启用和编辑-wiki)
- [编写好的文档](#编写好的文档)
- [使用 Markdown](#使用-markdown)
- [常见问题](#常见问题)

---

## 什么是 Wiki？

### 基本定义

**Wiki** 是项目的内置文档系统，适合编写详细的使用手册、API 文档等。

### Wiki vs README

| 特性 | README.md | Wiki |
|------|-------------|------|
| **位置** | 仓库根目录 | 单独的 Wiki 标签页 |
| **适合** | 项目简介、快速开始 | 详细文档、教程 |
| **长度** | 短（1-3 屏） | 长（多页） |
| **维护** | 随代码一起更新 | 独立维护 |

### 何时使用 Wiki？

```
✅ 适合用 Wiki：
├── 详细的用户手册
├── API 文档
├── 教程和示例
└── FAQ

❌ 不适合用 Wiki（应该用 README）：
├── 项目简介
├── 安装步骤
└── 快速开始
```

---

## 启用和编辑 Wiki

### 启用 Wiki

```
1. 进入仓库页面
2. 点击 Settings
3. 勾选 "Wikis" 功能
4. 保存
```

### 创建 Wiki 页面

#### 方法1：在网页上编辑（推荐新手）✅

```
1. 点击仓库的 "Wiki" 标签
2. 点击 "Create the first page"
3. 填写页面内容（使用 Markdown）
4. 点击 "Save Page"
```

#### 方法2：克隆 Wiki 到本地

Wiki 本身也是一个 Git 仓库！

```bash
# 克隆 Wiki 仓库
git clone https://github.com/用户名/仓库名.wiki.git

# 创建新页面（就是一个 Markdown 文件）
echo "# 安装指南" > 安装指南.md

# 提交并推送
git add .
git commit -m "Add: 添加安装指南"
git push origin main
```

---

## 编写好的文档

### ✅ 好的文档应该...

| 特点 | 说明 | 示例 |
|------|------|------|
| **结构清晰** | 使用标题、列表、表格 | 有目录、分章节 |
| **图文并茂** | 添加截图、GIF | 展示操作步骤 |
| **有示例** | 提供可运行的代码 | "复制这段代码..." |
| **保持更新** | 代码更新时同步更新文档 | 版本对应 |

---

### 文档结构模板

```markdown
# 项目名称 文档

## 📋 目录

1. [快速开始](#快速开始)
2. [安装](#安装)
3. [基本用法](#基本用法)
4. [API 参考](#api-参考)
5. [常见问题](#常见问题)
6. [贡献指南](#贡献指南)

---

## 🚀 快速开始

### 前置条件

- Node.js 18+
- npm 9+

### 30 秒上手

```bash
# 安装
npm install my-lib

# 运行
npm start
```

---

## 📦 安装

### npm

```bash
npm install my-lib
```

### yarn

```bash
yarn add my-lib
```

---

## 📖 基本用法

### 示例1：Hello World

```javascript
import { hello } from 'my-lib';

console.log(hello());  // 输出：Hello, World!
```

---

## 📚 API 参考

### `hello(name?)`

返回一个问候语。

**参数：**
- `name` (string, 可选): 名字

**返回：**
- (string): 问候语

**示例：**

```javascript
hello();        // "Hello, World!"
hello("张三");  // "Hello, 张三!"
```

---

## ❓ 常见问题

### Q: 支持 TypeScript 吗？

A: ✅ 支持！已内置类型定义。

---

## 🤝 贡献

欢迎贡献！请查看 [CONTRIBUTING.md](../CONTRIBUTING.md)。
```

---

## 使用 Markdown

### 常用语法

#### 标题

```markdown
# 一级标题（文档标题）
## 二级标题（主要章节）
### 三级标题（子章节）
#### 四级标题（细节说明）
```

#### 代码块

````markdown
```javascript
function hello() {
    console.log("Hello!");
}
```
````

#### 表格

```markdown
| 参数 | 类型 | 说明 |
|------|------|------|
| `name` | string | 用户名 |
| `age` | number | 年龄 |
```

#### 提示框

```markdown
> 💡 **提示：** 这是一个提示。

> ⚠️ **警告：** 这是一个警告。

> 📝 **注意：** 这是一个注意事项。
```

#### 折叠内容

```markdown
<details>
<summary>点击展开</summary>

隐藏的内容...

</details>
```

---

## 常见问题

### Q1: Wiki 可以被搜索引擎收录吗？

**A**: ✅ **可以**，但 SEO 权重通常比独立网站低。

**建议**：
- 如果是开源项目，Wiki 够了
- 如果需要更好的 SEO，使用 [GitHub Pages](chapter20-pages-website.md) 搭建独立文档网站

---

### Q2: 可以禁用 Wiki 吗？

**A**: ✅ **可以！**

```
Settings → Features → 取消勾选 "Wikis"
```

---

### Q3: 如何添加 Wiki 首页？

**A**: 创建名为 `Home` 的页面，它就是 Wiki 的首页。

---

## 📝 实战练习

### 练习1：为你的项目创建 Wiki

**任务**：
1. 启用 Wiki
2. 创建 `Home` 页面（项目简介）
3. 创建 `安装指南` 页面
4. 创建 `API 文档` 页面

---

### 练习2：克隆 Wiki 到本地

**任务**：
1. 克隆 Wiki 仓库
2. 在本地创建新页面
3. 提交并推送
4. 在网页上查看效果

---

## 📚 延伸阅读

- [第15章：README 编写艺术](chapter15-readme-art.md) - 编写吸引人的 README
- [第20章：GitHub Pages 搭建网站](chapter20-pages-website.md) - 搭建独立文档网站
- [附录A：项目结构](appendix-a-project-structure.md) - 了解项目需要哪些文件

---

## 📝 本章小结

你现在学会了：

- ✅ 什么是 Wiki，以及何时使用
- ✅ 如何启用和编辑 Wiki
- ✅ 如何编写好的文档
- ✅ 如何使用 Markdown 语法

### 💡 核心要点

> **文档和代码一样重要！**
> 
> 代码写得再好，没有人会用也是白搭。

---

<p align="right">
  <a href="chapter11-projects-management.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter13-star-meaning.md">下一章 →</a>
</p>