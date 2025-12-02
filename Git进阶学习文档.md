# Git 进阶学习文档

## 第7课：远程仓库操作（GitHub/GitLab）

### 7.1 理解远程仓库

远程仓库是托管在网络上的 Git 仓库，常见平台：
- **GitHub** - 最流行的代码托管平台
- **GitLab** - 提供 CI/CD 功能
- **Gitee** - 国内代码托管平台
- **Bitbucket** - Atlassian 旗下平台

```
本地仓库 ←→ 远程仓库
   ↓              ↓
  你的电脑      GitHub/GitLab
```

### 7.2 配置远程仓库

```bash
# 查看远程仓库
git remote -v

# 添加远程仓库
git remote add origin https://github.com/用户名/仓库名.git

# 修改远程仓库地址
git remote set-url origin 新地址

# 删除远程仓库
git remote remove origin

# 查看远程仓库详细信息
git remote show origin
```

### 7.3 克隆仓库

```bash
# 克隆远程仓库
git clone https://github.com/用户名/仓库名.git

# 克隆到指定目录
git clone https://github.com/用户名/仓库名.git 目录名

# 克隆指定分支
git clone -b 分支名 https://github.com/用户名/仓库名.git

# 浅克隆（只克隆最近的提交历史）
git clone --depth 1 https://github.com/用户名/仓库名.git
```

### 7.4 推送到远程仓库

```bash
# 推送到远程仓库
git push origin master

# 首次推送并设置上游分支
git push -u origin master

# 推送所有分支
git push --all origin

# 推送标签
git push --tags

# 强制推送（危险！）
git push -f origin master
```

### 7.5 从远程仓库拉取

```bash
# 拉取并合并
git pull origin master

# 相当于
git fetch origin
git merge origin/master

# 拉取并使用 rebase
git pull --rebase origin master

# 只获取不合并
git fetch origin
```

### 7.6 远程分支操作

```bash
# 查看远程分支
git branch -r

# 查看所有分支（本地+远程）
git branch -a

# 拉取远程分支到本地
git checkout -b 本地分支名 origin/远程分支名

# 删除远程分支
git push origin --delete 分支名

# 更新远程分支列表
git remote update origin --prune
```

### 7.7 实践练习

```bash
# === 练习1：推送本地仓库到 GitHub ===

# 1. 在 GitHub 创建新仓库（通过网页）

# 2. 添加远程仓库
git remote add origin https://github.com/你的用户名/demo.git

# 3. 查看远程仓库
git remote -v

# 4. 推送到远程
git push -u origin master

# === 练习2：克隆和协作 ===

# 1. 克隆一个开源项目
git clone https://github.com/microsoft/vscode.git

# 2. 查看远程分支
git branch -r

# 3. 获取最新更新
git pull origin main
```

---

## 第8课：协作开发流程（Pull Request）

### 8.1 Fork 工作流

这是开源项目最常用的协作方式：

```
原始仓库 (upstream)
    ↓ fork
你的远程仓库 (origin)
    ↓ clone
你的本地仓库
```

### 8.2 完整的 PR 流程

#### **步骤1：Fork 项目**
在 GitHub 上点击 "Fork" 按钮

#### **步骤2：克隆到本地**
```bash
# 克隆你 fork 的仓库
git clone https://github.com/你的用户名/项目名.git
cd 项目名

# 添加原始仓库为上游
git remote add upstream https://github.com/原作者/项目名.git

# 验证
git remote -v
```

#### **步骤3：创建功能分支**
```bash
# 从最新的 main 创建分支
git checkout -b feature-new-function

# 进行开发
# ...编辑文件...

# 提交更改
git add .
git commit -m "添加新功能"
```

#### **步骤4：同步上游更新**
```bash
# 获取上游更新
git fetch upstream

# 切换到主分支
git checkout main

# 合并上游更新
git merge upstream/main

# 推送到你的远程仓库
git push origin main
```

