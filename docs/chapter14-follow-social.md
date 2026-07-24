# 第14章：Follow 与社交 🤝

> GitHub 不只是一个代码托管平台，它还是一个技术社交网络。本章教你如何建立你的技术社交圈。

## 📋 目录

- [什么是 Follow？](#什么是-follow)
- [为什么要 Follow 别人？](#为什么要-follow-别人)
- [如何找到值得关注的人？](#如何找到值得关注的人)
- [社交功能详解](#社交功能详解)
- [建立个人品牌](#建立个人品牌)
- [GitHub Mobile App](#github-mobile-app)
- [常见问题](#常见问题)

---

## 什么是 Follow？

### 基本定义

**Follow（关注）** 类似于微博、Twitter 的"关注"功能。

### 作用

| 功能 | 说明 |
|------|------|
| **动态推送** | 你关注的人有新动态，会显示在你的首页 |
| **发现项目** | 通过关注的人，发现好项目 |
| **建立连接** | 和同领域的开发者建立联系 |

---

## 为什么要 Follow 别人？

### 对于初学者

```
关注大佬 → 看他们在做什么项目 → 学习优秀代码 → 提升技术
```

**示例**：

```
你关注了：
├── Evan You（Vue.js 作者）
│   → 你会看到：Evan 给某个 Issue 提了意见
│   → 你会看到：Evan 给某个 PR 点了 Review
│   → 学习：大佬是怎么审查代码的？
│
├── Linus Torvalds（Linux 作者）
│   → 你会看到：Linus 合并了哪些 PR
│   → 学习：顶级项目的维护流程
│
└── 你同事
    → 你会看到：同事在做什么项目
    → 发现：可以互相学习
```

---

### 对于资深开发者

| 好处 | 说明 |
|------|------|
| **招聘** | 招聘官会看你的 Follow 列表，判断你的技术圈 |
| **影响力** | Follow 你的人越多，你的项目越容易被看到 |
| **合作** | 通过关注，找到可以合作的人 |

---

## 如何找到值得关注的人？

### 方法1：通过项目找人 ✅

```
1. 找到一个你喜欢的项目（如 Vue.js）
2. 进入项目页面 → Contributors
3. 点击贡献者的头像 → Follow
```

---

### 方法2：通过 GitHub 推荐

```
GitHub 首页 → "People you may know"
│
GitHub 会根据你关注的人、Star 的项目，推荐相似的人。
```

---

### 方法3：通过 Twitter/X

很多开发者会在 Twitter 上分享自己的 GitHub 账号。

**示例**：

```
Twitter 上关注了 @youyuxi（尤雨溪）
↓
看到他发的推文："我刚开源了一个新项目..."
↓
点击链接，进入他的 GitHub
↓
Follow！
```

---

## 社交功能详解

### 1️⃣ Follow（关注）

#### 如何关注？

```
方法1：访问用户的个人主页 → 点击 Follow 按钮
方法2：在贡献者列表中点击头像 → Follow
```

#### 查看你的 Follow 列表

```
方法1：访问 https://github.com/你的用户名?tab=following
方法2：点击你的头像 → Your profile → Following
```

---

### 2️⃣ Followers（粉丝）

**作用**：显示有多少人关注了你。

**如何查看？**

```
访问 https://github.com/你的用户名?tab=followers
```

> 💡 **建议**：
> - 不定期查看你的粉丝
> - 如果对方是真实用户（不是机器人），可以回关
> - 建立互惠的社交网络

---

### 3️⃣ Sponsoring（赞助）

**作用**：给开源作者打钱，支持他们继续维护项目。

#### 如何赞助？

```
1. 进入作者的 GitHub 主页
2. 点击 Sponsor 按钮（如果有）
3. 选择赞助金额（通常 $5/月 起）
4. 支付
```

**知名开源项目的赞助：**

| 项目 | 作者月收入（估算） |
|------|-------------------|
| [Vue.js](https://github.com/sponsors/youyuxi) | $5,000+ |
| [VS Code](https://github.com/sponsors/microsoft) | 企业赞助 |
| [Webpack](https://github.com/sponsors/webpack) | $2,000+ |

---

### 4️⃣ Discussions（讨论）

**作用**：项目的"论坛"，适合长篇讨论。

#### 如何参与？

```
1. 进入仓库页面 → Discussions 标签
2. 选择分类（Q&A、Ideas、Show and tell）
3. 创建新讨论或参与已有讨论
```

---

## 建立个人品牌

### 为什么需要个人品牌？

| 好处 | 说明 |
|------|------|
| **求职** | 招聘官能通过你的 GitHub 了解你的技术能力 |
| **影响力** | 你的项目更容易被 Star |
| **合作机会** | 有人会主动联系你合作 |
| **演讲邀请** | 技术会议会邀请有影响力的人 |

---

### 如何建立个人品牌？

#### 1️⃣ 完善个人资料 ✅

```
必须有的：
├── 专业的头像
├── 有意义的 Bio
├── 个人网站链接
└──  pinned 项目（置顶项目）
```

**如何置顶项目？**

```
1. 访问你的个人主页
2. 点击 Customize your pins
3. 选择你想置顶的项目（最多 6 个）
4. 保存
```

---

#### 🏆 Profile README（个人主页 README）

**Profile README** 是一个特殊的仓库，它的 README 会展示在你的 GitHub 个人主页顶部。

**如何创建**：
1. 创建一个与你的 GitHub 用户名**同名**的仓库（如 `GTX950L/GTX950L`）
2. 勾选 **Add a README file**
3. 编辑 README.md，内容会自动显示在你的个人主页

**内容建议**：

```markdown
# 你好，我是小明 👋

## 🚀 关于我
- 🔭 我正在做：开源项目 A
- 🌱 正在学：Rust 和 WebAssembly
- 👯 希望合作：Python 数据工具
- 💬 可以问我：Python、Git、开源

## 🛠️ 技术栈
![Python](https://img.shields.io/badge/-Python-3776AB?style=flat&logo=python)
![JavaScript](https://img.shields.io/badge/-JavaScript-F7DF1E?style=flat&logo=javascript)

## 📊 GitHub 统计
![GitHub Stats](https://github-readme-stats.vercel.app/api?username=你的用户名&show_icons=true)
```

> 💡 **效果**：别人访问你的主页时，第一眼就看到你的自我介绍、技术栈和项目统计，印象分直接拉满！

---

#### 2️⃣ 持续贡献

```
✅ 好的做法：
├── 每天提交代码（哪怕只是一行）
├── 持续维护自己的项目
└── 给别人的项目贡献代码

❌ 不好的做法：
├── 一个月提交 100 次，然后消失
└── 只 Fork 不贡献
```

---

#### 3️⃣ 写技术博客

```
方法1：使用 GitHub Pages 搭建博客
      → 见 [第20章：GitHub Pages](chapter20-pages-website.md)

方法2：在掘金、知乎、Dev.to 写文章
      → 在文章末尾放上你的 GitHub 链接

方法3：在项目的 README 中放上教程
      → 帮助别人使用你的项目
```

---

#### 4️⃣ 参与社区

```
✅ 好的做法：
├── 在 Issues 中友好地帮助新手
├── 在 Discussions 中分享经验
├── 在 Twitter 上分享你的项目
└── 参加开源活动（Hacktoberfest 等）
```

---

## 📱 GitHub Mobile App

> GitHub Mobile 是 GitHub 官方手机 App，让你随时随地管理项目。

### 下载方式

| 平台 | 下载方式 |
|------|---------|
| **iOS** | App Store 搜索 "GitHub" |
| **Android** | Google Play / 国内各大应用市场搜索 "GitHub" |

### 能做什么？

```
├── 📋 查看和管理 Issues / PR
│   ├── 浏览 Issue 详情和评论
│   ├── 合并 Pull Request
│   └── 回复讨论
│
├── 🔔 接收通知
│   ├── @ 提及、Review 请求
│   ├── PR 审查通过/失败
│   └── 推送和构建状态
│
├── 👀 审查代码
│   ├── 浏览文件变更
│   ├── 添加行内评论
│   └── 批准或请求修改
│
├── 📝 编辑文件
│   ├── 直接修改代码
│   ├── 创建新文件
│   └── 提交变更
│
└── 🔍 探索项目
    ├── 搜索仓库和用户
    ├── 浏览 Trending
    └── 管理 Star 列表
```

### 特色功能

**1. 暗色模式**
App 支持跟随系统或手动切换暗色/亮色模式。

**2. 生物识别解锁**
支持 Face ID / 指纹解锁，快速安全地访问。

**3. 离线缓存**
在有网络时浏览过的 Issue 和 PR，无网络时可继续查看。

**4. Widget（iOS）**
在主屏幕添加 GitHub Widget，快速查看通知数。

> 💡 **适用场景**：通勤路上审查 PR、紧急合并热修复、随时响应 Issue 讨论。

---

## 常见问题

### Q1: 可以取消关注吗？

**A**: ✅ **可以！**

```
方法：访问对方的主页 → 点击 Unfollow
```

**礼仪**：

- ✅ 你可以随时取消关注，不需要理由
- ❌ 但不要恶意取关（今天 Follow，明天 Unfollow，刷粉丝数）

---

### Q2: 如何知道谁关注了我？

**A**: 

```
方法1：访问 https://github.com/你的用户名?tab=followers
方法2：GitHub 不会通知你"有人关注了你"
      → 需要手动查看
```

> 💡 **提示**：可以定期查看粉丝列表，回关有价值的人。

---

### Q3: Follow 和 Star 有什么区别？

| 操作 | 对象 | 作用 |
|------|------|------|
| **Follow** | 人 | 关注这个人的动态 |
| **Star** | 项目 | 点赞/收藏这个项目 |

**示例**：

```
你 Follow 了 Evan You（人）
→ 你会看到他的动态

你 Star 了 Vue.js（项目）
→ 你收藏了这个项目
```

---

## 📝 实战练习

### 练习1：关注 5 个开发者

**任务**：
1. 找一个你喜欢的开源项目
2. 查看 Contributors 列表
3. 关注其中 5 个活跃的贡献者
4. 查看你的首页动态，看看他们的最新活动

---

### 练习2：完善你的个人品牌

**任务**：
1. 检查你的 Bio 是否有意义
2. 置顶你的 3 个最佳项目
3. 写一篇技术博客，介绍你的项目
4. 分享到社交媒体

---

### 练习3：参与社区讨论

**任务**：
1. 找一个你使用的开源项目
2. 进入 Discussions 页面
3. 回答一个新手的问题
4. 体验帮助别人的感觉 😊

---

## 📚 延伸阅读

- [第13章：Star 的意义](chapter13-star-meaning.md) - 了解 Star 的作用
- [第15章：README 编写艺术](chapter15-readme-art.md) - 写出吸引人的项目介绍
- [第20章：GitHub Pages 搭建网站](chapter20-pages-website.md) - 搭建个人博客

---

## 📝 本章小结

你现在了解了：

- ✅ 什么是 Follow，为什么要关注别人
- ✅ 如何找到值得关注的人
- ✅ GitHub 的社交功能（Followers、Sponsoring、Discussions）
- ✅ 如何建立个人品牌
- ✅ GitHub Mobile App 的使用方法

### 💡 核心要点

> **GitHub 不只是一个代码托管平台。**
> 
> 它是一个技术社交网络。通过关注、Star、贡献，你可以建立自己的技术圈，提升影响力。

---

<p align="right">
  <a href="chapter13-star-meaning.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter15-readme-art.md">下一章 →</a>
</p>