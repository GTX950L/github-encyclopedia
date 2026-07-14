# 第27章：GitHub API 详解 🔌

> 想开发 GitHub 工具？想自动化操作？GitHub REST API 是你的好帮手！

## 📋 目录

- [什么是 GitHub API？](#什么是-github-api)
- [认证方式](#认证方式)
- [常用 API 端点](#常用-api-端点)
- [分页和限流](#分页和限流)
- [示例：开发一个 CLI 工具](#示例开发一个-cli-工具)
- [常见问题](#常见问题)

---

## 什么是 GitHub API？

### 基本定义

**GitHub REST API** 是 GitHub 提供的 **HTTP 接口**，让你可以编程式地操作 GitHub。

### 可以做什么？

| 操作 | API 端点 |
|------|------------|
| **获取用户信息** | `GET /users/用户名` |
| **列出所有仓库** | `GET /user/repos` |
| **创建 Issue** | `POST /repos/用户名/仓库名/issues` |
| **创建 PR** | `POST /repos/用户名/仓库名/pulls` |
| **合并 PR** | `PUT /repos/用户名/仓库名/pulls/123/merge` |

---

## 认证方式

### 方式1：使用 Token（推荐）✅

```bash
# 在请求头中加上 Token
curl -H "Authorization: Bearer ghp_你的token" \
  https://api.github.com/user
```

---

### 方式2：使用 OAuth App

适合需要让用户授权你的应用的场景。

**流程**：

```
1. 用户点击"使用 GitHub 登录"
2. 跳转到 GitHub 授权页面
3. 用户点击 Authorize
4. GitHub 回调你的网站，带上 code
5. 你的后端用 code 换取 access_token
6. 使用 access_token 调用 API
```

---

## 常用 API 端点

### 1️⃣ 用户相关

#### 获取当前用户信息

```bash
curl -H "Authorization: Bearer ghp_xxx" \
  https://api.github.com/user
```

**响应示例**：

```json
{
  "login": "GTX950L",
  "id": 123456,
  "name": "GTX950L",
  "public_repos": 10,
  "followers": 45,
  "following": 67
}
```

---

#### 获取某个用户的信息

```bash
curl https://api.github.com/users/GTX950L
```

---

### 2️⃣ 仓库相关

#### 列出当前用户的所有仓库

```bash
curl -H "Authorization: Bearer ghp_xxx" \
  https://api.github.com/user/repos
```

---

#### 获取某个仓库的信息

```bash
curl https://api.github.com/repos/GTX950L/github-encyclopedia
```

---

#### 创建新仓库

```bash
curl -X POST \
  -H "Authorization: Bearer ghp_xxx" \
  -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/user/repos \
  -d '{"name":"my-new-repo","description":"我的新仓库","public":true}'
```

---

### 3️⃣ Issue 相关

#### 列出仓库的所有 Issue

```bash
curl https://api.github.com/repos/GTX950L/github-encyclopedia/issues
```

---

#### 创建一个 Issue

```bash
curl -X POST \
  -H "Authorization: Bearer ghp_xxx" \
  https://api.github.com/repos/GTX950L/github-encyclopedia/issues \
  -d '{"title":"Bug: 登录失败","body":"详细描述..."}'
```

---

### 4️⃣ PR 相关

#### 列出仓库的所有 PR

```bash
curl https://api.github.com/repos/GTX950L/github-encyclopedia/pulls
```

---

#### 创建一个 PR

```bash
curl -X POST \
  -H "Authorization: Bearer ghp_xxx" \
  https://api.github.com/repos/GTX950L/github-encyclopedia/pulls \
  -d '{"title":"添加新功能","head":"feature/login","base":"main","body":"描述..."}'
```

---

## 分页和限流

### 分页（Pagination）

**为什么需要？**

GitHub API 默认**每页只返回 30 条记录**。

#### 如何获取所有记录？

```bash
# 第1页（默认）
curl https://api.github.com/user/repos?page=1&per_page=100

# 第2页
curl https://api.github.com/user/repos?page=2&per_page=100
```

#### 自动处理分页（推荐）✅

```python
import requests

token = "ghp_你的token"
headers = {"Authorization": f"Bearer {token}"}

all_repos = []
page = 1

while True:
    response = requests.get(
        "https://api.github.com/user/repos",
        headers=headers,
        params={"page": page, "per_page": 100}
    )
    
    repos = response.json()
    if not repos:  # 没有更多记录了
        break
    
    all_repos.extend(repos)
    page += 1

print(f"总共有 {len(all_repos)} 个仓库")
```

---

### 限流（Rate Limiting）

**为什么需要？**

防止滥用，GitHub 对 API 调用次数有限制。

#### 限额

| 认证方式 | 每小时限额 |
|------------|---------------|
| **未认证** | 60 次 |
| **Token 认证** | 5000 次 |

#### 查看剩余限额

```bash
curl -I https://api.github.com/user
```

**响应头**：

```
X-RateLimit-Limit: 5000       # 每小时限额
X-RateLimit-Remaining: 4999  # 剩余次数
X-RateLimit-Reset: 1623456789   # 重置时间（Unix 时间戳）
```

#### 超限了怎么办？

```python
import time
import requests

# 如果收到 403 错误，且响应头显示超限：
if response.status_code == 403:
    reset_time = int(response.headers["X-RateLimit-Reset"])
    wait_seconds = reset_time - time.time()
    print(f"超限了！等待 {wait_seconds} 秒...")
    time.sleep(wait_seconds + 1)
    # 然后重试
```

---

## 示例：开发一个 CLI 工具

### 目标：开发一个 `gh-stats` 工具

**功能**：生成你的 GitHub 统计报告。

#### 代码实现

```python
#!/usr/bin/env python3
"""
gh-stats: 生成 GitHub 统计报告
"""

import os
import requests
from datetime import datetime

# 从环境变量读取 Token
token = os.environ.get("GITHUB_TOKEN")
if not token:
    print("❌ 请设置 GITHUB_TOKEN 环境变量")
    exit(1)

headers = {
    "Authorization": f"Bearer {token}",
    "Accept": "application/vnd.github.v3+json"
}

# 获取当前用户信息
response = requests.get("https://api.github.com/user", headers=headers)
user = response.json()
username = user["login"]

print(f"📊 生成 {username} 的统计报告...")
print("=" * 50)

# 获取所有仓库
all_repos = []
page = 1
while True:
    response = requests.get(
        "https://api.github.com/user/repos",
        headers=headers,
        params={"page": page, "per_page": 100}
    )
    repos = response.json()
    if not repos:
        break
    all_repos.extend(repos)
    page += 1

# 统计
total_stars = sum(repo["stargazers_count"] for repo in all_repos)
total_forks = sum(repo["forks_count"] for repo in all_repos)
public_repos = [r for r in all_repos if not r["private"]]
private_repos = [r for r in all_repos if r["private"]]

print(f"📁 公开仓库: {len(public_repos)}")
print(f"🔒 私有仓库: {len(private_repos)}")
print(f"⭐ 总 Star 数: {total_stars}")
print(f"🍴 总 Fork 数: {total_forks}")

# 找出 Star 最多的仓库
if public_repos:
    top_repo = max(public_repos, key=lambda r: r["stargazers_count"])
    print(f"\n🏆 最受欢迎的仓库:")
    print(f"   {top_repo['name']} ({top_repo['stargazers_count']} ⭐)")

print("=" * 50)
print("✅ 报告生成完成！")
```

---

#### 使用效果

```bash
# 设置 Token
export GITHUB_TOKEN="ghp_你的token"

# 运行工具
python3 gh-stats.py
```

**输出**：

```
📊 生成 GTX950L 的统计报告...
==================================================

📁 公开仓库: 5
🔒 私有仓库: 3
⭐ 总 Star 数: 123
🍴 总 Fork 数: 45

🏆 最受欢迎的仓库:
   github-encyclopedia (100 ⭐)

==================================================
✅ 报告生成完成！
```

---

## 🔷 GraphQL API（新版 API）

### 什么是 GraphQL？

除了 REST API，GitHub 还提供了 **GraphQL API**（v4），它可以让你**精确获取想要的数据**，不多不少。

### REST vs GraphQL

| 对比 | REST API | GraphQL API |
|------|----------|-------------|
| **版本** | v3 | v4 |
| **数据量** | 返回固定数据（可能过多或不足） | ✅ 精确指定要什么字段 |
| **请求次数** | 可能需要多次请求 | ✅ 一次请求获取所有数据 |
| **学习曲线** | ⭐ 简单 | ⭐⭐ 中等 |
| **推荐场景** | 简单操作、脚本 | 复杂查询、工具开发 |

### 示例对比

**场景**：获取你的仓库名和 Star 数

#### REST 方式（先获取用户，再获取每个仓库）

```bash
# 第1步：获取所有仓库（返回很多不需要的字段）
curl -H "Authorization: Bearer ghp_xxx" \
  https://api.github.com/user/repos

# 第2步：从返回的 JSON 中提取仓库名和 Star 数
# 返回了一堆你可能不需要的字段
```

#### GraphQL 方式（精确指定你要的字段）✅

```bash
# 一次请求，精确查询
curl -H "Authorization: Bearer ghp_xxx" \
  -H "Content-Type: application/json" \
  -X POST https://api.github.com/graphql \
  -d '{
    "query": "{
      viewer {
        repositories(first: 10) {
          nodes {
            name
            stargazerCount
          }
        }
      }
    }"
  }'
```

**响应**（只有你要的字段）：

```json
{
  "data": {
    "viewer": {
      "repositories": {
        "nodes": [
          {"name": "github-encyclopedia", "stargazerCount": 100},
          {"name": "my-tool", "stargazerCount": 42}
        ]
      }
    }
  }
}
```

### 在 Python 中使用 GraphQL

```python
import requests

token = "ghp_你的token"
headers = {
    "Authorization": f"Bearer {token}",
    "Content-Type": "application/json"
}

# 查询：获取你的仓库名、Star 数、主要语言
query = """
{
  viewer {
    repositories(first: 50, orderBy: {field: STARGAZERS, direction: DESC}) {
      nodes {
        name
        stargazerCount
        primaryLanguage {
          name
        }
        description
      }
    }
  }
}
"""

response = requests.post(
    "https://api.github.com/graphql",
    headers=headers,
    json={"query": query}
)

data = response.json()
for repo in data["data"]["viewer"]["repositories"]["nodes"]:
    lang = repo["primaryLanguage"]["name"] if repo["primaryLanguage"] else "N/A"
    print(f"⭐ {repo['name']} ({repo['stargazerCount']} stars, {lang})")
```

### 什么时候用 REST，什么时候用 GraphQL？

| 场景 | 推荐 |
|------|------|
| **简单操作**（创建 Issue、合并 PR） | REST ✅ |
| **复杂查询**（获取仓库和所有关联数据） | GraphQL ✅ |
| **写脚本** | REST（更简单） |
| **开发工具** | GraphQL（更高效） |
| **需要兼容旧代码** | REST |
| **新项目新功能** | GraphQL |

> 💡 **GitHub 官方推荐**：尽量使用 GraphQL API，但 REST API 仍然完全支持，不会弃用。

---

## 常见问题

### Q1: API 返回的时区是什么？

**A**: GitHub API 返回的时间是 **UTC 时区**。

**如何转换？**

```python
from datetime import datetime
import pytz

# GitHub API 返回的时间
utc_time_str = "2026-06-12T10:00:00Z"

# 转换为本地时区（如北京时间 UTC+8）
utc_time = datetime.strptime(utc_time_str, "%Y-%m-%dT%H:%M:%SZ")
local_time = utc_time.replace(tzinfo=pytz.utc).astimezone(pytz.timezone("Asia/Shanghai"))

print(local_time)  # 输出：2026-06-12 18:00:00+08:00
```

---

### Q2: 如何上传文件到仓库？

**A**: 使用 **Create or update file contents** 端点。

```python
import base64

# 文件内容需要 Base64 编码
content = "Hello, World!"
encoded_content = base64.b64encode(content.encode()).decode()

response = requests.put(
    "https://api.github.com/repos/用户名/仓库名/contents/README.md",
    headers=headers,
    json={
        "message": "Update README",
        "content": encoded_content,
        "sha": "现有文件的 SHA（如果是更新）"
    }
)
```

---

### Q3: 如何删除仓库中的文件？

**A**: 使用 **Delete file** 端点。

```python
# 先获取文件的 SHA
response = requests.get(
    "https://api.github.com/repos/用户名/仓库名/contents/文件名",
    headers=headers
)
file_sha = response.json()["sha"]

# 删除文件
requests.delete(
    "https://api.github.com/repos/用户名/仓库名/contents/文件名",
    headers=headers,
    json={
        "message": "Delete file",
        "sha": file_sha
    }
)
```

---

## 📝 实战练习

### 练习1：调用 API 获取你的信息

**任务**：
1. 生成 Personal Access Token
2. 使用 `curl` 调用 `GET /user` 端点
3. 查看返回的 JSON 数据

---

### 练习2：开发一个"自动 Star"工具

**任务**：
1. 写一个 Python 脚本
2. 从文件读取仓库列表（每行一个）
3. 调用 API 给这些仓库点 Star
4. 运行工具，确认 Star 成功

---

### 练习3：开发一个 Issue 统计工具

**任务**：
1. 调用 API 获取某个仓库的所有 Issue
2. 统计：打开的、关闭的、带标签的
3. 生成统计报告（可以输出为 Markdown 表格）

---

## 📚 延伸阅读

- [第17章：GitHub Token 完全指南](chapter17-token-guide.md) - Token 的生成和使用
- [第26章：GitHub CLI 使用](chapter26-gh-cli.md) - 官方 CLI 工具
- [GitHub API 官方文档](https://docs.github.com/en/rest)

---

## 📝 本章小结

你现在学会了：

- ✅ 什么是 GitHub API，为什么需要它
- ✅ 如何认证（Token、OAuth）
- ✅ 常用 API 端点（用户、仓库、Issue、PR）
- ✅ 如何处理分页和限流
- ✅ 如何开发一个 CLI 工具

### 💡 核心要点

> **GitHub API 是自动化和工具开发的基石。**
> 
> 熟练掌握 API 后，你可以：
> - 开发自己的 GitHub 工具
> - 自动化重复性任务
> - 集成 GitHub 到其他系统

---

<p align="right">
  <a href="chapter26-gh-cli.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter28-best-practices.md">下一章 →</a>
</p>