#### **步骤5：推送功能分支**
```bash
# 推送功能分支
git push origin feature-new-function
```

#### **步骤6：创建 Pull Request**
1. 在 GitHub 上进入你的仓库
2. 点击 "Compare & pull request"
3. 填写 PR 标题和描述
4. 点击 "Create pull request"

### 8.3 PR 最佳实践

#### **好的 PR 标题**
```
✅ 添加用户登录功能
✅ 修复购物车计算错误 (#123)
✅ 优化数据库查询性能

❌ 更新
❌ 修改了一些东西
❌ fix bug
```

#### **好的 PR 描述模板**
```markdown
## 变更说明
简要描述这个 PR 做了什么

## 变更类型
- [ ] 新功能
- [x] Bug 修复
- [ ] 文档更新
- [ ] 性能优化

## 测试
说明如何测试这些变更

## 相关 Issue
关闭 #123
```

### 8.4 代码审查（Code Review）

#### **作为审查者**
```bash
# 拉取 PR 分支到本地测试
git fetch origin pull/ID/head:pr-branch
git checkout pr-branch

# 测试代码
# ...

# 切换回主分支
git checkout main
```

#### **审查重点**
- 代码质量和可读性
- 是否有测试
- 是否有潜在 bug
- 是否符合项目规范

### 8.5 处理审查意见

```bash
# 根据反馈修改代码
# ...编辑文件...

# 提交修改
git add .
git commit -m "根据审查意见修改代码"

# 推送更新（会自动更新 PR）
git push origin feature-new-function
```

### 8.6 实践练习

```bash
# === 练习：完整的 PR 流程 ===

# 1. Fork 一个小型开源项目

# 2. 克隆到本地
git clone https://github.com/你的用户名/项目.git
cd 项目

# 3. 添加上游
git remote add upstream https://github.com/原作者/项目.git

# 4. 创建功能分支
git checkout -b fix-typo

# 5. 修改文件（比如修复拼写错误）
echo "修复拼写错误" >> README.md

# 6. 提交
git add README.md
git commit -m "修复 README 中的拼写错误"

# 7. 推送
git push origin fix-typo

# 8. 在 GitHub 上创建 PR
```

---

## 第9课：Git 工作流

### 9.1 常见工作流对比

| 工作流 | 适合团队 | 复杂度 | 特点 |
|--------|----------|--------|------|
| Git Flow | 大型团队 | 高 | 严格规范，多分支 |
| GitHub Flow | 中小团队 | 低 | 简单直接，主分支部署 |
| GitLab Flow | 中大型团队 | 中 | 环境分支，适合 CI/CD |
| Trunk Based | 高频部署团队 | 低 | 主干开发，快速迭代 |

### 9.2 Git Flow 详解

#### **分支结构**
```
master/main     - 生产环境，只接受 release 和 hotfix 合并
develop         - 开发主分支
feature/*       - 功能分支
release/*       - 发布分支
hotfix/*        - 紧急修复分支
```

#### **工作流程**

**开发新功能**
```bash
# 从 develop 创建功能分支
git checkout develop
git checkout -b feature/user-login

# 开发完成后合并回 develop
git checkout develop
git merge --no-ff feature/user-login
git branch -d feature/user-login
git push origin develop
```

**准备发布**
```bash
# 从 develop 创建发布分支
git checkout develop
git checkout -b release/v1.0.0

# 修复发布前的 bug
# ...

# 合并到 master 和 develop
git checkout master
git merge --no-ff release/v1.0.0
git tag -a v1.0.0 -m "Release version 1.0.0"

git checkout develop
git merge --no-ff release/v1.0.0

# 删除发布分支
git branch -d release/v1.0.0
```

