# 贡献指南 🤝

感谢你对 **GitHub 百科全书** 项目的关注！这份文档将指导你如何为项目做出贡献。

---

## 📋 目录

- [如何贡献](#如何贡献)
- [报告 Bug](#报告-bug)
- [提出新功能](#提出新功能)
- [提交代码](#提交代码)
- [写作规范](#写作规范)
- [社区准则](#社区准则)

---

## 如何贡献

有很多方式可以为本项目做出贡献：

- 🐛 **报告 Bug**：发现错误？请告诉我们！
- ✨ **提出新功能**：有好的想法？和我们讨论！
- 📝 **改进文档**：修正错别字、补充内容
- 🌍 **翻译**：帮助翻译成其他语言
- 💻 **改进代码**：优化网站、添加新功能（如果有的话）

---

## 报告 Bug

### 在报告之前

1. 🔍 **搜索已有的 Issues**，确认没有重复
2. 📝 **使用最新的代码**，确认 Bug 仍然存在

### 如何提交 Bug Report

**请使用 [Bug Report 模板](https://github.com/GTX950L/github-encyclopedia/issues/new?template=bug_report.md)**（如果配置了的话），或者手动创建 Issue 并包含以下信息：

```markdown
## 🐛 Bug 描述

简要描述这个 Bug。

## 🔄 复现步骤

1. 打开 '...'
2. 点击 '...'
3. 看到错误

## ✅ 预期行为

描述你期望发生的事情。

## 📷 截图

如果适用，添加截图帮助解释问题。

## 💻 环境信息

- 操作系统：[如 Windows 11, macOS Sonoma]
- 浏览器：[如 Chrome 120, Firefox 115]
- 屏幕分辨率：[如 1920x1080]

## 📝 其他信息

任何其他有助于解决问题的信息。
```

---

## 提出新功能

### 在提出之前

1. 🔍 **搜索已有的 Issues 和 Discussions**，确认没有重复
2. 💭 **考虑范围**：这个功能是否符合项目的目标？

### 如何提出新功能

**请使用 [Feature Request 模板](https://github.com/GTX950L/github-encyclopedia/issues/new?template=feature_request.md)**，或者手动创建 Issue 并包含以下信息：

```markdown
## ✨ 功能描述

简要描述这个新功能。

## 🎯 目标

这个功能是为了解决什么问题？

## 💡 建议的解决方案

描述你希望的解决方案。

## 🔄 替代方案

描述其他考虑过的方案。

## 📝 其他信息

任何其他有助于实现这个功能的信息。
```

---

## 提交代码

### 开发流程

#### 第1步：Fork 本项目

```
1. 访问 https://github.com/GTX950L/github-encyclopedia
2. 点击右上角的 Fork 按钮
3. 等待 GitHub 复制仓库到你的账号
```

#### 第2步：克隆你的 Fork

```bash
git clone https://github.com/你的用户名/github-encyclopedia.git
cd github-encyclopedia
```

#### 第3步：创建分支

```bash
# 从 main 分支创建新分支
git checkout -b fix/chapter-05-typo

# 分支命名建议：
# - fix/xxx：修复 Bug
# - feat/xxx：新功能
# - docs/xxx：文档更新
# - chore/xxx：其他改动
```

#### 第4步：进行修改

```bash
# 进行修改...
# 使用你喜欢的编辑器

# 预览修改（如果是 Markdown 文件）
# 可以使用任何 Markdown 编辑器预览
```

#### 第5步：提交修改

```bash
# 查看修改
git status
git diff

# 添加修改的文件
git add docs/chapter05-download-code.md

# 提交（使用有意义的提交信息！）
git commit -m "fix: 修正第5章中的错别字"

# 提交信息格式建议：
# feat: 添加新章节
# fix: 修复错别字
# docs: 更新文档
# chore: 更新依赖
```

#### 第6步：推送到你的 Fork

```bash
git push origin fix/chapter-05-typo
```

#### 第7步：创建 Pull Request

```
1. 访问你的 Fork 页面
2. GitHub 会提示 "Compare & pull request"
3. 点击，填写 PR 描述
4. 提交 PR
```

---

## 写作规范

### 📝 Markdown 格式

本项目使用 Markdown 格式编写，请确保你的修改符合以下规范：

#### 标题层级

```markdown
# 一级标题（章节标题）

## 二级标题（主要小节）

### 三级标题（子小节）

#### 四级标题（细节说明）
```

#### 代码块

````markdown
```bash
# Bash 代码
git clone https://github.com/...
```

```python
# Python 代码
def hello():
    print("Hello, World!")
```
````

#### 表格

```markdown
| 列1 | 列2 | 列3 |
|------|------|------|
| 内容 | 内容 | 内容 |
```

#### 列表

```markdown
- 无序列表项1
- 无序列表项2
  - 子项

1. 有序列表项1
2. 有序列表项2
```

---

### 📖 内容规范

#### 语言风格

- ✅ **使用中文**，专业术语可保留英文原文（如 Commit、Push）
- ✅ **简洁明了**，避免啰嗦
- ✅ **使用主动语态**
- ❌ **避免使用** "想必大家都知道"、"显而易见" 等表述

#### 示例和截图

- ✅ **提供实际示例**，帮助理解
- ✅ **截图要清晰**，关键信息用红框标注
- ✅ **示例代码要可运行**，不要有语法错误

#### 链接

- ✅ **使用相对链接** 引用本项目内的其他章节
  - 正确：`[第5章](chapter05-download-code.md)`
  - 错误：`[第5章](https://github.com/...)`
- ✅ **外部链接** 使用完整 URL

---

### 🖼️ 图片规范

如果需要添加图片：

1. **放在 `images/` 目录**
2. **使用有意义的文件名**：`chapter03-interface-overview.png`
3. **压缩图片**：使用 [TinyPNG](https://tinypng.com/) 压缩 PNG/JPG
4. **添加 alt 文本**：

```markdown
![GitHub 主页界面](images/chapter03-dashboard.png)
```

---

## 社区准则

### 📜 Code of Conduct

请阅读 [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) 了解我们的社区准则。

### ✅ 我们的标准

- 🌍 **友善和尊重**：尊重不同观点和经验
- 💬 **接受建设性批评**：客观回应反馈
- 📚 **关注什么对用户最好**：我们的首要任务是帮助学习者
- 🤝 **展现同理心**：记住，不是每个人都有相同的经验

### ❌ 不可接受的行为

- ❌ 使用性暗示语言或图像
- ❌ 挑衅、侮辱或贬低性评论
- ❌ 公开或私下骚扰
- ❌ 未经明确许可，发布他人的私人信息
- ❌ 其他不专业的行为

---

## 📧 联系我们

如果你有任何问题，欢迎通过以下方式联系我们：

- 🐛 **报告 Bug**：[创建 Issue](https://github.com/GTX950L/github-encyclopedia/issues)
- 💬 **讨论交流**：[GitHub Discussions](https://github.com/GTX950L/github-encyclopedia/discussions)
- 📧 **邮件**：在 Issue 中留言（为保护隐私，不公开邮箱）

---

## 🎉 致谢

感谢所有贡献者！你们的帮助让这个项目变得更好。

**首次贡献者**：不要害怕犯错，我们会友好地帮助你改进。

---

<p align="center">
  期待你的贡献！ 💖
</p>