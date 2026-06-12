# 第21章：常见安全问题 🔒

> 网络安全很重要！本章介绍 GitHub 使用中的常见安全陷阱和防范措施。

## 📋 目录

- [安全事故案例](#安全事故案例)
- [陷阱1：泄露敏感信息](#陷阱1泄露敏感信息)
- [陷阱2：钓鱼攻击](#陷阱2钓鱼攻击)
- [陷阱3：恶意 Pull Request](#陷阱3恶意-pull-request)
- [陷阱4：依赖投毒](#陷阱4依赖投毒)
- [安全最佳实践](#安全最佳实践)
- [遇到问题怎么办？](#遇到问题怎么办)
- [常见问题](#常见问题)

---

## 安全事故案例

### 案例1：Token 泄露导致服务器被入侵

**时间**：2021年  
**事件**：某开发者把 GitHub Token 提交到了公开仓库  
**后果**：
- ❌ 攻击者用 Token 访问了他的所有私有仓库
- ❌ 利用仓库中的云服务器密钥，创建了挖矿容器
- ❌ 产生了 **$50,000** 的云服务费用

**教训**：**永远不要把 Token、密钥提交到公开仓库！**

---

### 案例2：npm 包被植入恶意代码

**时间**：2022年  
**事件**：流行的 npm 包 `node-ipc` 被作者植入恶意代码  
**后果**：
- ❌ 所有使用该包的项目都受到了影响
- ❌ 恶意代码删除了用户的文件

**教训**：**不要盲目信任依赖，定期检查依赖的安全性！**

---

### 案例3：钓鱼 Issue 窃取账号

**时间**：2023年  
**事件**：攻击者伪造 GitHub 官方邮件，要求用户"验证账号"  
**后果**：
- ❌ 用户点击了钓鱼链接，输入了 GitHub 账号密码
- ❌ 攻击者登录账号，转走了所有私有仓库

**教训**：**GitHub 不会通过邮件要求你输入密码！**

---

## 陷阱1：泄露敏感信息

### ⚠️ 哪些信息不能提交？

| 敏感信息类型 | 示例 | 后果 |
|--------------|------|------|
| **API Token** | `ghp_xxxxxxxx`, `sk_test_xxxxx` | 账号被接管 |
| **云服务器密钥** | AWS Access Key, 阿里云 Secret | 服务器被入侵 |
| **数据库密码** | `DB_PASSWORD=123456` | 数据库被拖库 |
| **私钥文件** | `id_rsa`, `*.pem` | 服务器被控制 |
| **个人信息** | 身份证号、手机号 | 隐私泄露 |

---

### 🚫 如何防止提交敏感信息？

#### 方法1：使用 .gitignore

```gitignore
# .gitignore 示例

# 环境变量文件（可能包含密钥！）
.env
.env.local
.env.production

# 私钥文件
*.pem
*.key
id_rsa
id_ed25519

# 配置文件（可能包含密码）
config.json
secrets.yml

# 日志文件（可能包含敏感请求）
*.log
```

---

#### 方法2：使用环境变量

**❌ 错误做法**：

```python
# config.py
API_TOKEN = "ghp_xxxxxxxxxxxxx"  # 不要硬编码！
```

**✅ 正确做法**：

```python
# config.py
import os
API_TOKEN = os.environ.get("GITHUB_TOKEN")  # 从环境变量读取
```

```bash
# 在本地设置环境变量
export GITHUB_TOKEN="ghp_xxxxxxxxxxxxx"

# 或者在 .env 文件中设置（记得加入 .gitignore！）
echo "GITHUB_TOKEN=ghp_xxxxxxxxxxxxx" > .env
```

---

#### 方法3：使用 Pre-commit Hook 检查

**安装 pre-commit**：

```bash
pip install pre-commit
```

**配置 .pre-commit-config.yaml**：

```yaml
repos:
  - repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
      - id: detect-secrets
        args: ['--baseline', '.secrets.baseline']
```

**安装 hook**：

```bash
pre-commit install
```

现在，每次提交前都会自动检查是否包含敏感信息！

---

### 🔍 如果已经泄露了怎么办？

#### 步骤1：立即撤销 Token/密钥

```
1. 登录对应平台（GitHub、AWS 等）
2. 找到泄露的 Token/密钥
3. 立即删除或重新生成
```

#### 步骤2：从 Git 历史中彻底删除

**使用 BFG Repo-Cleaner**（推荐）：

```bash
# 下载 BFG
wget https://repo1.maven.org/maven2/com/madgag/bfg/1.14.0/bfg-1.14.0.jar

# 删除所有包含 'secret' 的文件
java -jar bfg.jar --replace-text secrets.txt 你的仓库.git

# 清理 Git 历史
git reflog expire --expire=now --all
git gc --prune=now --aggressive

# 强制推送（会重写历史！）
git push origin --force --all
```

> ⚠️ **警告**：重写 Git 历史会影响所有协作者，务必提前通知团队！

---

## 陷阱2：钓鱼攻击

### 🎣 常见的 GitHub 钓鱼手法

#### 手法1：伪造 GitHub 邮件

**钓鱼邮件示例**：

```
发件人：noreply@github.com  ❌（真实是 noreply@github.com）
主题：⚠️ 您的 GitHub 账号出现异常活动

请立即点击以下链接验证您的身份：
https://github.com.verify-security.xyz/  ❌（假链接！）

如果您不验证，您的账号将被暂停。
```

**如何识别**：
- ✅ 真链接一定是 `https://github.com/...`
- ✅ GitHub 不会通过邮件要求你输入密码
- ✅ 检查发件人邮箱是否为官方域名

---

#### 手法2：恶意 Issue 或 PR

**示例**：

```
Issue 标题：🐛 Critical security vulnerability found

内容：
Hi, I found a critical vulnerability in your project.
Please check this link to see the details:
https://malicious-site.xyz/github-exploit  ❌（恶意链接）

You must apply this patch immediately:
（附件：malicious-patch.zip） ❌（包含恶意代码）
```

**如何防范**：
- ✅ 不要点击来历不明的链接
- ✅ 不要下载来历不明的附件
- ✅ 先在网上搜索这个"漏洞"是否真实存在

---

### 🔒 如何防范钓鱼攻击？

| 措施 | 说明 |
|------|------|
| **开启两步验证（2FA）** | 即使密码被盗，攻击者也无法登录 |
| **检查 URL** | 确保链接是 `https://github.com/...` |
| **不要轻信** | GitHub 官方不会主动联系你 |
| **使用密码管理器** | 避免访问钓鱼网站 |

---

## 陷阱3：恶意 Pull Request

### ⚠️ 风险说明

当你收到一个 PR 时，里面可能包含：
- ❌ 恶意代码（窃取数据、植入后门）
- ❌ 依赖投毒（引入有漏洞的依赖）
- ❌ 隐晦的 Bug（故意引入安全漏洞）

---

### 🔍 如何安全地审查 PR？

#### 步骤1：不要直接合并

```
收到 PR → 先审查代码 → 测试 → 再合并
```

#### 步骤2：在隔离环境测试

```bash
# 创建临时环境
docker run -it --rm node:latest bash

# 在容器内克隆 PR 的分支
git clone -b pr-branch https://github.com/用户名/仓库.git
cd 仓库

# 运行代码，观察行为
npm install
npm start
```

#### 步骤3：使用 GitHub 的 PR 审查功能

```
PR 页面 → Files changed → 逐行审查
│
├─ 点击 `+` 号添加评论
├─ 提出修改意见
└─ 确认无误后，再点击 Merge
```

---

## 陷阱4：依赖投毒

### ⚠️ 什么是依赖投毒？

攻击者上传一个和流行包**名字相似**的恶意包。

**示例**：

```
正版包：requests（Python 热门 HTTP 库）
恶意包：requestss（多了一个 s！）

如果你手滑打错了，就会安装恶意包！
```

---

### 🔒 如何防范？

#### 方法1：使用锁文件

**Python**：

```bash
# 使用 poetry 或 pipenv，它们会生成锁文件
poetry add requests  # 自动更新 poetry.lock
```

**Node.js**：

```bash
# package-lock.json 会锁定依赖版本
npm install
git add package-lock.json
```

---

#### 方法2：检查依赖的安全性

**使用 GitHub Dependabot**（自动）：

```
仓库页面 → Settings → Security & analysis → Enable Dependabot alerts
```

GitHub 会自动扫描你的依赖，并提醒你是否有漏洞。

---

#### 方法3：手动审计依赖

```bash
# Python：使用 safety 检查漏洞
pip install safety
safety check

# Node.js：使用 npm audit
npm audit

# 查看某个包的安装量（判断是否为恶意包）
npm view 包名
```

---

## 安全最佳实践

### ✅ 账号安全

| 做法 | 说明 |
|------|------|
| **开启两步验证（2FA）** | 最重要！见 [第23章：两步验证](chapter23-2fa-setup.md) |
| **使用强密码** | 至少 12 位，包含大小写、数字、符号 |
| **不要在公共电脑登录** | 如果必须，记得退出并撤销 Session |
| **定期检查 Session** | Settings → Sessions → 撤销不明设备 |

---

### ✅ 代码安全

| 做法 | 说明 |
|------|------|
| **不要提交敏感信息** | 使用 .gitignore 和环境变量 |
| **定期更新依赖** | 修复已知漏洞 |
| **使用最小权限原则** | Token 只给必需权限，不要给 `repo` 所有权限 |
| **审查每一个 PR** | 不要盲目合并 |

---

### ✅ 仓库安全

| 做法 | 说明 |
|------|------|
| **私有仓库不要随意公开** | 确保里面没有敏感信息 |
| **开启分支保护** | 防止直接推送到 main 分支 |
| **启用 GitHub Security** | 自动扫描漏洞和密钥 |
| **限制协作者权限** | 只给必需的权限 |

---

## 遇到问题怎么办？

### 如果你的账号被盗了

**步骤1：立即重置密码**

```
访问 https://github.com/password_reset
输入你的邮箱，重置密码
```

**步骤2：撤销所有 Token 和 SSH Key**

```
Settings → Developer settings → Personal access tokens → 删除所有
Settings → SSH and GPG keys → 删除所有不明密钥
```

**步骤3：检查是否有恶意提交**

```
查看你的仓库的提交记录
如果发现有不明的提交，立即回滚
```

**步骤4：联系 GitHub 支持**

```
访问 https://support.github.com/
报告账号被盗，请求协助调查
```

---

### 如果发现漏洞被利用

**步骤1：立即修复**

```
1. 修复漏洞代码
2. 提交补丁
3. 发布新版本
```

**步骤2：披露漏洞**

```
1. 在 SECURITY.md 中说明漏洞详情
2. 提醒用户升级
3. 感谢发现漏洞的人（如果有的话）
```

**步骤3：复盘**

```
1. 分析漏洞是如何引入的
2. 改进开发流程，防止类似问题
```

---

## 常见问题

### Q1: 我的 Token 泄露了，但我已经删除了，还需要做什么？

**A**: 
1. ✅ 检查 Git 历史，确保没有把 Token 提交到仓库
2. ✅ 检查是否有不明提交或仓库
3. ✅ 开启两步验证，防止再次被盗

---

### Q2: 我可以信任所有 GitHub 上的开源代码吗？

**A**: 
- ❌ **不可以！**
- ✅ 只使用知名、维护活跃的项目
- ✅ 定期审计你的依赖
- ✅ 不要使用来历不明的代码片段

---

### Q3: 如何判断一个 Issue 或 PR 是否是钓鱼攻击？

**A**: 警惕以下特征：
- 🚩 发帖者账号很新，没有贡献记录
- 🚩 内容中包含外部链接，要求你点击
- 🚩 语气紧急，要求你"立即"做某事
- 🚩 附件或补丁来自不明来源

---

## 📝 实战练习

### 练习1：审计你的仓库

**任务**：
1. 检查你的所有公开仓库
2. 确保没有提交 .env、密钥文件
3. 检查 .gitignore 是否配置正确

---

### 练习2：配置安全工具

**任务**：
1. 安装 pre-commit
2. 配置 detect-secrets hook
3. 测试提交一个包含密钥的文件，看是否被拦截

---

### 练习3：启用 GitHub 安全功能

**任务**：
1. 进入仓库 Settings → Security & analysis
2. 开启 Dependabot alerts
3. 开启 Code scanning alerts
4. 查看是否有漏洞提醒

---

## 📚 延伸阅读

- [第22章：SSH Key 配置](chapter22-ssh-key-setup.md) - 更安全的身份验证
- [第23章：两步验证（2FA）](chapter23-2fa-setup.md) - 保护账号安全
- [第24章：私钥与密钥管理](chapter24-key-management.md) - 安全管理所有密钥

---

## 📝 本章小结

你现在了解了：

- ✅ 常见的安全陷阱（泄露敏感信息、钓鱼攻击、恶意 PR、依赖投毒）
- ✅ 如何防范这些安全问题
- ✅ 遇到安全问题时的应对措施
- ✅ GitHub 安全最佳实践

### ⚠️ 安全提醒

> **安全不是一次性的工作，而是持续的过程。**
> 
> 定期审计你的代码、更新依赖、检查账号活动，才能确保你的项目和账号安全。

---

<p align="right">
  <a href="chapter20-pages-website.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter22-ssh-key-setup.md">下一章 →</a>
</p>