**紧急修复**
```bash
# 从 master 创建 hotfix 分支
git checkout master
git checkout -b hotfix/critical-bug

# 修复 bug
# ...

# 合并到 master 和 develop
git checkout master
git merge --no-ff hotfix/critical-bug
git tag -a v1.0.1 -m "Hotfix version 1.0.1"

git checkout develop
git merge --no-ff hotfix/critical-bug

# 删除 hotfix 分支
git branch -d hotfix/critical-bug
```

### 9.3 GitHub Flow 详解

更简单的工作流，适合持续部署：

```
main 分支 - 始终可部署
    ↓
创建功能分支
    ↓
定期推送到远程
    ↓
创建 Pull Request
    ↓
代码审查
    ↓
合并到 main
    ↓
立即部署
```

#### **操作步骤**
```bash
# 1. 创建分支
git checkout -b feature-branch

# 2. 开发并定期推送
git add .
git commit -m "进展更新"
git push origin feature-branch

# 3. 创建 PR（在 GitHub 上）

# 4. 审查通过后合并

# 5. 删除功能分支
git branch -d feature-branch
git push origin --delete feature-branch
```

### 9.4 选择合适的工作流

#### **使用 Git Flow 如果：**
- 有固定的发布周期
- 需要维护多个版本
- 团队规模较大

#### **使用 GitHub Flow 如果：**
- 持续部署
- 团队敏捷开发
- 只维护一个生产版本

#### **使用 GitLab Flow 如果：**
- 有多个部署环境（开发、测试、生产）
- 使用 GitLab CI/CD

### 9.5 实践练习

```bash
# === 练习：GitHub Flow ===

# 1. 从 main 创建功能分支
git checkout main
git checkout -b add-contact-form

# 2. 开发功能
echo "联系表单" > contact.html
git add contact.html
git commit -m "添加联系表单"

# 3. 推送到远程
git push -u origin add-contact-form

# 4. 在 GitHub 创建 PR

# 5. 模拟审查和合并（本地）
git checkout main
git merge add-contact-form

# 6. 清理分支
git branch -d add-contact-form
```

---

## 第10课：高级功能（rebase, cherry-pick, stash）

### 10.1 Rebase - 变基

#### **rebase 与 merge 的区别**

**Merge（合并）**
```
      A---B---C feature
     /         \
D---E---F---G---M main
```

**Rebase（变基）**
```
              A'--B'--C' feature
             /
D---E---F---G main
```

#### **基本用法**
```bash
# 将当前分支变基到 main
git checkout feature
git rebase main

# 交互式 rebase（修改历史）
git rebase -i HEAD~3

# 继续 rebase
git rebase --continue

# 中止 rebase
git rebase --abort
```

#### **交互式 rebase 命令**
```bash
pick   - 使用该提交
reword - 使用该提交，但修改提交信息
edit   - 使用该提交，但停下来修改
squash - 与上一个提交合并
drop   - 删除该提交
```

#### **实践示例**
```bash
# 合并最近 3 次提交
git rebase -i HEAD~3

# 在编辑器中：
pick abc123 第一次提交
squash def456 第二次提交
squash ghi789 第三次提交

# 保存后编辑合并后的提交信息
```

### 10.2 Cherry-pick - 挑选提交

#### **什么是 cherry-pick**
从其他分支挑选特定提交应用到当前分支

```
feature: A---B---C---D
              ↓ cherry-pick C
main:    E---F---C'
```

#### **基本用法**
```bash
# 应用单个提交
git cherry-pick commit-id

# 应用多个提交
git cherry-pick commit1 commit2

# 应用一个范围的提交
git cherry-pick start-commit^..end-commit

# 只应用修改，不提交
git cherry-pick -n commit-id

# 继续 cherry-pick
git cherry-pick --continue

# 中止 cherry-pick
git cherry-pick --abort
```

#### **使用场景**
```bash
# 场景：在 feature 分支修复了一个 bug，也需要应用到 main

# 1. 在 feature 分支找到修复的提交
git log --oneline

# 2. 切换到 main
git checkout main

# 3. 挑选该提交
git cherry-pick abc123
```

