# Git 学习文档

## 第1课：理解 Git 基础概念（工作区、暂存区、仓库）

### 1.1 三个核心区域

Git 有三个重要的工作区域：

```
工作区 (Working Directory)
    ↓  git add
暂存区 (Staging Area / Index)
    ↓  git commit
本地仓库 (Repository)
    ↓  git push
远程仓库 (Remote Repository)
```

#### **工作区 (Working Directory)**
- 你实际编辑文件的地方
- 就是你电脑上能看到的项目文件夹
- 所有的修改最初都发生在这里

#### **暂存区 (Staging Area)**
- 临时存储区域，用于准备下一次提交
- 可以选择性地添加文件到暂存区
- 使用 `git add` 命令将文件从工作区添加到暂存区

#### **本地仓库 (Repository)**
- Git 真正存储项目历史的地方
- 位于 `.git` 文件夹中
- 使用 `git commit` 将暂存区的内容保存到仓库

### 1.2 文件的四种状态

```
未跟踪 (Untracked)  →  已跟踪 (Tracked)
                        ├─ 未修改 (Unmodified)
                        ├─ 已修改 (Modified)
                        └─ 已暂存 (Staged)
```

### 1.3 实践练习

```bash
# 查看当前状态
git status

# 查看工作区文件
ls

# 查看 .git 目录（这里存储了所有版本信息）
ls -la .git/
```

---

## 第2课：练习添加和提交文件（add & commit）

### 2.1 添加文件到暂存区

```bash
# 添加单个文件
git add 文件名.txt

# 添加多个文件
git add 文件1.txt 文件2.txt

# 添加当前目录所有文件
git add .

# 添加所有修改（包括删除）
git add -A
```

### 2.2 提交到本地仓库

```bash
# 提交暂存区的文件
git commit -m "提交说明"

# 添加并提交（跳过 git add 步骤，仅对已跟踪文件有效）
git commit -am "提交说明"

# 修改上一次提交（慎用）
git commit --amend -m "新的提交说明"
```

### 2.3 提交信息规范

好的提交信息应该：
- 简洁明了，说明做了什么
- 使用现在时态："添加功能" 而非 "已添加功能"
- 第一行不超过 50 字符

```bash
# 好的示例
git commit -m "添加用户登录功能"
git commit -m "修复购物车计算错误"
git commit -m "更新 README 文档"

# 不好的示例
git commit -m "修改"
git commit -m "更新了一些东西"
```

### 2.4 实践练习

```bash
# 1. 创建新文件
echo "Hello Git" > hello.txt

# 2. 查看状态
git status

# 3. 添加到暂存区
git add hello.txt

# 4. 再次查看状态
git status

# 5. 提交
git commit -m "添加 hello.txt 文件"

# 6. 查看状态
git status
```

---

## 第3课：查看历史和差异（log & diff）

### 3.1 查看提交历史

```bash
# 查看完整历史
git log

# 单行显示（推荐）
git log --oneline

# 显示最近 3 条记录
git log -3

# 图形化显示分支
git log --oneline --graph --all

# 显示每次提交的文件变化
git log --stat

# 查看某个文件的历史
git log 文件名
```

### 3.2 查看差异

```bash
# 查看工作区与暂存区的差异
git diff

# 查看暂存区与最后一次提交的差异
git diff --staged
# 或
git diff --cached

# 查看两个提交之间的差异
git diff commit1 commit2

# 查看某个文件的差异
git diff 文件名
```

### 3.3 查看提交详情

```bash
# 查看最新提交的详细信息
git show

# 查看特定提交
git show commit-id

# 查看某次提交的某个文件
git show commit-id:文件路径
```

### 3.4 实践练习

```bash
# 1. 修改文件
echo "添加一行新内容" >> hello.txt

# 2. 查看差异
git diff

# 3. 添加到暂存区
git add hello.txt

# 4. 查看暂存区差异
git diff --staged

# 5. 提交
git commit -m "更新 hello.txt"

# 6. 查看历史
git log --oneline
```

---

## 第4课：分支操作（branch & checkout）

### 4.1 为什么需要分支

- 开发新功能时不影响主分支
- 多人协作时各自在独立分支工作
- 修复 bug 时创建专门的修复分支

### 4.2 分支基本操作

```bash
# 查看所有分支
git branch

# 查看所有分支（包括远程）
git branch -a

# 创建新分支
git branch 分支名

# 切换到某个分支
git checkout 分支名

# 创建并切换到新分支（常用）
git checkout -b 分支名

# 删除分支
git branch -d 分支名

# 强制删除分支
git branch -D 分支名

# 重命名分支
git branch -m 旧名称 新名称
```

### 4.3 Git 2.23+ 新命令

```bash
# 切换分支（更清晰）
git switch 分支名

# 创建并切换
git switch -c 分支名

# 恢复文件（替代 checkout 的文件恢复功能）
git restore 文件名
```

### 4.4 实践练习

```bash
# 1. 查看当前分支
git branch

# 2. 创建新分支
git branch feature-login

# 3. 切换到新分支
git checkout feature-login

# 4. 在新分支上创建文件
echo "登录功能" > login.txt
git add login.txt
git commit -m "添加登录功能"

# 5. 切换回主分支
git checkout master

# 6. 查看文件（login.txt 不存在）
ls

# 7. 再切换回 feature-login
git checkout feature-login

# 8. 查看文件（login.txt 存在）
ls
```

---

## 第5课：合并分支（merge）

### 5.1 合并的两种方式

#### **快进合并 (Fast-forward)**
当目标分支没有新提交时，直接移动指针

```
Before:          After:
master → A       master → A → B → C
         ↓                    ↑
feature  B → C            feature
```

