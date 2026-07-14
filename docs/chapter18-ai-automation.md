# 第18章：让 AI 操控你的 GitHub 🤖

> 有了 Token，你可以让 AI 助手（如 WorkBuddy、Copilot）帮你管理 GitHub 项目。

## 📋 目录

- [AI 可以做什么？](#ai-可以做什么)
- [使用 WorkBuddy 管理 GitHub](#使用-workbuddy-管理-github)
- [使用 GitHub Copilot](#使用-github-copilot)
- [自己编写 AI 脚本](#自己编写-ai-脚本)
- [常见问题](#常见问题)

---

## AI 可以做什么？

### 自动化任务清单

| 任务 | AI 可以... |
|------|------------|
| **创建 Issue** | ✅ 根据你的描述自动创建 |
| **审查 PR** | ✅ 自动检查代码质量 |
| **生成文档** | ✅ 根据代码自动生成 API 文档 |
| **回复 Issue** | ✅ 自动回答常见问题 |
| **合并 PR** | ✅ 自动合并简单的 PR |
| **生成报告** | ✅ 统计项目数据 |

---

## 使用 WorkBuddy 管理 GitHub

### 配置步骤

#### 第1步：连接 GitHub

```
1. 打开 WorkBuddy 设置
2. 找到 连接器（Connectors）
3. 选择 GitHub
4. 点击 连接
```

#### 第2步：授权

```
方法1：浏览器授权（推荐）
│
├── WorkBuddy 会打开 GitHub 授权页面
├── 登录 GitHub
├── 点击 Authorize
└── 完成！

方法2：手动输入 Token
│
├── 在 GitHub 生成 Token（见第17章）
├── 复制 Token
└── 粘贴到 WorkBuddy
```

#### 第3步：开始使用

现在你可以对 WorkBuddy 说：

```
✅ 可以做的：
│
├── "帮我创建一个 Issue：修复登录 Bug"
├── "查看我的所有仓库"
├── "把这个代码提交到 GitHub"
├── "创建一个 PR"
├── "查看最近的提交记录"
└── "给 vuejs/core 点个 Star"
```

---

### 示例：让 AI 创建 Issue

**你对 WorkBuddy 说**：

```
帮我在 my-project 仓库创建一个 Issue：

标题：登录按钮在 Safari 上无法点击

描述：
- 复现步骤：用 Safari 打开网站，点击登录按钮，没有反应
- 期望：点击后弹出登录框
- 环境：macOS Sonoma, Safari 17
```

**AI 会执行**：

```bash
curl -X POST \
  -H "Authorization: Bearer YOUR_TOKEN" \
  https://api.github.com/repos/你的用户名/my-project/issues \
  -d '{"title":"登录按钮在 Safari 上无法点击","body":"..."}'
```

**结果**：Issue 创建成功！🎉

---

## 使用 GitHub Copilot

### 什么是 GitHub Copilot？

**GitHub Copilot** 是 GitHub 官方的 AI 编程助手。

| 版本 | 适用场景 | 价格 |
|------|---------|------|
| **Copilot Individual** | 个人开发者 | $10/月（有免费试用） |
| **Copilot Business** | 企业团队 | $19/用户/月 |
| **Copilot Enterprise** | 大型企业 | $39/用户/月 |
| **Copilot Free** | 个人学习 | ✅ 免费 |

---

### Copilot 的各个功能

#### 1️⃣ 代码补全（Code Completion）

Copilot 最基础的功能。你写注释或代码开头，它自动补全剩余部分。

```
// 在 VS Code 中输入：
function calculateTotal(prices) {
    // Copilot 会自动补全：
    return prices.reduce((sum, price) => sum + price, 0);
}
```

**使用技巧**：
- ✍️ **写清晰的注释** — Copilot 根据注释生成代码
- 🧩 **写函数签名** — 好的参数名 + 返回类型，Copilot 更准确
- 🔍 **打开相关文件** — Copilot 会参考你打开的其他文件

#### 2️⃣ Copilot Chat（对话式 AI）

**Copilot Chat** 让你可以和 AI 对话，而不仅仅是补全代码。

```
在 VS Code 中按 Ctrl+Shift+I 打开 Chat：

你：这段代码有什么问题？
Copilot：第5行变量名拼写错误，第12行缺少空指针检查...

你：帮我写一个二分查找的测试用例
Copilot：以下是使用 pytest 的测试用例...
```

**Chat 能做的事情**：
- 🔍 **代码审查**：选中代码 → 让 Copilot 审查
- 📖 **代码解释**：选中看不懂的代码 → 让 Copilot 解释
- 🐛 **调试帮助**：粘贴报错 → 让 Copilot 分析原因
- ♻️ **重构建议**：选中代码 → 让 Copilot 提改善方案

#### 3️⃣ Copilot Workspace（AI 工作区）

**Copilot Workspace** 是一个浏览器内的 AI 开发环境（预览功能）。

```
与传统开发的对比：

传统开发：
1. 手动创建 Issue
2. 手动创建分支
3. 手动写代码
4. 手动提交 PR

Copilot Workspace：
1. 选择 Issue
2. AI 分析 Issue 并生成修改方案
3. 你审查并微调 AI 的修改
4. 一键提交 PR
```

#### 4️⃣ 内联代码审查（Code Review）

在 PR 页面，Copilot 可以自动审查代码变更：

```
1. 打开一个 PR 页面
2. 在 "Files changed" 标签页
3. 点击 Copilot 图标
4. Copilot 会分析所有变更，给出审查意见

审查内容包括：
- 潜在的 Bug
- 安全漏洞
- 性能问题
- 代码风格建议
```

---

### Copilot 使用技巧

#### 技巧1：用注释引导 AI

```python
# ❌ 不好：没有注释，Copilot 猜不准
def process(data):
    pass

# ✅ 好：注释清晰，Copilot 知道你要什么
def process(data):
    """清洗数据：去除空值、标准化格式、排序"""
    # Copilot 会生成：
    data = [d for d in data if d is not None]
    data = [normalize(d) for d in data]
    return sorted(data)
```

#### 技巧2：利用上下文窗口

Copilot 会参考你当前打开的**所有文件**来生成代码。所以：
- ✅ 打开相关文件（接口定义、测试文件）
- ❌ 关掉不需要的文件（避免干扰）

#### 技巧3：多个建议中选择

```python
# 按 Ctrl+Enter 查看更多建议
# 然后选择最合适的
```

#### 技巧4：用 Copilot 写测试

```
# 给一个函数写测试
def test_calculate_total():
    # Copilot 会自动补全测试用例
    assert calculate_total([10, 20, 30]) == 60
    assert calculate_total([]) == 0
    assert calculate_total([-5, 10]) == 5
```

---

## 自己编写 AI 脚本

### 示例：让 AI 自动审查 PR

```python
import requests
import openai  # 假设使用 OpenAI API

# 1. 获取 PR 的代码
pr_number = 123
repo = "用户名/仓库名"
token = "ghp_你的token"

headers = {"Authorization": f"Bearer {token}"}

# 获取 PR 的文件变更
response = requests.get(
    f"https://api.github.com/repos/{repo}/pulls/{pr_number}/files",
    headers=headers
)
files = response.json()

# 2. 让 AI 审查代码
for file in files:
    code = file["patch"]
    prompt = f"请审查以下代码变更，指出潜在问题：\n{code}"
    
    ai_response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}]
    )
    
    review = ai_response["choices"][0]["message"]["content"]
    
    # 3. 将审查意见提交到 PR
    comment_data = {"body": review}
    requests.post(
        f"https://api.github.com/repos/{repo}/pulls/{pr_number}/comments",
        headers=headers,
        json=comment_data
    )
```

---

### 示例：让 AI 自动回复 Issue

```python
# 获取新 Issue
response = requests.get(
    f"https://api.github.com/repos/{repo}/issues?state=open",
    headers=headers
)
issues = response.json()

for issue in issues:
    title = issue["title"]
    body = issue["body"]
    
    # 让 AI 生成回复
    prompt = f"这是一个开源项目的 Issue：\n标题：{title}\n描述：{body}\n\n请生成一个友好的回复。"
    
    ai_response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}]
    )
    
    reply = ai_response["choices"][0]["message"]["content"]
    
    # 提交回复
    issue_number = issue["number"]
    requests.post(
        f"https://api.github.com/repos/{repo}/issues/{issue_number}/comments",
        headers=headers,
        json={"body": reply}
    )
```

---

## 常见问题

### Q1: AI 会自动合并 PR 吗？会不会有风险？

**A**: 

**可以配置，但要小心！**

```
✅ 安全的做法：
│
├── AI 只负责审查（提意见）
├── 人类决定是否合并
└── 或者：AI 只合并"绿色"的 PR（所有测试通过）

❌ 危险的做法：
│
├── AI 自动合并所有 PR
└── 可能合并恶意代码！
```

**建议**：

```
AI 权限设置：
│
├── ✅ 允许：创建 Issue、评论、审查
├── ⚠️ 谨慎：合并 PR（只合并测试通过的）
└── ❌ 禁止：删除仓库、修改权限
```

---

### Q2: AI 操作会消耗 Token 配额吗？

**A**: 

| 操作 | 是否消耗 API 配额？ |
|------|----------------------|
| **WorkBuddy 操作** | ✅ 会（使用你的 AI 配额） |
| **GitHub Copilot** | ✅ 会（需要订阅） |
| **自己写的脚本** | ✅ 会（如果使用 GPT-4 等付费 API） |

**节省配额的方法**：

```
1. 缓存 AI 响应（避免重复请求）
2. 使用更便宜的模型（如 GPT-3.5 代替 GPT-4）
3. 只在必要时调用 AI
```

---

### Q3: 可以让 AI 自动给开源项目贡献代码吗？

**A**: ⚠️ **技术上可以，但不推荐！**

**原因**：

```
❌ 为什么不好：
│
├── 开源贡献的价值在于"人类智慧"
├── AI 生成的代码可能质量不高
├── 维护者不喜欢"机器人 PR"
└── 可能被社区拉黑
```

**正确的做法**：

```
✅ AI 辅助，人类审查：
│
├── 你：用 AI 生成代码草稿
├── 你：审查和改进 AI 的代码
├── 你：自己提交 PR
└── 你：在 PR 中说明"使用了 AI 辅助"
```

---

## 📝 实战练习

### 练习1：使用 WorkBuddy 创建 Issue

**任务**：
1. 连接 WorkBuddy 到你的 GitHub
2. 让 AI 帮你在项目创建一个 Issue
3. 检查 Issue 是否创建成功

---

### 练习2：使用 Copilot 生成提交信息

**任务**：
1. 安装 GitHub Copilot
2. 修改一些代码
3. 使用 Copilot 生成提交信息
4. 提交

---

### 练习3：编写简单的 AI 脚本

**任务**：
1. 写一个 Python 脚本
2. 使用 GitHub API 获取你仓库的所有 Issue
3. 让 AI 总结这些 Issue 的主题
4. 输出总结报告

---

## 📚 延伸阅读

- [第17章：GitHub Token 完全指南](chapter17-token-guide.md) - 生成 Token 的详细步骤
- [第27章：GitHub API 详解](chapter27-github-api.md) - 使用 API 开发工具
- [第19章：GitHub Actions 入门](chapter19-actions-intro.md) - 自动化工作流

---

## 📝 本章小结

你现在了解了：

- ✅ AI 可以帮你做哪些 GitHub 任务
- ✅ 如何配置 WorkBuddy 管理 GitHub
- ✅ GitHub Copilot 的各个功能（补全、Chat、Workspace、Review）
- ✅ Copilot 使用技巧和最佳实践
- ✅ 如何自己编写 AI 脚本
- ✅ 安全风险和注意事项

### 💡 核心要点

> **AI 是助手，不是替代品。**
> 
> 让 AI 帮你处理重复性任务，但关键的代码审查和决策，还是要人类来做。
> 
> **Copilot 推荐配置**：Copilot Free（免费版）足够个人日常使用，Copilot Chat 是提升效率的关键功能。

---

<p align="right">
  <a href="chapter17-token-guide.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter19-actions-intro.md">下一章 →</a>
</p>