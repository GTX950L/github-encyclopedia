# 第23章：两步验证（2FA）🔒

> 密码被盗了怎么办？开启两步验证，即使密码泄露也不怕！

## 📋 目录

- [什么是 2FA？](#什么是-2fa)
- [开启 2FA](#开启-2fa)
- [恢复代码](#恢复代码)
- [常见问题](#常见问题)

---

## 什么是 2FA？

### 基本定义

**2FA（Two-Factor Authentication，两步验证）** 是在密码之外，再加一层验证。

### 比喻

```
普通登录（只有密码）：
│
├── 输入用户名 → 输入密码 → ✅ 登录成功
│
└── 如果密码被盗 → ❌ 账号被盗！

开启 2FA 后：
│
├── 输入用户名 → 输入密码 → 还需要验证码 → ✅ 登录成功
│
└── 即使密码被盗，没有手机也登录不了！
```

---

### 两种验证方式对比

| 方式 | 安全性 | 便利性 | 推荐度 |
|------|----------|----------|----------|
| **Authenticator App** | ✅ 高 | ✅ 方便 | ⭐⭐⭐ 强烈推荐 |
| **SMS 短信** | ⚠️ 中（可被拦截） | ✅ 方便 | ⭐ 不推荐 |

---

## 开启 2FA

### 方法1：使用 Authenticator App（推荐）✅

#### 第1步：安装 App

| App | 平台 | 说明 |
|-----|------|------|
| **Google Authenticator** | iOS/Android | 最简单 |
| **Microsoft Authenticator** | iOS/Android | 功能更强 |
| **1Password** | iOS/Android | 密码管理器自带 |

---

#### 第2步：在 GitHub 上开启

```
1. 登录 GitHub → Settings → Password and authentication
2. 找到 Two-factor authentication → 点击 Enable two-factor authentication
3. 选择 Set up using an app
4. GitHub 会显示一个 QR 码
```

---

#### 第3步：扫描 QR 码

```
1. 打开 Authenticator App
2. 点击 "+" 号 → 扫描 QR 码
3. 扫描成功后，App 会显示一个 6 位验证码
4. 在 GitHub 页面输入这个验证码
5. 点击 Enable
```

🎉 **成功！** 以后登录时，需要输入 App 中的验证码。

---

### 方法2：使用 SMS（不推荐）

```
1. 在 2FA 设置页面，选择 Set up using SMS
2. 输入手机号
3. 接收验证码，输入
4. 完成
```

> ⚠️ **为什么不推荐？**
> - SMS 可以被拦截（SIM 卡克隆攻击）
> - 没有网络时收不到验证码

---

## 恢复代码

### 为什么需要？

```
场景：你的手机丢了 → 无法生成验证码 → ❌ 无法登录！
│
└── 如果有恢复代码 → ✅ 输入恢复代码 → 登录成功！
```

---

### 如何生成？

```
开启 2FA 时，GitHub 会自动生成恢复代码
│
├── 务必下载保存！
├── 建议打印出来，放在安全的地方
└── 每次使用一个，代码就会失效（防止被 reuse）
```

**保存建议**：

| 方式 | 安全性 | 说明 |
|------|----------|------|
| **打印纸质版** | ✅ 高 | 放在保险箱 |
| **保存在密码管理器** | ✅ 高 | 1Password、Bitwarden |
| **保存在电脑文件** | ⚠️ 中 | 如果电脑被盗就完了 |
| **截图保存** | ❌ 低 | 截图可能被同步到云端 |

---

### 如何使用恢复代码登录？

```
1. 登录时，在验证码输入框下方，点击 "Use a recovery code"
2. 输入一个恢复代码
3. 登录成功！
4. 立即重新配置 2FA（因为你用掉了一次恢复机会）
```

---

## 常见问题

### Q1: 换了手机，如何迁移 2FA？

**A**: 

**方法1：在旧手机上操作（推荐）**

```
旧手机上打开 Authenticator App
│
├── 导出账户（Export accounts）
├── 生成二维码
├── 新手机扫描二维码
└── 完成迁移！
```

**方法2：使用恢复代码**

```
1. 用恢复代码登录 GitHub
2. 关闭 2FA
3. 在新手机上重新开启 2FA
```

---

### Q2: 可以关闭 2FA 吗？

**A**: ✅ **可以，但不推荐！**

```
Settings → Password and authentication → Two-factor authentication → Disable
```

> ⚠️ **注意**：关闭后，账号安全性会大幅下降。

---

### Q3: 恢复代码泄露了怎么办？

**A**: ⚠️ **立即生成新的恢复代码！**

```
Settings → Password and authentication → Regenerate recovery codes
│
├── 旧代码全部失效
├── 下载新的恢复代码
└── 保存在安全的地方
```

---

## 📝 实战练习

### 练习1：开启 2FA

**任务**：
1. 安装 Google Authenticator
2. 在 GitHub 上开启 2FA
3. 保存恢复代码
4. 退出登录，然后重新登录，体验 2FA 流程

---

### 练习2：测试恢复代码

**任务**：
1. 故意不携带手机
2. 尝试登录 GitHub
3. 使用恢复代码登录
4. 确认登录成功

---

## 📚 延伸阅读

- [第21章：常见安全问题](chapter21-security-issues.md) - 更多安全陷阱
- [第24章：私钥与密钥管理](chapter24-key-management.md) - 安全管理所有密钥

---

## 📝 本章小结

你现在了解了：

- ✅ 什么是 2FA，为什么需要它
- ✅ 如何开启 2FA（Authenticator App）
- ✅ 如何生成和使用恢复代码
- ✅ 换手机时如何迁移

### ⚠️ 安全提醒

> **2FA 是保护 GitHub 账号的最重要措施！**
> 
> 在开启 2FA 之前，你的账号就像没锁门的房子。

---

<p align="right">
  <a href="chapter22-ssh-key-setup.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter24-key-management.md">下一章 →</a>
</p>
