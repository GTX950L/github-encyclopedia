# 第22章：SSH Key 配置 🔑

> HTTPS 每次都要输密码（或 Token），太麻烦！SSH Key 可以让你"免密"操作。

## 📋 目录

- [为什么需要 SSH Key？](#为什么需要-ssh-key)
- [生成 SSH Key](#生成-ssh-key)
- [添加到 GitHub](#添加到-github)
- [测试连接](#测试连接)
- [常见问题](#常见问题)

---

## 为什么需要 SSH Key？

### HTTPS vs SSH 对比

| 方式 | 优点 | 缺点 |
|------|------|------|
| **HTTPS** | 简单，不需要配置 | ❌ 每次要输密码/Token |
| **SSH** | ✅ 一次配置，永久免密 | 需要生成和配置 Key |

### 使用场景

```
推荐用 SSH 的场景：
├── 你每天都在用 Git 推送代码
├── 你不想每次都输 Token
└── 你想更安全地连接 GitHub

推荐用 HTTPS 的场景：
├── 你只是偶尔下载代码
├── 你在公司内网（SSH 端口可能被封）
└── 你觉得配置 SSH 太麻烦
```

---

## 生成 SSH Key

### 步骤详解

#### 第1步：检查是否已有 SSH Key

```bash
# 查看是否有现成的 Key
ls -al ~/.ssh

# 如果看到以下文件，说明已经有了：
# id_rsa      （私钥）
# id_rsa.pub  （公钥）
# id_ed25519
# id_ed25519.pub
```

> 💡 **如果已经有了，跳过生成步骤！**

---

#### 第2步：生成新的 SSH Key

```bash
# 推荐使用 ed25519 算法（更安全、更快）
ssh-keygen -t ed25519 -C "你的邮箱@example.com"

# 如果是旧系统（不支持 ed25519），用 RSA：
ssh-keygen -t rsa -b 4096 -C "你的邮箱@example.com"
```

**交互式提问**：

```
Enter file in which to save the key
(/c/Users/你/.ssh/id_ed25519):
→ 直接按回车（使用默认路径）

Enter passphrase (empty for no passphrase):
→ 输入密码（推荐！增加安全性）
→ 不输入也可以（但不安全）

Enter same passphrase again:
→ 再次输入密码
```

> 💡 **是否设置密码？**
> - ✅ **推荐设置**：即使私钥泄露，没有密码也用不了
> - ❌ **不设置**：方便，但安全性低

---

#### 第3步：启动 SSH Agent

```bash
# 启动 ssh-agent
eval "$(ssh-agent -s)"

# 添加私钥到 ssh-agent
ssh-add ~/.ssh/id_ed25519

# 如果设置了密码，会要求输入一次
```

---

## 添加到 GitHub

### 步骤详解

#### 第1步：复制公钥

```bash
# 查看公钥内容
cat ~/.ssh/id_ed25519.pub

# 复制输出的内容（包括 ssh-ed25519 开头和邮箱结尾）
```

**示例输出**：

```
ssh-ed25519 AAAAC3NzaC1lE2EAAAAI... 你的邮箱@example.com
```

> ⚠️ **注意**：
> - 复制**公钥**（`.pub` 文件），不是私钥！
> - 私钥（`id_ed25519`，没有 `.pub` 后缀）要保密！

---

#### 第2步：在 GitHub 上添加

```
1. 登录 GitHub
2. 点击右上角头像 → Settings
3. 左侧菜单找到 SSH and GPG keys
4. 点击 New SSH key
5. 填写信息：
   ├── Title: 给你的 Key 起个名字（如：My Laptop）
   ├── Key type: Authentication Key
   └── Key: 粘贴刚才复制的公钥内容
6. 点击 Add SSH key
```

---

## 测试连接

### 测试命令

```bash
# 测试是否能连接 GitHub
ssh -T git@github.com

# 第一次会提示：
The authenticity of host 'github.com (140.82.121.3)' can't be established.
...
Are you sure you want to continue connecting (yes/no/[fingerprint])?

# 输入 yes 并回车
```

**成功的输出**：

```
Hi GTX950L! You've successfully authenticated, but GitHub does not provide shell access.
```

> 🎉 **成功！** 你现在可以用 SSH 克隆和推送了。

---

### 修改远程仓库地址

```bash
# 查看当前远程地址（大概率是 HTTPS）
git remote -v
# 输出：
# origin  https://github.com/用户名/仓库名.git (fetch)
# origin  https://github.com/用户名/仓库名.git (push)

# 改成 SSH 地址
git remote set-url origin git@github.com:用户名/仓库名.git

# 验证
git remote -v
# 输出：
# origin  git@github.com:用户名/仓库名.git (fetch)
# origin  git@github.com:用户名/仓库名.git (push)
```

**以后 `git push` 就不再需要输密码了！** 🎉

---

## 常见问题

### Q1: 私钥泄露了怎么办？

**A**: ⚠️ **立即删除 GitHub 上的公钥！**

```
步骤：
1. Settings → SSH and GPG keys
2. 找到对应的 Key
3. 点击 Delete
4. 生成新的 Key 对，重新添加
```

---

### Q2: 多个电脑如何配置？

**A**: 每台电脑都需要自己的 SSH Key。

```
电脑 A：
├── 生成 Key 对
├── 添加公钥到 GitHub
└── 标题写 "My Laptop"

电脑 B：
├── 生成 Key 对
├── 添加公钥到 GitHub
└── 标题写 "My Office PC"
```

> 💡 **不要** 把同一对 Key 复制多台电脑（不安全）。

---

### Q3: Windows 上如何生成 SSH Key？

**A**: 使用 **Git Bash**（安装 Git 时会自带）。

```
1. 右键桌面 → Git Bash Here
2. 运行 ssh-keygen 命令（和上面一样）
3. 私钥会保存在 C:\Users\你的用户名\.ssh\
```

---

## 📝 实战练习

### 练习1：配置你的第一对 SSH Key

**任务**：
1. 生成 ed25519 Key 对
2. 添加公钥到 GitHub
3. 测试连接
4. 修改一个仓库的远程地址为 SSH
5. 推送一次，确认不需要输密码

---

### 练习2：为多台电脑配置

**任务**：
1. 如果你有两台电脑（如公司和家里）
2. 分别为它们生成 Key 对
3. 都添加到 GitHub
4. 确认都能免密推送

---

## 📚 延伸阅读

- [第17章：GitHub Token 完全指南](chapter17-token-guide.md) - Token 的详细用法
- [第23章：两步验证（2FA）](chapter23-2fa-setup.md) - 保护账号安全
- [第24章：私钥与密钥管理](chapter24-key-management.md) - 安全管理所有密钥

---

## 📝 本章小结

你现在学会了：

- ✅ 为什么需要 SSH Key
- ✅ 如何生成 SSH Key 对
- ✅ 如何添加公钥到 GitHub
- ✅ 如何测试连接并修改仓库地址
- ✅ SSH Key 的安全管理

### ⚠️ 安全提醒

> **私钥 = 你的身份凭证，永远不要：**
> - ❌ 提交到 GitHub
> - ❌ 发送给别人
> - ❌ 放在公共电脑上

---

<p align="right">
  <a href="chapter21-security-issues.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter23-2fa-setup.md">下一章 →</a>
</p>