### 10.3 Stash - 储藏

#### **什么是 stash**
暂时保存工作区和暂存区的修改，让工作区变干净

#### **基本用法**
```bash
# 储藏当前修改
git stash

# 储藏并添加说明
git stash save "修改说明"

# 查看储藏列表
git stash list

# 应用最新的储藏
git stash apply

# 应用并删除储藏
git stash pop

# 应用特定储藏
git stash apply stash@{2}

# 删除储藏
git stash drop stash@{0}

# 清空所有储藏
git stash clear

# 查看储藏的内容
git stash show -p stash@{0}
```

#### **高级用法**
```bash
# 储藏时包括未跟踪的文件
git stash -u

# 储藏时包括被忽略的文件
git stash -a

# 从储藏创建分支
git stash branch 新分支名 stash@{0}
```

#### **使用场景**
```bash
# 场景：正在开发功能，突然需要紧急修复 bug

# 1. 储藏当前工作
git stash save "功能开发到一半"

# 2. 切换到 main 修复 bug
git checkout main
# ...修复 bug...
git commit -am "修复紧急 bug"

# 3. 切换回 feature 分支
git checkout feature

# 4. 恢复之前的工作
git stash pop
```

### 10.4 实践练习

```bash
# === 练习1：Rebase ===

# 1. 创建测试分支
git checkout -b test-rebase
echo "line1" > rebase.txt
git add rebase.txt
git commit -m "提交1"
echo "line2" >> rebase.txt
git commit -am "提交2"

# 2. 切换到 main 添加提交
git checkout main
echo "main line" > main.txt
git add main.txt
git commit -m "main 提交"

# 3. rebase
git checkout test-rebase
git rebase main

# 4. 查看历史
git log --oneline --graph

# === 练习2：Stash ===

# 1. 修改文件但不提交
echo "临时修改" >> hello.txt

# 2. 储藏
git stash save "临时工作"

# 3. 查看状态（应该是干净的）
git status

# 4. 恢复储藏
git stash pop

# 5. 查看状态（修改回来了）
git status
```

---

## 第11课：Git 标签（Tags）

### 11.1 理解标签

标签是对某个提交的永久引用，通常用于标记发布版本。

```
commits: A---B---C---D---E
tags:        v1.0  v1.1   v2.0
```

### 11.2 创建标签

#### **轻量标签**
```bash
# 创建轻量标签
git tag v1.0.0

# 为特定提交创建标签
git tag v1.0.0 commit-id
```

#### **附注标签（推荐）**
```bash
# 创建附注标签
git tag -a v1.0.0 -m "Release version 1.0.0"

# 为特定提交创建附注标签
git tag -a v1.0.0 commit-id -m "Release version 1.0.0"
```

### 11.3 查看标签

```bash
# 列出所有标签
git tag

# 列出符合模式的标签
git tag -l "v1.*"

# 查看标签详细信息
git show v1.0.0
```

### 11.4 推送标签

```bash
# 推送单个标签
git push origin v1.0.0

# 推送所有标签
git push origin --tags

# 推送所有标签（包括轻量标签）
git push --follow-tags
```

### 11.5 删除标签

```bash
# 删除本地标签
git tag -d v1.0.0

# 删除远程标签
git push origin --delete v1.0.0
# 或
git push origin :refs/tags/v1.0.0
```

### 11.6 检出标签

```bash
# 查看标签对应的代码（分离头指针状态）
git checkout v1.0.0

# 基于标签创建分支
git checkout -b branch-name v1.0.0
```

### 11.7 语义化版本（Semantic Versioning）

```
v主版本号.次版本号.修订号

v1.0.0 → v1.0.1 → v1.1.0 → v2.0.0
   ↓        ↓        ↓        ↓
 初始版   修复bug   新功能   重大更新
```

