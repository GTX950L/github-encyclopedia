# 附录B：Git 常用命令速查表 🎯

> 快速查找 Git 命令，每个命令附带最常用的用法

---

## 🏗️ 仓库管理

| 命令 | 说明 | 示例 |
|------|------|------|
| `git init` | 初始化本地仓库 | `git init my-project` |
| `git clone` | 克隆远程仓库 | `git clone https://github.com/user/repo.git` |
| `git remote` | 管理远程仓库 | `git remote add origin <url>` |

---

## 📝 基本操作

| 命令 | 说明 | 示例 |
|------|------|------|
| `git status` | 查看当前状态 | `git status` |
| `git add` | 暂存文件 | `git add file.txt` / `git add .` |
| `git commit` | 提交更改 | `git commit -m "feat: 添加新功能"` |
| `git push` | 推送到远程 | `git push origin main` |
| `git pull` | 拉取远程更新 | `git pull origin main` |
| `git fetch` | 获取远程信息（不合并） | `git fetch origin` |

---

## 🌿 分支操作

| 命令 | 说明 | 示例 |
|------|------|------|
| `git branch` | 列出所有分支 | `git branch` / `git branch -a` |
| `git branch <name>` | 创建分支 | `git branch feature-login` |
| `git checkout` | 切换分支 | `git checkout feature-login` |
| `git switch` | 切换分支（新版） | `git switch feature-login` |
| `git checkout -b` | 创建并切换 | `git checkout -b feature-login` |
| `git merge` | 合并分支 | `git merge feature-login` |
| `git branch -d` | 删除分支 | `git branch -d feature-login` |
| `git push --delete` | 删除远程分支 | `git push origin --delete feature-login` |

---

## 📜 查看历史

| 命令 | 说明 | 示例 |
|------|------|------|
| `git log` | 查看提交历史 | `git log --oneline --graph` |
| `git log --author` | 按作者筛选 | `git log --author="小明"` |
| `git log --since` | 按时间筛选 | `git log --since="2026-01-01"` |
| `git diff` | 查看未暂存的修改 | `git diff` |
| `git diff --staged` | 查看已暂存的修改 | `git diff --staged` |
| `git show` | 查看某次提交详情 | `git show abc123` |
| `git blame` | 查看每行代码的作者 | `git blame file.txt` |

---

## ↩️ 撤销与回退

| 命令 | 说明 | 示例 |
|------|------|------|
| `git restore` | 撤销工作区修改 | `git restore file.txt` |
| `git restore --staged` | 取消暂存 | `git restore --staged file.txt` |
| `git reset --soft` | 撤销提交（保留修改） | `git reset --soft HEAD~1` |
| `git reset --hard` | 彻底回退 ⚠️ | `git reset --hard HEAD~1` |
| `git revert` | 反向提交（安全） | `git revert abc123` |
| `git stash` | 暂存当前修改 | `git stash` |
| `git stash pop` | 恢复暂存的修改 | `git stash pop` |

---

## 🎯 高级命令

| 命令 | 说明 | 示例 |
|------|------|------|
| `git rebase` | 变基 | `git rebase main` |
| `git rebase -i` | 交互式变基 | `git rebase -i HEAD~3` |
| `git cherry-pick` | 挑选提交 | `git cherry-pick abc123` |
| `git bisect` | 二分查找 Bug | `git bisect start` |
| `git tag` | 打标签 | `git tag -a v1.0 -m "版本1.0"` |
| `git reflog` | 查看操作日志（救命用） | `git reflog` |

---

## 🔧 配置命令

| 命令 | 说明 | 示例 |
|------|------|------|
| `git config --global user.name` | 设置用户名 | `git config --global user.name "小明"` |
| `git config --global user.email` | 设置邮箱 | `git config --global user.email "xiaoming@example.com"` |
| `git config --list` | 查看所有配置 | `git config --list` |
| `git config --global alias` | 设置别名 | `git config --global alias.co checkout` |

---

## 🚨 常见问题解决

### 提交信息写错了

```bash
# 修改最近一次提交信息
git commit --amend -m "新的提交信息"

# 如果已经 push，需要强制推送
git push --force-with-lease
```

### 忘记创建 .gitignore

```bash
# 创建 .gitignore 后
git rm -r --cached .
git add .
git commit -m "chore: 添加 .gitignore"
```

### 合并冲突怎么办

```bash
# 1. 查看冲突文件
git status

# 2. 手动编辑冲突文件，删除 <<<< ==== >>>> 标记

# 3. 标记已解决
git add .

# 4. 完成合并
git commit -m "fix: 解决合并冲突"
```

### 不小心提交了敏感信息

```bash
# 1. 立即修改/删除敏感信息
# 2. 从历史中彻底删除
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch 敏感文件" \
  --prune-empty --tag-name-filter cat -- --all

# 3. 强制推送
git push --force --all
```

> ⚠️ 如果敏感信息已被他人看到，请立即更换密码/Token！

---

## 📥 下载单文件（不克隆整个仓库）

```bash
# 方法1：使用 wget
wget https://raw.githubusercontent.com/user/repo/main/file.py

# 方法2：使用 curl
curl -O https://raw.githubusercontent.com/user/repo/main/file.py

# 方法3：使用 SVN（GitHub 支持）
svn export https://github.com/user/repo/trunk/path/to/folder
```

---

<p align="right">
  <a href="appendix-a-project-structure.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="../README.md">返回首页 →</a>
</p>
