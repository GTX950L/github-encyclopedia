# 🚀 快速入门指南

> 5分钟了解 GitHub，开始你的开源之旅！

---

## 第一步：注册 GitHub 账号

1. 打开 [github.com](https://github.com)
2. 点击 **Sign up** → 输入邮箱 → 设置用户名和密码
3. 验证邮箱 → 完成注册 🎉

> 💡 **用户名建议**：使用英文 + 数字，避免特殊字符。这个名字会出现在你的所有开源项目地址中。

---

## 第二步：了解核心概念（只需知道这3个）

| 概念 | 一句话解释 | 类比 |
|------|---------|------|
| **仓库（Repository）** | 存放代码的地方 | 快递箱 |
| **提交（Commit）** | 保存一次修改 | Ctrl+S |
| **推送（Push）** | 把本地修改发到 GitHub | 发送快递 |

---

## 第三步：创建你的第一个仓库

1. 登录 GitHub → 点击右上角 **+** → **New repository**
2. 填写仓库名（如 `hello-world`）
3. 勾选 **Add a README file**
4. 点击 **Create repository** ✅

> 🎉 恭喜！你已经创建了第一个 GitHub 仓库。

---

## 第四步：上传代码（3种方式）

### 方式1：网页上传（最简单）

1. 进入仓库 → 点击 **Add file** → **Upload files**
2. 拖拽文件到网页 → 填写提交信息 → 提交

### 方式2：GitHub Desktop（适合 Windows/Mac）

1. 下载 [GitHub Desktop](https://desktop.github.com/)
2. 登录 → Clone 仓库 → 修改文件 → 提交 → Push

### 方式3：命令行（最专业）

```bash
# 克隆仓库
git clone https://github.com/你的用户名/仓库名.git

# 修改后提交
git add .
git commit -m "我的第一次提交"
git push
```

---

## 第五步：发现好项目

1. 在 GitHub 搜索栏输入感兴趣的主题（如 `python beginner`）
2. 点 **Star** 收藏喜欢的项目 ⭐
3. 点 **Fork** 把别人的项目复制到你的账号下 🍴

---

## 🗺️ 接下来学什么？

| 你的目标 | 推荐阅读 |
|---------|---------|
| 了解 GitHub 全貌 | [第1章：什么是 GitHub？](chapter01-what-is-github.md) |
| 学会下载代码 | [第5章：下载代码](chapter05-download-code.md) |
| 学会贡献开源项目 | [第9章：Fork 与 Pull Request](chapter09-fork-pr.md) |
| 想写好项目介绍 | [第15章：README 编写艺术](chapter15-readme-art.md) |
| 完整系统学习 | [按顺序阅读目录](../README.md#-内容结构) |

---

## ❓ 常见问题

**Q: 我需要学习 Git 命令行吗？**
A: 不需要！网页版和 GitHub Desktop 足够满足日常使用。想进阶再看 [第25章：Git 高级命令](chapter25-git-advanced.md)。

**Q: 我的代码是隐私的，可以不让别人看到吗？**
A: 可以！创建仓库时选择 **Private**。免费账号也可以创建私有仓库。

**Q: Token 是什么？我需要吗？**
A: 用网页版操作不需要。但如果用 AI 助手或脚本操控 GitHub，就需要 Token。详见 [第17章：Token 完全指南](chapter17-token-guide.md)。
