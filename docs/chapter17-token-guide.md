# 第17章：GitHub Token 完全指南 🔑

> Token 是让 AI、脚本、命令行访问你 GitHub 账号的"钥匙"。本章详解如何安全生成和使用 Token。

## 📋 目录

- [什么是 Token？](#什么是-token)
- [为什么需要 Token？](#为什么需要-token)
- [Token 的类型](#token-的类型)
- [生成 Personal Access Token](#生成-personal-access-token)
- [使用 Token 的场景](#使用-token-的场景)
- [让 AI 操控你的 GitHub](#让-ai-操控你的-github)
- [Token 的安全管理](#token-的安全管理)
- [常见问题](#常见问题)

---

## 什么是 Token？

**Token（令牌）** 是一个字符串，相当于你的"临时密码"。

### 传统方式 vs Token 方式

| 方式 | 优点 | 缺点 |
|------|------|------|
| **密码** | 简单 | ❌ 不安全，❌ 无法精细控制权限 |
| **Token** | ✅ 精细权限控制，✅ 可随时撤销，✅ 设置过期时间 | 需要额外步骤生成 |

### Token 的样子

```
ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

- 以 `ghp_` 开头（Personal Access Token）
- 通常 40 个字符
- **像密码一样保密！**

---

## 为什么需要 Token？

### 场景1：命令行操作

想用 Git 命令推送代码，但不想每次都输密码？

```bash
# 没有 Token，每次都要输入用户名和密码
git push origin main
Username: your_username
Password: ********

# 有了 Token，可以缓存凭证
git push origin main
# 使用 Token 作为密码，一次配置，永久使用
```

### 场景2：让 AI 帮你管理项目

想让 AI 助手（如 WorkBuddy、Copilot）自动：
- 创建 Issue
- 提交 PR
- 管理仓库设置

**这就需要 Token！**

### 场景3：自动化脚本

想写脚本自动：
- 备份仓库
- 批量星标项目
- 生成统计报告

**Token 是脚本访问 GitHub API 的凭证。**

---

## Token 的类型

GitHub 提供两种 Token：

### 1️⃣ Personal Access Token (PAT)

**适用场景**：个人使用、测试、AI 助手。

#### Fine-grained PAT（细粒度令牌）✅ 推荐

**优点**：
- ✅ 可以精确控制权限（只给需要的权限）
- ✅ 可以限定只能访问特定仓库
- ✅ 可以设置过期时间

**生成步骤**：见下方 [生成 Fine-grained Token](#生成-fine-grained-token)

#### Classic PAT（经典令牌）

**优点**：简单，兼容性好  
**缺点**：权限控制粗糙（要么全有，要么全无）

### 2️⃣ GitHub App Token

**适用场景**：为组织、团队创建自动化工具。

**说明**：本书不深入讨论，感兴趣的读者可以查看 [GitHub 官方文档](https://docs.github.com/en/developers/apps)。

---

## 生成 Personal Access Token

### 方法1：Fine-grained Token（推荐）🔒

#### 步骤详解

**第1步**：进入设置页面

1. 点击右上角头像
2. 选择 **Settings**
3. 在左侧菜单找到 **Developer settings**
4. 选择 **Personal access tokens** → **Fine-grained tokens**
5. 点击 **Generate new token**

**第2步**：填写基本信息

```
Token name: WorkBuddy AI Assistant
Expiration: 30 days  (推荐选择短期，到期后再续)
Description: 让 AI 助手帮我管理仓库
```

**第3步**：选择权限（最关键！）

| 权限类别 | 需要的权限 | 说明 |
|----------|------------|------|
| **Repository permissions** | | |
| Contents | Read and write | 读取和提交代码 |
| Issues | Read and write | 创建和管理 Issue |
| Pull requests | Read and write | 创建和合并 PR |
| Actions | Read and write | 管理 GitHub Actions |
| **Account permissions** | | |
| Email addresses | Read-only | 读取邮箱（可选） |

> ⚠️ **原则**：**只给需要的权限！** 不要随意勾选"全部权限"。

**第4步**：选择可访问的仓库

- **Only select repositories**（推荐）：只选择需要 AI 操作的仓库
- **All repositories**：所有仓库（不推荐，安全风险大）

**第5步**：生成并复制

1. 点击页面底部的 **Generate token**
2. **立即复制 Token**！（只显示一次）
3. 保存到安全的地方

```
⚠️ 重要提醒：
Token 只显示一次！
如果忘记了，只能重新生成。
```

---

### 方法2：Classic Token（传统方式）

#### 生成步骤

1. 进入 **Settings** → **Developer settings**
2. 选择 **Personal access tokens** → **Tokens (classic)**
3. 点击 **Generate new token** → **Generate new token (classic)**
4. 勾选权限（scopes）：

| Scope | 说明 | 何时需要 |
|-------|------|----------|
| `repo` | 完整仓库访问权限 | 需要读写私有仓库 |
| `workflow` | 更新 GitHub Actions | 需要修改 Actions 配置 |
| `write:packages` | 上传包 | 需要发布到 GitHub Packages |
| `delete:packages` | 删除包 | 需要删除已发布的包 |
| `admin:org` | 组织管理 | 需要管理组织设置 |

5. 点击 **Generate token**
6. 复制 Token

---

## 使用 Token 的场景

### 场景1：Git 命令行使用 Token

#### 配置 Git 使用 Token

**方法A：使用凭证管理器（推荐）**

```bash
# 配置 Git 使用凭证管理器
git config --global credential.helper manager-core

# 下次 push 时，输入用户名，密码处输入 Token
git push origin main
```

**方法B：直接在 URL 中嵌入 Token（不推荐）**

```bash
# 不推荐！Token 会保存在 Git 历史中
git clone https://你的用户名:ghp_你的token@github.com/用户名/仓库.git
```

> ⚠️ **警告**：不要在代码中硬编码 Token！

---

### 场景2：使用 GitHub CLI

GitHub CLI (`gh`) 是使用 Token 的最简单方式。

#### 安装并登录

```bash
# 1. 安装 GitHub CLI
# macOS: brew install gh
# Windows: winget install GitHub.cli

# 2. 登录
gh auth login

# 选择 GitHub.com
# 选择 HTTPS
# 选择用浏览器登录（会自动配置 Token）
```

#### 使用 `gh` 命令

```bash
# 创建 Issue
gh issue create --title "Bug: 登录失败" --body "描述..."

# 创建 PR
gh pr create --title "添加新功能" --body "说明..."

# 克隆仓库
gh repo clone 用户名/仓库名

# 查看 PR 列表
gh pr list
```

---

### 场景3：调用 GitHub API

#### 使用 Token 访问 API

```bash
# 获取用户信息
curl -H "Authorization: Bearer ghp_你的token" \
  https://api.github.com/user

# 创建一个 Issue
curl -X POST \
  -H "Authorization: Bearer ghp_你的token" \
  -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/repos/用户名/仓库名/issues \
  -d '{"title":"新Issue","body":"描述"}'
```

#### 使用 Python 调用 API

```python
import requests

token = "ghp_你的token"
headers = {
    "Authorization": f"Bearer {token}",
    "Accept": "application/vnd.github.v3+json"
}

# 获取用户信息
response = requests.get("https://api.github.com/user", headers=headers)
print(response.json())

# 创建一个 Issue
data = {
    "title": "新Issue",
    "body": "这是通过 API 创建的 Issue"
}
response = requests.post(
    "https://api.github.com/repos/用户名/仓库名/issues",
    headers=headers,
    json=data
)
print(response.json())
```

---

## 让 AI 操控你的 GitHub

### 为什么需要让 AI 操控？

想象一下：
- 🤖 AI 可以自动审查代码
- 🤖 AI 可以自动回复 Issue
- 🤖 AI 可以自动生成文档
- 🤖 AI 可以自动合并简单的 PR

**这就是 AI 驱动的开发流程！**

---

### 方法1：使用 WorkBuddy 的 GitHub 集成

#### 配置步骤

**第1步**：在 WorkBuddy 中连接 GitHub

1. 打开 WorkBuddy 设置
2. 找到 **连接器（Connectors）**
3. 选择 **GitHub**
4. 点击 **连接**

**第2步**：授权 WorkBuddy 访问你的 GitHub

- WorkBuddy 会引导你生成 Token
- 或者你可以手动粘贴之前生成的 Token

**第3步**：开始使用

现在你可以对 WorkBuddy 说：
- "帮我创建一个 Issue"
- "查看我的所有仓库"
- "把这个代码提交到 GitHub"
- "创建一个 PR"

---

### 方法2：使用 GitHub Copilot

GitHub 官方的 AI 助手。

**功能**：
- ✅ 代码补全
- ✅ 代码解释
- ✅ 生成提交信息
- ✅ 代码审查

**配置**：
1. 安装 GitHub Copilot 扩展（VS Code、JetBrains 等）
2. 登录 GitHub 账号
3. 订阅 Copilot（付费，有免费试用）

---

### 方法3：自己编写 AI 脚本

#### 示例：让 AI 自动审查 PR

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

## Token 的安全管理

### ⚠️ Token 泄露的后果

如果 Token 泄露（比如提交到了公开仓库）：

1. ❌ 攻击者可以访问你的仓库
2. ❌ 攻击者可以冒充你提交代码
3. ❌ 攻击者可以删除你的项目

**真实案例**：
- 2021年，有开发者把 Token 放在代码中传到了 GitHub
- 几分钟后，攻击者用他的 Token 创建了挖矿容器
- 产生了数千美元的费用

### ✅ Token 安全管理原则

#### 1. 使用环境变量

```python
# ❌ 错误做法
token = "ghp_xxxxxxxxxxxxx"

# ✅ 正确做法
import os
token = os.environ.get("GITHUB_TOKEN")
```

#### 2. 使用 .gitignore 排除配置文件

```gitignore
# .gitignore
.env
config.json
secrets/
```

#### 3. 定期轮换 Token

- 每 30-90 天更换一次 Token
- 发现泄露立即撤销

#### 4. 使用最小权限原则

- 只给 Token 必需的权限
- 只让 Token 访问必需的仓库

---

### 如何撤销 Token

**步骤**：

1. 进入 **Settings** → **Developer settings**
2. 选择 **Personal access tokens**
3. 找到要撤销的 Token
4. 点击 **Delete**
5. 确认删除

> 💡 **建议**：给 Token 起有意义的名字，方便管理。比如：
> - `WorkBuddy_AI_2026`
> - `Backup_Script_Prod`

---

## 常见问题

### Q1: Token 过期了怎么办？

**A**: 
1. 如果设置了过期时间，Token 到期后会自动失效
2. 重新生成一个新的 Token
3. 更新所有使用旧 Token 的地方

### Q2: 我可以同时使用多个 Token 吗？

**A**: 可以！建议为不同用途创建不同的 Token：
- 一个给 AI 助手
- 一个给备份脚本
- 一个给 GitHub CLI

这样即使一个 Token 泄露，其他 Token 仍然安全。

### Q3: Fine-grained Token 和 Classic Token 该选哪个？

**A**: 优先选择 **Fine-grained Token**，因为：
- 权限更精细
- 可以限定仓库
- 更安全

只有在 Fine-grained Token 不满足需求时（比如某些旧工具不支持），才使用 Classic Token。

### Q4: 我的 Token 泄露了，该怎么办？

**A**: 
1. ⚠️ **立即撤销 Token**（进入设置删除）
2. 🔍 **检查审计日志**，看是否有异常操作
3. 🔄 **更换所有受影响的仓库的部署密钥**
4. 📧 **通知团队**，如果有协作者

### Q5: 可以让 AI 自动生成 Token 吗？

**A**: **不可以！** Token 必须由你手动生成，然后安全地交给 AI 使用。

AI 可以帮你：
- ✅ 调用 API（如果你提供了 Token）
- ✅ 管理仓库（如果你提供了 Token）

AI 不能帮你：
- ❌ 生成 Token（需要你的账号权限）
- ❌ 自动续期 Token（需要你的授权）

---

## 📝 实战练习

### 练习1：生成你的第一个 Token

**任务**：
1. 生成一个 Fine-grained Token
2. 只给 `repo` 和 `issues` 权限
3. 设置 7 天过期
4. 测试用这个 Token 创建一个 Issue

**提示**：使用 `curl` 或 Python 脚本测试。

### 练习2：配置 Git 使用 Token

**任务**：
1. 配置 Git 凭证管理器
2. 使用 Token 推送一次代码
3. 验证不再需要输入密码

### 练习3：让 AI 帮你管理仓库

**任务**：
1. 连接 WorkBuddy 到你的 GitHub
2. 让 AI 帮你创建一个 Issue
3. 让 AI 帮你查看最近的提交记录

---

## 📚 延伸阅读

- [第18章：让 AI 操控你的 GitHub](chapter18-ai-automation.md) - 更深入的 AI 自动化
- [第21章：常见安全问题](chapter21-security-issues.md) - Token 泄露的预防
- [第24章：私钥与密钥管理](chapter24-key-management.md) - 安全管理所有密钥

---

## 📝 本章小结

你现在学会了：

- ✅ 什么是 Token，为什么需要它
- ✅ 如何生成 Fine-grained Token（推荐）
- ✅ 如何使用 Token 访问 GitHub API
- ✅ 如何让 AI 助手帮你管理仓库
- ✅ 如何安全管理 Token

### ⚠️ 安全提醒

- 🔒 **永远不要把 Token 提交到公开仓库！**
- 🔒 **使用最小权限原则！**
- 🔒 **定期轮换 Token！**

---

<p align="right">
  <a href="chapter16-license-choice.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter18-ai-automation.md">下一章 →</a>
</p>