### 11.8 实践练习

```bash
# 1. 创建附注标签
git tag -a v1.0.0 -m "首次正式发布"

# 2. 查看标签
git tag
git show v1.0.0

# 3. 推送标签到远程
git push origin v1.0.0

# 4. 继续开发并创建新版本
echo "新功能" > feature.txt
git add feature.txt
git commit -m "添加新功能"
git tag -a v1.1.0 -m "添加新功能"

# 5. 推送所有标签
git push origin --tags
```

---

## 第12课：Git 钩子（Hooks）

### 12.1 什么是 Git Hooks

Git Hooks 是在特定 Git 事件发生时自动运行的脚本。

```
.git/hooks/
├── pre-commit          # 提交前运行
├── commit-msg          # 提交信息验证
├── pre-push            # 推送前运行
├── post-commit         # 提交后运行
└── ...
```

### 12.2 常用钩子

#### **客户端钩子**
- `pre-commit` - 提交前检查（代码格式、测试）
- `prepare-commit-msg` - 准备提交信息
- `commit-msg` - 验证提交信息格式
- `post-commit` - 提交后通知
- `pre-push` - 推送前检查

#### **服务端钩子**
- `pre-receive` - 接收推送前验证
- `update` - 更新引用前验证
- `post-receive` - 接收推送后通知

### 12.3 创建钩子

#### **示例1：pre-commit - 检查代码格式**

```bash
# .git/hooks/pre-commit

#!/bin/bash

echo "运行代码检查..."

# 检查是否有 JavaScript 文件
JS_FILES=$(git diff --cached --name-only --diff-filter=ACM | grep '\.js$')

if [ -n "$JS_FILES" ]; then
    # 运行 ESLint
    npx eslint $JS_FILES
    if [ $? -ne 0 ]; then
        echo "ESLint 检查失败，请修复后再提交"
        exit 1
    fi
fi

echo "代码检查通过"
exit 0
```

#### **示例2：commit-msg - 验证提交信息**

```bash
# .git/hooks/commit-msg

#!/bin/bash

commit_msg=$(cat $1)

# 检查提交信息是否符合规范
if ! echo "$commit_msg" | grep -qE "^(feat|fix|docs|style|refactor|test|chore): .+"; then
    echo "错误：提交信息必须符合格式："
    echo "  feat: 新功能"
    echo "  fix: 修复bug"
    echo "  docs: 文档更新"
    echo "  style: 代码格式"
    echo "  refactor: 重构"
    echo "  test: 测试"
    echo "  chore: 构建/工具"
    exit 1
fi

exit 0
```

#### **示例3：pre-push - 推送前运行测试**

```bash
# .git/hooks/pre-push

#!/bin/bash

echo "运行测试..."

npm test

if [ $? -ne 0 ]; then
    echo "测试失败，禁止推送"
    exit 1
fi

echo "测试通过，允许推送"
exit 0
```

### 12.4 启用钩子

```bash
# 1. 进入 hooks 目录
cd .git/hooks

# 2. 创建钩子文件
touch pre-commit

# 3. 编辑钩子
nano pre-commit

# 4. 添加可执行权限
chmod +x pre-commit
```

### 12.5 Husky - 共享钩子

Husky 可以让钩子脚本被 Git 跟踪和共享。

#### **安装 Husky**
```bash
# 安装 Husky
npm install husky --save-dev

# 初始化
npx husky init
```

#### **配置钩子**
```bash
# 添加 pre-commit 钩子
npx husky add .husky/pre-commit "npm test"

# 添加 commit-msg 钩子
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit "$1"'
```

#### **package.json 配置**
```json
{
  "scripts": {
    "prepare": "husky install"
  },
  "devDependencies": {
    "husky": "^8.0.0"
  }
}
```

### 12.6 实践练习

