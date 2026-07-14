# 第30章：GitHub Discussions — 社区讨论 💬

> Issues 适合报告 Bug 和提需求。如果你的问题是"怎么用"或"你觉得怎么样"，Discussions 是更好的选择。

## 📋 目录

- [什么是 Discussions？](#什么是-discussions)
- [Issues vs Discussions](#issues-vs-discussions)
- [启用和创建 Discussions](#启用和创建-discussions)
- [讨论分类](#讨论分类)
- [问答模式](#问答模式)
- [社区运营技巧](#社区运营技巧)
- [常见问题](#常见问题)

---

## 什么是 Discussions？

### 基本定义

**GitHub Discussions** 是 GitHub 的**社区讨论空间**。

### 可以做什么？

| 用途 | 说明 |
|------|------|
| **提问和解答** | 用户问"这个功能怎么用"，其他人回答 |
| **想法交流** | 讨论"你觉得下一代版本应该有什么功能" |
| **社区公告** | 发布新版本、维护通知 |
| **水区闲聊** | 非技术话题的交流 |

### Discussions 和 Issue 的本质区别

```
Issues = 工作任务追踪
├── 🐛 Bug 报告
├── ✨ 功能请求
└── 📋 任务分配

Discussions = 社区交流空间
├── 💬 使用疑问
├── 💡 想法讨论
├── 📢 公告通知
└── 🗣️ 社区闲聊
```

---

## Issues vs Discussions

| 对比项 | Issues | Discussions |
|--------|--------|-------------|
| **目的** | 任务追踪 | 社区交流 |
| **是否可分配** | ✅ 可以 Assign | ❌ 不能直接 Assign |
| **是否关联 PR** | ✅ 可以 | ❌ 不直接关联 |
| **是否可关闭** | ✅ 有 Open/Closed | ✅ 可以标记为已回答 |
| **投票** | ✅ Reactions | ✅ 可以 Upvote/Downvote |
| **分类** | 标签（Labels） | 分类（Categories）+ 标签 |
| **问答支持** | ❌ 不原生支持 | ✅ 有问答模式 |
| **适合** | 具体可执行的任务 | 开放式的讨论和问答 |

### 如何选择？

```
你想做什么？
│
├─ 报告一个 Bug → Issue
├─ 请求一个新功能 → Issue 或 Discussion
├─ 问"这个东西怎么用" → Discussion
├─ 讨论"未来发展方向" → Discussion
├─ 发公告 → Discussion
└─ 闲聊 → Discussion
```

---

## 启用和创建 Discussions

### 启用 Discussions

```
1. 进入仓库 Settings
2. 向下滚动到 "Features"
3. 勾选 "Discussions"
4. 选择分类模板
5. 点击 "Set up discussions"
```

### 创建一个讨论

```
1. 进入仓库的 Discussions 标签
2. 点击 "New discussion"
3. 选择分类
4. 填写标题和内容
5. 点击 "Start discussion"
```

### 分类选择

创建讨论时，你需要选择分类：

| 分类 | 说明 | 自动前缀 |
|------|------|---------|
| **💬 General** | 通用讨论 | 无 |
| **💡 Ideas** | 新想法、功能建议 | 💡 |
| **🏆 Show and tell** | 展示你的作品 | 🏆 |
| **🙏 Q&A** | 问答模式（接受答案） | ❓ |
| **🗳️ Poll** | 投票（需要额外配置） | 📊 |

---

## 讨论分类

### 1️⃣ 问答（Q&A）✅

**Q&A 模式是 Discussions 最有用的功能之一。**

```
用户提问：
┌──────────────────────────────┐
│ ❓ 如何用 API 创建 Issue？    │
│                              │
│ 我看了文档，但没有找到     │
│ 创建 Issue 的 API 端点...    │
└──────────────────────────────┘

其他人回答：
┌──────────────────────────────┐
│ 💡 用 POST /repos/.../issues │
│                              │
│ 参考这个代码：                │
│ curl -X POST ...              │
└──────────────────────────────┘

提问者标记为已回答：
┌──────────────────────────────┐
│ ✅ 问题已解决！               │
│ 这个答案解决了我的问题     │
└──────────────────────────────┘
```

#### 如何标记答案

```
1. 在回复的右下角，点击 ✅ 图标
2. 或者点击回复旁边的 "Mark as answer"
3. 被标记的回复会置顶显示
```

> 💡 **效果**：下次有相同问题的用户，第一眼就能看到正确答案。

### 2️⃣ 投票（Polls）

```
你可以创建一个投票，让社区决定：
├── "下个版本应该优先做什么？"
├── A) 深色模式 (30 票)
├── B) 移动端适配 (45 票)  ← 胜出
└── C) API 文档 (25 票)
```

#### 启用投票

Polls 功能需要额外配置：
1. 安装 [GitHub Polls App](https://github.com/marketplace/polls)
2. 在 Discussions 中点击 "New poll"
3. 设置问题 + 选项

---

## 问答模式

### 如何写出好的问题

#### ❌ 不好的问题

```
为什么我的代码报错？
（没有报错信息、没有代码、没有上下文）
```

#### ✅ 好的问题

```
# ❓ 问题：Flask 路由返回 404

当我访问 /api/users 时，Flask 返回 404 Not Found。

## 代码
```python
@app.route('/api/users')
def get_users():
    return jsonify([{"id": 1, "name": "小明"}])

## 尝试过的解决方法
- 确认路由已注册：app.url_map 显示有这条路由
- 重新启动了服务器

## 环境
- Python 3.12
- Flask 3.0
- Windows 11
```

### 如何写出好的回答

#### ❌ 不好的回答

```
看看文档吧。
```

#### ✅ 好的回答

```
# 💡 解决方案

你的路由定义是正确的，但 Flask 默认是大小写敏感的。
试试加一个 `strict_slashes=False`：

```python
@app.route('/api/users', strict_slashes=False)
def get_users():
    return jsonify([{"id": 1, "name": "小明"}])
```

## 为什么？

Flask 默认情况下，`/api/users` 和 `/api/users/` 是不同的路由。
加上 `strict_slashes=False` 后，两者都会被匹配到同一个函数。

如果还有问题，请告诉我！😊
```

---

## 社区运营技巧

### 1. 置顶重要讨论

```
进入讨论页面 → 右上角 ⚙️ → Pin discussion
```

适合置顶的内容：
- 📢 **项目公告**（新版本发布、迁移通知）
- 📚 **入门指南**（FAQ、常见问题汇总）
- 🏆 **贡献者展示**

### 2. 使用标签（Labels）

```
Q&A 相关：
├── answered ✅     ← 问题已解决
├── unanswered ❓   ← 等待回答
└── duplicate 📋   ← 重复问题

公告相关：
├── announcement 📢  ← 官方公告
└── release 🎉       ← 版本发布
```

### 3. 回复模板

可以在仓库设置中配置回复模板：

```
Settings → Repository templates → 添加 Discussion 模板
```

### 4. 鼓励社区参与

```
✅ 好的做法：
├── 新用户提问 → 24 小时内回复
├── 好问题 → 点赞 👍
├── 好回答 → 标记为答案 ✅
└── 月度最佳贡献者 → 在公告频道表扬
```

### 5. 将 Discussion 转为 Issue

如果讨论中确定了具体任务：

```
1. 打开 Discussion 页面
2. 点击右下角 "Create issue from discussion"
3. 确认 Issue 内容
4. Issue 会自动关联到这个 Discussion
```

---

## 常见问题

### Q1: Discussions 可以搜索吗？

**A**: ✅ 可以！和 Issues 使用相同的搜索语法。

```
# 在仓库内搜索
is:discussion 部署问题

# 按分类搜索
category:Q&A

# 按标签搜索
label:answered
```

### Q2: 如何防止水贴/垃圾广告？

**A**: 

```
方法1：限制发言权限
├── 只有协作者可以创建 Discussion
└── Settings → Discussions → 修改权限

方法2：启用内容审核
├── 开启 "Code of conduct"
└── 违规内容 → 报告 → 管理员删除

方法3：设置过滤词
└── Settings → Moderation → Blocked words
```

### Q3: 可以用 Discussions 做客服吗？

**A**: ✅ **适合小项目！**

```
优点：
├── 免费
├── 和 GitHub 仓库集成
└── 社区可以互相帮助

局限：
├── 没有工单系统
├── 没有 SLA 管理
└── 大项目建议用 Zendesk、Discourse 等专业工具
```

---

## 📝 实战练习

### 练习1：启用 Discussions

**任务**：
1. 在你的仓库中启用 Discussions
2. 配置分类（General、Q&A、Ideas）
3. 新建一个"自我介绍"讨论

### 练习2：问答实践

**任务**：
1. 在 Q&A 分类中创建一个问题（如"如何安装这个项目"）
2. 用另一个账号回答
3. 标记为正确答案

### 练习3：创建投票

**任务**：
1. 安装 GitHub Polls App
2. 创建一个投票："你最喜欢什么编程语言？"
3. 邀请朋友投票
4. 查看结果

---

## 📚 延伸阅读

- [第10章：Issues 使用指南](chapter10-issues-guide.md) - 任务追踪
- [第13章：Star 的意义](chapter13-star-meaning.md) - 社区指标
- [第15章：README 编写艺术](chapter15-readme-art.md) - 项目门面

---

## 📝 本章小结

你现在学会了：

- ✅ Discussions 和 Issues 的区别
- ✅ 如何启用和创建 Discussions
- ✅ Q&A 问答模式的用法
- ✅ 分类管理（General、Q&A、Ideas）
- ✅ 社区运营技巧

### 💡 核心要点

> **Discussions 让项目从"代码仓库"变成"开发者社区"。**
> 
> 有了 Discussions，用户不再只是下载代码的人，而是参与到项目成长中的伙伴。

---

<p align="right">
  <a href="chapter29-codespaces.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="appendix-e-conventional-commits.md">附录E：Conventional Commits →</a>
</p>
