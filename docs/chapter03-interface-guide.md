# 第3章：GitHub 界面全解析 🖥️

> 第一次打开 GitHub，是不是感觉按钮太多了？本章带你认识每一个按钮、每一个页面。

## 📋 目录

- [GitHub 主界面](#github-主界面)
- [仓库（Repository）页面](#仓库页面)
- [个人资料页](#个人资料页)
- [Issues 页面](#issues-页面)
- [Pull Requests 页面](#pull-requests-页面)
- [设置页面](#设置页面)
- [移动端界面](#移动端界面)

---

## GitHub 主界面

### 登录后的首页（Dashboard）

当你登录 GitHub 后，看到的首页包含以下部分：

#### 1️⃣ 顶部导航栏

```
┌─────────────────────────────────────────────────────┐
│ [Logo] [Search] [Pull requests] [Issues] [Codespaces] │
│                                    [@你的头像] [...] │
└─────────────────────────────────────────────────────┘
```

| 按钮/区域 | 作用 | 使用场景 |
|-----------|------|----------|
| 🐙 **GitHub Logo** | 返回首页 | 随时点击返回 |
| 🔍 **Search 搜索框** | 搜索仓库、用户、代码 | 找开源项目、找人 |
| **Pull requests** | 查看你参与的所有 PR | 查看别人对你的代码的审查 |
| **Issues** | 查看你参与的所有问题 | 跟踪 Bug 报告、功能请求 |
| **Codespaces** | 云端开发环境 | 在线编写代码（高级功能） |
| **头像图标** | 个人菜单 | 访问设置、退出登录等 |

#### 2️⃣ 左侧边栏 - 你的动态

```
┌─────────────────────┐
│ 📊 Your activity   │  ← 你的活动统计
│ 🔔 Notifications   │  ← 通知中心
│ ⭐ Starred repos    │  ← 你点过星的项目
│ 📚 Your repositories│  ← 你的所有仓库
└─────────────────────┘
```

#### 3️⃣ 中间主区域 - 动态流

显示你关注的人的最新动态：
- 某人创建了新的仓库
- 某人给某个项目点了星
- 某人有新的提交

> 💡 **小贴士**：刚开始使用时，这里可能是空的。关注（Follow）一些活跃开发者后，这里会丰富起来。

---

## 仓库页面

这是你最常访问的页面！一个典型的仓库页面长这样：

### 📂 仓库主页结构

```
┌────────────────────────────────────────────────────────┐
│ [用户名] / [仓库名]                                    │
│ 📝 仓库描述                                            │
│ 🌟 123 ⭐  🍴 45 🍴  👁️ 678 👀  🔔 Watch  ⭐ Star  🍴 Fork │
├────────────────────────────────────────────────────────┤
│ [Code] [Issues 12] [Pull requests 3] [Actions] [Projects] │
│ [Wiki] [Security] [Insights] [Settings]               │
├────────────────────────────────────────────────────────┤
│ 📄 README.md                                           │
│                                                        │
│ [文件列表]                                             │
│ ├── 📄 README.md                                       │
│ ├── 📁 src/                                            │
│ ├── 📁 docs/                                           │
│ └── 📄 LICENSE                                         │
└────────────────────────────────────────────────────────┘
```

### 🔘 顶部按钮详解

#### ⭐ Star（点赞/收藏）

**作用**：给项目点赞，表示你喜欢或收藏这个项目。

**意义**：
- 📊 对作者：Star 数量代表项目的受欢迎程度
- 📚 对你：收藏的项目可以在"Your stars"页面找到
- 🔥 对社区：高 Star 项目通常质量较好

**如何操作**：点击右上角的 ⭐ Star 按钮即可。

> ⚠️ **注意**：Star 不等于 Follow！Star 是给项目的，Follow 是给人的。

#### 🍴 Fork（复刻）

**作用**：复制别人的仓库到你的账号下。

**使用场景**：
- 想给开源项目贡献代码
- 想基于别人的项目做修改
- 想保存一份副本供自己使用

**操作步骤**：
1. 点击右上角的 🍴 Fork 按钮
2. 等待几秒，GitHub 会复制一份到你的账号
3. 你现在可以随意修改这个副本

#### 👁️ Watch（关注）

**作用**：订阅仓库的通知。

**三个级别**：
- 👁️ **All Activity**（所有活动）：每次有新的 Issue、PR 都会通知你
- 💬 **Participating**（仅参与）：只有当你被 @ 提及时才通知
- 🔇 **Ignore**（忽略）：不接收通知

**如何修改**：点击 👁️ Watch 按钮，选择级别。

#### 🔔 Sponsor（赞助）

**作用**：给作者打钱，支持开源创作。

**说明**：不是所有项目都有这个功能，只有作者开启了 Sponsors 才会有。

---

### 📑 标签页详解

#### 📁 Code（代码）

这是默认打开的页面，显示仓库的文件列表。

**功能区域**：

```
┌────────────────────────────────────────────┐
│ 🌿 [Branch: main ▼]  [Tags ▼]            │  ← 切换分支/标签
│ [Go to file]  [Add file ▼]  [Code ▼]    │  ← 快速操作
├────────────────────────────────────────────┤
│ 📄 文件名          | 最后提交信息    | 时间 │
│ 📄 README.md      | Update docs    | 2d ago│
│ 📁 src/           | Add feature    | 3d ago│
└────────────────────────────────────────────┘
```

| 按钮 | 作用 |
|------|------|
| **Branch 切换器** | 切换查看不同分支的代码 |
| **Go to file** | 快速跳转到某个文件（快捷键：T） |
| **Add file** | 新建文件或上传文件 |
| **Code** | 下载代码（三种方式：HTTPS、SSH、GitHub CLI） |

#### 🐛 Issues（问题追踪）

**作用**：讨论区，用于报告 Bug、提出功能请求、询问问题。

**界面元素**：

```
┌────────────────────────────────────────────┐
│ 🟢 12 Open  🔴 45 Closed                 │  ← 状态筛选
│ [New Issue]  [Filters]  [Labels]  [Milestones] │
├────────────────────────────────────────────┤
│ 🐛 Bug: Login fails on Safari  #123      │
│    @user1 opened 2 days ago               │
│                                            │
│ ✨ Feature: Add dark mode    #122          │
│    @user2 opened 3 days ago               │
└────────────────────────────────────────────┘
```

#### 🔀 Pull Requests（拉取请求）

**作用**：提交代码修改，请求合并到主分支。

**详细说明见 [第9章：Fork 与 Pull Request](chapter09-fork-pr.md)**。

#### ⚡ Actions（自动化）

**作用**：配置自动化流程（CI/CD）。

**功能**：
- 自动运行测试
- 自动构建项目
- 自动部署到服务器

**详细说明见 [第19章：GitHub Actions](chapter19-actions-intro.md)**。

#### 📊 Projects（项目管理）

**作用**：使用看板方式管理项目任务。

**类似工具**：Trello、Jira。

**详细说明见 [第11章：Projects](chapter11-projects-management.md)**。

#### 📖 Wiki（文档）

**作用**：编写详细的项目文档。

**适用场景**：
- 项目使用手册
- API 文档
- 设计文档

#### 🔒 Security（安全）

**作用**：管理项目的安全设置。

**功能**：
- 安全公告
- 依赖漏洞扫描
- 密钥扫描

#### 📈 Insights（洞察）

**作用**：查看项目的统计信息。

**包含内容**：
- 👥 贡献者统计
- 📊 提交频率
- 💻 代码语言分布
- 🔀 分支合并情况

#### ⚙️ Settings（设置）

**作用**：配置仓库的各种设置。

**重要选项**：

| 设置项 | 说明 |
|--------|------|
| **General** | 仓库名称、描述、可见性 |
| **Collaborators** | 添加协作者 |
| **Branches** | 设置保护分支 |
| **Security & analysis** | 安全功能开关 |
| **Secrets and variables** | 存储密钥（用于 Actions） |
| **Pages** | 开启 GitHub Pages 网站 |

> ⚠️ **警告**：Settings 页面很重要，不要随意修改不了解的设置！

---

## 个人资料页

访问方式：点击右上角头像 → Your profile

### 📇 资料页布局

```
┌────────────────────────────────────────────────────────┐
│ [头像]  @你的用户名                                   │
│ 📍 所在地  🔗 个人网站  🐦 社交媒体                    │
│ 📝 个人简介                                           │
├────────────────────────────────────────────────────────┤
│ [Overview] [Repositories 12] [Projects] [Packages]    │
│ [Stars 123] [Followers 45] [Following 67]            │
├────────────────────────────────────────────────────────┤
│ 🔥 Contribution Graph（贡献图）                        │
│                                                         │
│ 你的提交记录会在这里以绿色方块显示                        │
└────────────────────────────────────────────────────────┘
```

### 🔥 贡献图（Contribution Graph）

这是 GitHub 最有名的特性之一！

**含义**：
- 每个绿色方块代表一天
- 颜色越深，当天的提交越多
- 连续提交会形成"火焰"效果

**如何有漂亮的贡献图**：
- 每天提交代码（哪怕只是一行）
- 参与开源项目
- 创建自己的项目

> 💡 **小贴士**：很多公司招聘时会看这个图，连续的绿色方块很加分！

### 📊 统计卡片

可以在 README 中添加动态统计卡片：

```markdown
![Your GitHub stats](https://github-readme-stats.vercel.app/api?username=你的用户名&show_icons=true)
```

---

## Issues 页面

这是 GitHub 的"论坛"功能。

### 创建新 Issue

1. 进入仓库的 Issues 标签
2. 点击 **New Issue** 按钮
3. 选择 Issue 类型（如果配置了模板）
4. 填写标题和描述
5. 添加标签（Label）、指派给人（Assignee）
6. 点击 **Submit new issue**

### Issue 模板

好的项目会有 Issue 模板，引导你提供必要的信息。

**常见模板**：
- 🐛 Bug Report（Bug 报告）
- ✨ Feature Request（功能请求）
- ❓ Question（提问）

---

## Pull Requests 页面

### 创建 Pull Request

1. Fork 别人的仓库
2. 在你的副本上修改代码
3. 点击 **Pull Request** 标签
4. 点击 **New Pull Request**
5. 选择基础仓库和你的仓库
6. 填写 PR 标题和描述
7. 点击 **Create Pull Request**

### PR 的状态

| 状态 | 含义 |
|------|------|
| 🟡 Open | 等待审查 |
| ✅ Merged | 已合并 |
| ❌ Closed | 已关闭（未合并） |

---

## 设置页面

### 重要设置项

#### Profile（个人资料）

- **Name**：显示名称
- **Bio**：个人简介
- **Company**：公司
- **Location**：所在地
- **Website**：个人网站

#### Account（账号安全）

- **Change password**：修改密码
- **Two-factor authentication**：开启两步验证（**强烈推荐！**）
- **Sessions**：查看活跃会话

#### Emails（邮箱）

- **Primary email**：主邮箱
- **Backup email**：备用邮箱
- **Email preferences**：邮件通知设置

#### Developer settings（开发者设置）

- **Personal access tokens**：生成 Token（**重点！**）
- **OAuth Apps**：授权第三方应用
- **SSH and GPG keys**：配置 SSH 密钥

> ⚠️ **重要**：Token 和 SSH 密钥涉及安全，详见 [第17章：Token 指南](chapter17-token-guide.md) 和 [第22章：SSH 配置](chapter22-ssh-key-setup.md)

---

## 移动端界面

GitHub 有官方移动 App（iOS 和 Android）。

### 移动端功能

- ✅ 查看通知
- ✅ 阅读和评论 Issue/PR
- ✅ 审查和合并 PR
- ❌ 不支持复杂代码编辑

### 推荐使用场景

- 📱 在通勤路上查看通知
- 📱 快速回复评论
- 📱 紧急合并重要的 PR

---

## 📝 实战练习

### 练习1：探索个人主页

**任务**：
1. 登录 GitHub，进入你自己的个人主页
2. 找到以下区域：Contributions 热力图、Repositories 列表、Pinned 仓库
3. 尝试 Pin 一个仓库到你的首页

### 练习2：熟悉仓库页面

**任务**：
1. 找一个你感兴趣的公开仓库
2. 在这个仓库中找到：README 内容、文件列表、Issues 页面、Pull Requests 页面
3. 记录每个页面的 URL 格式（观察 URL 模式）

### 练习3：探索 Settings 页面

**任务**：
1. 进入 Settings → Developer settings
2. 查看 Personal access tokens 页面（先不创建）
3. 浏览左侧菜单，了解 Settings 有哪些子页面

---

## 📚 延伸阅读

- [第4章：第一个仓库](chapter04-first-repository.md) - 创建你的第一个 GitHub 仓库
- [第13章：Star 的意义](chapter13-star-meaning.md) - 深入了解 Star 的作用
- [第17章：Token 完全指南](chapter17-token-guide.md) - Developer settings 中的 Token 详解

---

## 📝 本章小结

你现在应该了解了：

- ✅ GitHub 的各个页面布局
- ✅ 每个按钮的作用
- ✅ 如何导航到不同功能

### 下一步学习

- 想创建自己的仓库？去 [第4章：第一个仓库](chapter04-first-repository.md)
- 想下载别人的代码？去 [第5章：下载代码](chapter05-download-code.md)
- 想了解 Star 的意义？去 [第13章：Star 的意义](chapter13-star-meaning.md)

---

## 常见问题

**Q: 为什么我的贡献图不显示绿色？**  
A: 可能是邮箱设置问题。确保你提交代码时使用的邮箱是 GitHub 绑定的邮箱。

**Q: Fork 后的仓库会同步原仓库的更新吗？**  
A: 不会自动同步。你需要手动配置上游仓库（upstream）并拉取更新。

**Q: 我可以取消 Star 吗？**  
A: 可以，再次点击 Star 按钮即可取消。

**Q: Watch 和 Star 有什么区别？**  
A: Star 是点赞/收藏，Watch 是订阅通知。你可以 Star 但不 Watch，这样不会收到通知。

---

<p align="right">
  <a href="chapter02-registration-setup.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter04-first-repository.md">下一章 →</a>
</p>