```bash
# === 练习：创建简单的 pre-commit 钩子 ===

# 1. 进入 hooks 目录
cd .git/hooks

# 2. 创建 pre-commit
cat > pre-commit << 'EOF'
#!/bin/bash

echo "=== Pre-commit 检查 ==="

# 检查是否有 console.log
if git diff --cached | grep -E "console\.log"; then
    echo "❌ 错误：代码中包含 console.log"
    exit 1
fi

echo "✅ 检查通过"
exit 0
EOF

# 3. 添加执行权限
chmod +x pre-commit

# 4. 测试钩子
echo "console.log('test')" > test.js
git add test.js
git commit -m "测试提交"  # 应该失败

# 5. 修复并重新提交
echo "// 正常代码" > test.js
git add test.js
git commit -m "测试提交"  # 应该成功
```

---

## 第13课：Git 子模块（Submodules）

### 13.1 什么是子模块

子模块允许你在一个 Git 仓库中包含另一个 Git 仓库。

```
主项目/
├── .git/
├── .gitmodules
├── src/
└── libs/
    └── external-lib/  ← 子模块
        └── .git/
```

### 13.2 添加子模块

```bash
# 添加子模块
git submodule add https://github.com/用户/仓库.git 路径

# 例如：
git submodule add https://github.com/jquery/jquery.git libs/jquery

# 提交变更
git commit -m "添加 jQuery 子模块"
```

### 13.3 克隆含有子模块的项目

```bash
# 方法1：克隆后初始化子模块
git clone https://github.com/用户/项目.git
cd 项目
git submodule init
git submodule update

# 方法2：克隆时递归初始化（推荐）
git clone --recursive https://github.com/用户/项目.git

# 方法3：克隆后一步初始化
git clone https://github.com/用户/项目.git
git submodule update --init --recursive
```

### 13.4 更新子模块

```bash
# 更新所有子模块
git submodule update --remote

# 更新特定子模块
git submodule update --remote libs/jquery

# 进入子模块手动更新
cd libs/jquery
git pull origin main
cd ../..
git add libs/jquery
git commit -m "更新 jQuery 子模块"
```

### 13.5 删除子模块

```bash
# 1. 删除子模块条目
git submodule deinit -f libs/jquery

# 2. 删除 .git/modules 中的内容
rm -rf .git/modules/libs/jquery

# 3. 从工作区删除
git rm -f libs/jquery

# 4. 提交变更
git commit -m "删除 jQuery 子模块"
```

### 13.6 实践练习

```bash
# 1. 创建主项目
mkdir main-project
cd main-project
git init

# 2. 添加子模块
git submodule add https://github.com/twbs/bootstrap.git libs/bootstrap

# 3. 查看子模块
git submodule

# 4. 查看 .gitmodules
cat .gitmodules

# 5. 提交
git add .
git commit -m "添加 Bootstrap 子模块"
```

---

## 第14课：Git 最佳实践

### 14.1 提交规范

#### **提交信息格式**
```
<type>(<scope>): <subject>

<body>

<footer>
```

#### **类型（type）**
- `feat`: 新功能
- `fix`: 修复 bug
- `docs`: 文档更新
- `style`: 代码格式（不影响代码运行）
- `refactor`: 重构
- `test`: 测试相关
- `chore`: 构建过程或辅助工具变动

#### **示例**
```bash
feat(用户): 添加用户登录功能

实现了基于 JWT 的用户登录系统，包括：
- 登录接口
- Token 验证中间件
- 用户信息缓存

关闭 #123
```

### 14.2 分支命名规范

```bash
# 功能分支
feature/user-authentication
feature/shopping-cart

# 修复分支
fix/login-error
bugfix/cart-calculation

# 发布分支
release/v1.0.0
release/v2.1.0

# 热修复分支
hotfix/critical-security-issue
```

### 14.3 代码审查清单

