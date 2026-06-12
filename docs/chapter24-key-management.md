# 第24章：私钥与密钥管理 🔑

> Token、SSH Key、API Key…… 这些密钥怎么安全管理？本章详解。

## 📋 目录

- [什么是密钥？](#什么是密钥)
- [密钥的类型](#密钥的类型)
- [安全管理原则](#安全管理原则)
- [使用密码管理器](#使用密码管理器)
- [环境变量管理](#环境变量管理)
- [常见问题](#常见问题)

---

## 什么是密钥？

### 基本定义

**密钥（Secret）** 是用来"证明身份"的字符串。

### 比喻

```
现实世界：
├── 🔑 家门钥匙 → 证明你是房主
├── 🪪 门禁卡 → 证明你是员工
└── 🆔 车钥匙 → 证明车是你的

数字世界：
├── 🔑 GitHub Token → 证明你是账号主人
├── 🔑 SSH Private Key → 证明你的电脑是可信的
└── 🔑 API Key → 证明你有权调用 API
```

---

## 密钥的类型

### GitHub 相关密钥

| 密钥类型 | 格式 | 作用 |
|----------|------|------|
| **Personal Access Token** | `ghp_xxx...` | 访问 GitHub API |
| **SSH Private Key** | `id_ed25519` | SSH 连接 GitHub |
| **OAuth Token** | `gho_xxx...` | OAuth 授权 |

---

### 其他常见密钥

| 密钥 | 作用 | 泄露后果 |
|------|------|----------|
| **AWS Access Key** | 访问 AWS 云服务 | 服务器被控制、巨额账单 |
| **Stripe API Key** | 处理支付 | 资金被盗 |
| **数据库密码** | 访问数据库 | 数据被拖库 |

---

## 安全管理原则

### ❌ 千万不要做的事！

| 行为 | 后果 | 真实案例 |
|------|------|----------|
| **提交到公开仓库** | 全网都能看到 | 2021年有开发者把 AWS Key 提交到 GitHub，产生 $50,000 账单 |
| **写在代码里** | 代码泄露 = 密钥泄露 | 很多"开源"项目包含硬编码密钥 |
| **发给别人** | 对方可能泄露 | 微信/QQ 发送密钥 |
| **存在云笔记** | 云笔记可能被黑 | 印象笔记泄露事件 |

---

### ✅ 安全的做法！

#### 原则1：使用环境变量 ✅

```bash
# ❌ 错误做法（硬编码）
const API_KEY = "sk_test_abc123";

# ✅ 正确做法
const API_KEY = process.env.STRIPE_KEY;
```

---

#### 原则2：使用 .gitignore ✅

```gitignore
# .gitignore
.env
.env.local
secrets/
*.pem
*.key
```

---

#### 原则3：定期轮换密钥 ✅

```
建议周期：
├── Token → 每 30-90 天更换
├── SSH Key → 每 1-2 年更换
└── API Key → 每 90 天更换
```

---

#### 原则4：使用最小权限 ✅

```
Token 权限：
├── ❌ 不要勾选所有权限
└── ✅ 只给必需的权限

示例：
├── 只需要读取仓库 → 勾选 "Contents: Read-only"
└── 不需要删除仓库 → 不要勾选 "Delete repos"
```

---

## 使用密码管理器

### 为什么需要？

```
❌ 不用密码管理器的后果：
├── 所有密钥用同一个密码 → 一个泄露，全部完蛋
├── 密码写在纸上 → 纸条丢失或被盗
└── 密码存在浏览器 → 浏览器被黑，全部泄露

✅ 使用密码管理器：
├── 每个密钥独立随机密码
├── 加密存储
└── 只需要记住一个主密码
```

---

### 推荐工具

| 工具 | 类型 | 说明 |
|------|------|------|
| **1Password** | 商业 | 最易用、功能最强 ✅ |
| **Bitwarden** | 开源 | 免费、可自部署 ✅ |
| **KeePassXC** | 开源 | 本地存储、完全离线 |

---

### 使用 1Password 管理密钥

#### 第1步：保存密钥

```
1. 打开 1Password
2. 点击 "+" → 选择 "Password" 或 "API Credential"
3. 填写：
   ├── Title: GitHub Token (WorkBuddy)
   ├── Username: GTX950L
   ├── Password: ghp_xxxx...（粘贴 Token）
   └── Notes: 用于 WorkBuddy AI 助手
4. 保存
```

---

#### 第2步：在代码中使用

```bash
# 方法1：手动复制（推荐，最安全）
# 从 1Password 复制 Token → 粘贴到环境变量

# 方法2：使用 1Password CLI
op run --env-file=.env -- npm start
```

---

## 环境变量管理

### 什么是环境变量？

**环境变量** 是操作系统提供的"键值对"存储。

```bash
# 设置环境变量（Linux/macOS）
export GITHUB_TOKEN="ghp_xxxx..."

# 设置环境变量（Windows PowerShell）
$env:GITHUB_TOKEN = "ghp_xxxx..."

# 代码中读取
import os
token = os.environ.get("GITHUB_TOKEN")
```

---

### .env 文件管理 ✅

#### 创建 .env 文件

```bash
# .env（记得加入 .gitignore！）
GITHUB_TOKEN=ghp_xxxx...
STRIPE_KEY=sk_test_xxxx...
DB_PASSWORD=my_secret_password
```

#### 在代码中使用

```python
# Python：使用 python-dotenv
import os
from dotenv import load_dotenv

load_dotenv()  # 从 .env 加载

token = os.environ.get("GITHUB_TOKEN")
```

```javascript
// Node.js：使用 dotenv
require('dotenv').config();

const token = process.env.GITHUB_TOKEN;
```

---

### 不同环境使用不同密钥 ✅

```
开发环境：
└── .env.local（本地开发用，不要提交）

生产环境：
├── 使用服务器环境变量
└── 或使用密钥管理服务（如 AWS Secrets Manager）
```

---

## 常见问题

### Q1: 密钥已经提交到公开仓库了，怎么办？

**A**: ⚠️ **立即撤销并重新生成！**

#### 步骤：

```bash
# 第1步：撤销 Token/密钥
# 进入 GitHub Settings → Developer settings → Personal access tokens
# 删除泄露的 Token

# 第2步：从 Git 历史中彻底删除
# 使用 BFG Repo-Cleaner
java -jar bfg.jar --replace-text secrets.txt 你的仓库.git

# 第3步：强制推送（会重写历史！）
git push origin --force --all
```

> ⚠️ **注意**：重写 Git 历史会影响所有协作者。

---

### Q2: 可以把 .env 文件提交到私有仓库吗？

**A**: ❌ **不推荐！**

即使仓库是私有的：
- 协作者可以看到
- 如果仓库以后改为公开，历史中会有 .env

**正确做法**：
```
提交 .env.example（示例文件，不含真实密钥）
│
内容：
GITHUB_TOKEN=your_token_here
DB_PASSWORD=your_password_here

→ 让协作者复制为 .env 并填入自己的密钥
```

---

### Q3: 如何安全地分享密钥给队友？

**A**: ❌ **不要通过微信/QQ/邮件发送！**

#### 安全的方式：

| 方式 | 说明 |
|------|------|
| **密码管理器共享** | 1Password、Bitwarden 支持安全共享 |
| **临时加密链接** | 使用 [Onetimese](https://onetimese.com/) 生成一次性链接 |
| **公司内部系统** | 使用企业密码管理工具 |

---

## 📝 实战练习

### 练习1：审计你的密钥

**任务**：
1. 搜索你的所有公开仓库
2. 检查是否有硬编码的密钥
3. 如果有，立即撤销并重新生成

---

### 练习2：配置 .gitignore 和 .env

**任务**：
1. 在项目根目录创建 `.gitignore`
2. 添加 `.env` 到忽略列表
3. 创建 `.env.example` 作为模板
4. 提交到仓库

---

### 练习3：使用密码管理器

**任务**：
1. 注册一个密码管理器（推荐 Bitwarden 免费版）
2. 把你的 GitHub Token 保存进去
3. 从密码管理器中复制 Token，测试能否正常使用

---

## 📚 延伸阅读

- [第17章：GitHub Token 完全指南](chapter17-token-guide.md) - Token 的生成和使用
- [第21章：常见安全问题](chapter21-security-issues.md) - 更多安全陷阱
- [第22章：SSH Key 配置](chapter22-ssh-key-setup.md) - SSH Key 的安全管理

---

## 📝 本章小结

你现在了解了：

- ✅ 什么是密钥，有哪些类型
- ✅ 安全管理密钥的 4 个原则
- ✅ 如何使用密码管理器
- ✅ 如何使用环境变量
- ✅ 密钥泄露后的应对措施

### ⚠️ 安全提醒

> **密钥就是你的数字身份证。**
> 
> 泄露一个 Token，可能让攻击者：
> - 删除你的所有仓库
> - 发布恶意代码
> - 冒充你进行诈骗

---

<p align="right">
  <a href="chapter23-2fa-setup.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter25-git-advanced.md">下一章 →</a>
</p>