#### **三方合并 (Three-way merge)**
当两个分支都有新提交时，创建合并提交

```
Before:             After:
master → A → D      master → A → D → M
         ↓                   ↓       ↗
feature  B → C      feature  B → C
```

### 5.2 合并操作

```bash
# 切换到目标分支（通常是 master）
git checkout master

# 合并指定分支到当前分支
git merge 分支名

# 禁用快进合并（总是创建合并提交）
git merge --no-ff 分支名

# 取消合并
git merge --abort
```

### 5.3 解决合并冲突

当两个分支修改了同一文件的同一位置时，会产生冲突：

```bash
# 1. 执行合并
git merge feature-branch

# 2. 如果有冲突，Git 会提示
# 3. 查看冲突文件
git status

# 4. 打开文件，会看到冲突标记
<<<<<<< HEAD
当前分支的内容
=======
要合并分支的内容
>>>>>>> feature-branch

# 5. 手动编辑，删除标记，保留需要的内容
# 6. 添加解决后的文件
git add 冲突文件

# 7. 完成合并
git commit -m "解决合并冲突"
```

### 5.4 实践练习

```bash
# 1. 在 master 分支创建文件
git checkout master
echo "主分支内容" > main.txt
git add main.txt
git commit -m "在主分支添加文件"

# 2. 创建并切换到新分支
git checkout -b feature-test

# 3. 修改文件
echo "功能分支内容" >> main.txt
git commit -am "在功能分支修改文件"

# 4. 切换回 master
git checkout master

# 5. 合并分支
git merge feature-test

# 6. 查看历史
git log --oneline --graph
```

---

## 第6课：撤销操作（reset & revert）

### 6.1 撤销的不同场景

#### **场景1：撤销工作区的修改**
```bash
# 恢复单个文件
git checkout -- 文件名
# 或（推荐 Git 2.23+）
git restore 文件名

# 恢复所有文件
git checkout -- .
```

#### **场景2：撤销暂存区的文件**
```bash
# 取消暂存（文件修改还在工作区）
git reset HEAD 文件名
# 或（推荐 Git 2.23+）
git restore --staged 文件名
```

#### **场景3：撤销提交**

### 6.2 reset 命令详解

```bash
# 三种模式

# --soft：只移动 HEAD，保留暂存区和工作区
git reset --soft HEAD^

# --mixed（默认）：移动 HEAD，重置暂存区，保留工作区
git reset HEAD^
git reset --mixed HEAD^

# --hard：移动 HEAD，重置暂存区和工作区（危险！）
git reset --hard HEAD^
```

```
HEAD^   上一个版本
HEAD^^  上上个版本
HEAD~3  前三个版本
```

### 6.3 revert 命令（推荐）

`revert` 通过创建新提交来撤销，保留历史记录：

```bash
# 撤销指定提交
git revert commit-id

# 撤销最近一次提交
git revert HEAD

# 撤销但不自动提交
git revert -n commit-id
```

### 6.4 reset vs revert

| 特性 | reset | revert |
|------|-------|--------|
| 历史记录 | 删除提交历史 | 保留所有历史 |
| 安全性 | 危险（尤其 --hard） | 安全 |
| 协作 | 不适合已推送的提交 | 适合所有情况 |
| 使用场景 | 本地未推送的提交 | 已推送的提交 |

### 6.5 实践练习

```bash
# === 练习1：撤销工作区修改 ===
# 1. 修改文件
echo "错误的内容" >> hello.txt

# 2. 查看状态
git status

# 3. 撤销修改
git restore hello.txt

# === 练习2：撤销暂存 ===
# 1. 修改并暂存
echo "新内容" >> hello.txt
git add hello.txt

# 2. 取消暂存
git restore --staged hello.txt

# === 练习3：撤销提交 ===
# 1. 创建测试提交
echo "测试内容" > test.txt
git add test.txt
git commit -m "测试提交"

# 2. 查看历史
git log --oneline

# 3. 使用 revert 撤销（推荐）
git revert HEAD

# 或使用 reset（仅限本地未推送）
# git reset --soft HEAD^
```

---

## 附录：常用命令速查表

### 配置
```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

### 基础操作
```bash
git init              # 初始化仓库
git status            # 查看状态
git add .             # 添加所有文件
git commit -m "说明"  # 提交
git log --oneline     # 查看历史
```

### 分支操作
```bash
git branch            # 查看分支
git branch 名称       # 创建分支
git checkout 名称     # 切换分支
git checkout -b 名称  # 创建并切换
git merge 名称        # 合并分支
git branch -d 名称    # 删除分支
```

### 远程操作
```bash
git clone URL         # 克隆仓库
git pull              # 拉取更新
git push              # 推送到远程
git remote -v         # 查看远程仓库
```

### 撤销操作
```bash
git restore 文件      # 撤销工作区修改
git restore --staged 文件  # 取消暂存
git reset HEAD^       # 撤销上次提交
git revert HEAD       # 安全撤销提交
```

---

## 学习建议

1. **每天实践**：每个命令都动手操作一遍
2. **建立测试仓库**：创建一个专门练习的仓库
3. **多查 `git status`**：养成频繁查看状态的习惯
4. **理解原理**：不要只记命令，要理解背后的原理
5. **从简单开始**：先掌握基础命令，再学习高级用法

## 下一步学习方向

- 远程仓库操作（GitHub/GitLab）
- 协作开发流程（Pull Request）
- Git 工作流（Git Flow, GitHub Flow）
- 高级功能（rebase, cherry-pick, stash）
- Git 钩子（Hooks）

---

祝你学习顺利！