- [ ] 代码符合项目规范
- [ ] 有适当的注释
- [ ] 有单元测试
- [ ] 测试全部通过
- [ ] 没有调试代码（console.log）
- [ ] 没有硬编码
- [ ] 错误处理完善
- [ ] 性能考虑合理

### 14.4 常见错误和解决方案

#### **错误1：误提交了敏感信息**
```bash
# 从历史中完全删除文件
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch 敏感文件" \
  --prune-empty --tag-name-filter cat -- --all

# 强制推送（危险！）
git push origin --force --all
```

#### **错误2：提交到了错误的分支**
```bash
# 将最后一次提交移到新分支
git branch new-branch
git reset --hard HEAD^
git checkout new-branch
```

#### **错误3：需要修改已推送的提交信息**
```bash
# 不要修改已推送的历史！
# 如果必须修改，需要团队协调

# 本地修改
git commit --amend -m "新的提交信息"

# 强制推送（需要团队同意）
git push --force
```

### 14.5 .gitignore 最佳实践

```bash
# .gitignore 示例

# 依赖
node_modules/
vendor/

# 构建产物
dist/
build/
*.min.js

# 环境变量
.env
.env.local

# IDE
.vscode/
.idea/
*.swp

# 操作系统
.DS_Store
Thumbs.db

# 日志
*.log
logs/

# 临时文件
*.tmp
*.cache
```

### 14.6 团队协作建议

1. **频繁提交，谨慎推送**
   ```bash
   # 本地频繁提交
   git commit -m "小步骤提交"

   # 推送前整理提交
   git rebase -i HEAD~5
   ```

2. **保持主分支干净**
   - main/master 始终保持可部署状态
   - 所有开发在功能分支进行
   - 通过 PR 合并，不直接推送到主分支

3. **定期同步上游**
   ```bash
   # 每天开始工作前
   git checkout main
   git pull origin main
   git checkout feature-branch
   git rebase main
   ```

4. **使用 .gitattributes**
   ```bash
   # .gitattributes
   # 统一换行符
   * text=auto
   *.js text eol=lf
   *.css text eol=lf
   ```

---

## 附录：Git 配置优化

### 全局配置

```bash
# 用户信息
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"

# 默认编辑器
git config --global core.editor "code --wait"  # VS Code
git config --global core.editor "vim"          # Vim

# 默认分支名
git config --global init.defaultBranch main

# 颜色输出
git config --global color.ui auto

# 自动纠正
git config --global help.autocorrect 1
```

### Git 别名

```bash
# 常用别名
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.lg "log --oneline --graph --all"
```

### 提高效率的配置

```bash
# 自动补全和分支显示（需要安装 git-completion）
# 查看当前配置
git config --list

# 显示配置文件位置
git config --list --show-origin
```

---

## 学习资源

### 官方资源
- [Pro Git 电子书](https://git-scm.com/book/zh/v2)
- [Git 官方文档](https://git-scm.com/docs)
- [GitHub 学习实验室](https://lab.github.com/)

### 在线练习
- [Learn Git Branching](https://learngitbranching.js.org/?locale=zh_CN)
- [Git 沉浸式教程](http://gitimmersion.com/)

### 可视化工具
- GitKraken
- SourceTree
- GitHub Desktop
- Git Graph (VS Code 扩展)

### 备忘单
- [GitHub Git 备忘单](https://education.github.com/git-cheat-sheet-education.pdf)

---

## 总结

你已经学习了 Git 的进阶内容：

✅ 远程仓库操作
✅ Pull Request 工作流
✅ Git Flow / GitHub Flow
✅ Rebase, Cherry-pick, Stash
✅ 标签管理
✅ Git Hooks
✅ 子模块
✅ 最佳实践

### 下一步建议

1. **多实践**：在真实项目中应用这些技能
2. **参与开源**：为开源项目贡献代码
3. **深入学习**：研究 Git 内部原理
4. **团队协作**：在团队中推广最佳实践

继续探索，成为 Git 高手！
