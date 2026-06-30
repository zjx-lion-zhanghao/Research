# GitHub 仓库协作与同步教程

> 适用场景：你有两个 GitHub 账号，或者你和同学/队友分别使用不同 GitHub 账号。  
> 例如：A 克隆了 B 的仓库，现在 A 想把自己的修改同步到 GitHub 上。

---

## 1. 先理解三个核心概念

在 GitHub 协作里，不要先纠结“是谁克隆的”，而要先判断：

> A 有没有 B 仓库的写入权限？

根据这个问题，协作方式分成两种。

| 场景 | 推荐方式 | 结果 |
|---|---|---|
| A 有 B 仓库写入权限 | 直接 push 到 B 仓库 | A 可以直接把修改同步到 B 的仓库 |
| A 没有 B 仓库写入权限 | Fork + Pull Request | A 先推到自己的仓库，再请求 B 合并 |

---

## 2. 方案一：B 把 A 添加为协作者，A 直接 push

这是最适合同学合作、课程项目、小组项目的方式。

### 2.1 B 在 GitHub 上添加 A 为 Collaborator

用 B 账号登录 GitHub，进入 B 的仓库页面。

操作路径：

```text
B 的仓库页面
→ Settings
→ Collaborators 或 Collaborators and teams
→ Add people
→ 输入 A 的 GitHub 用户名或邮箱
→ 发送邀请
```

注意：

- 必须由仓库所有者 B 操作。
- A 必须接受邀请。
- A 接受邀请后，才有权限 push 到 B 的仓库。
- 对于个人账号仓库，Collaborator 通常可以 pull 代码，也可以 push 修改。

---

### 2.2 A 接受 GitHub 邀请

A 登录自己的 GitHub 账号。

通常有两种入口：

```text
GitHub 网页右上角通知
```

或者：

```text
A 账号绑定邮箱里的邀请邮件
```

点击接受邀请。

如果 A 没有接受邀请，本地执行 `git push` 仍然可能失败。

---

### 2.3 A 检查本地仓库远程地址

A 在本地项目文件夹打开终端：

```bash
cd 你的项目路径
git remote -v
```

你应该看到类似：

```bash
origin  https://github.com/B用户名/仓库名.git
```

这说明本地项目现在指向 B 的仓库。

如果不是 B 的仓库，可以改成 B 的仓库地址：

```bash
git remote set-url origin https://github.com/B用户名/仓库名.git
```

再检查一次：

```bash
git remote -v
```

---

### 2.4 A 提交本地修改

先查看当前修改状态：

```bash
git status
```

添加所有修改：

```bash
git add .
```

提交修改：

```bash
git commit -m "更新项目内容"
```

如果 Git 提示没有可提交内容，说明当前没有新的修改，或者之前已经提交过。

---

### 2.5 A 推送到 B 的仓库

先查看当前分支名：

```bash
git branch --show-current
```

如果显示是 `main`：

```bash
git push origin main
```

如果显示是 `master`：

```bash
git push origin master
```

如果显示是其他分支，比如 `dev`：

```bash
git push origin dev
```

---

## 3. 方案二：A 没有 B 仓库权限，使用 Fork + Pull Request

如果 A 没有 B 仓库权限，不要直接 push 到 B 的仓库。  
正确做法是：

```text
A fork B 的仓库
→ A 把修改 push 到自己的 fork 仓库
→ A 给 B 提 Pull Request
→ B 审查后合并
```

---

### 3.1 A fork B 的仓库

A 登录 GitHub，打开 B 的仓库页面。

点击右上角：

```text
Fork
```

Fork 之后，A 自己账号下会出现一个仓库：

```text
https://github.com/A用户名/仓库名
```

这个仓库是 A 自己的，所以 A 可以自由 push。

---

### 3.2 本地设置 origin 和 upstream

如果 A 之前是直接克隆的 B 仓库，本地的 `origin` 很可能指向 B：

```bash
git remote -v
```

可能显示：

```bash
origin  https://github.com/B用户名/仓库名.git
```

推荐改成下面这种结构：

```text
origin   = A 自己的 fork 仓库，用来 push
upstream = B 的原仓库，用来同步 B 的更新
```

执行：

```bash
git remote rename origin upstream
git remote add origin https://github.com/A用户名/仓库名.git
```

检查：

```bash
git remote -v
```

应该看到类似：

```bash
origin    https://github.com/A用户名/仓库名.git
upstream  https://github.com/B用户名/仓库名.git
```

---

### 3.3 A 提交并推送自己的修改

```bash
git status
git add .
git commit -m "更新项目内容"
```

查看分支名：

```bash
git branch --show-current
```

如果是 `main`：

```bash
git push -u origin main
```

如果是 `master`：

```bash
git push -u origin master
```

---

### 3.4 A 给 B 提 Pull Request

打开 A 的 fork 仓库页面。

通常 GitHub 会显示：

```text
Compare & pull request
```

点击后确认：

```text
base repository: B用户名/仓库名
base branch: main

head repository: A用户名/仓库名
compare branch: main
```

然后填写标题和说明，提交 Pull Request。

B 看到后，可以审查代码并合并。

---

## 4. A 同步 B 仓库的最新代码

如果 B 的仓库又更新了，A 想把 B 的最新内容同步到自己本地，可以执行：

```bash
git fetch upstream
git pull --rebase upstream main
```

然后再推送到 A 自己的 fork：

```bash
git push origin main
```

如果你的主分支叫 `master`，把上面的 `main` 换成 `master`。

---

## 5. 设置本地提交身份

Git 的提交身份和 GitHub 登录账号不是同一个概念。

提交身份影响的是 commit 记录里显示的作者：

```bash
git config user.name "A的GitHub用户名"
git config user.email "A绑定的邮箱"
```

只对当前仓库生效：

```bash
git config user.name "A的GitHub用户名"
git config user.email "A绑定的邮箱"
```

对当前电脑全局生效：

```bash
git config --global user.name "A的GitHub用户名"
git config --global user.email "A绑定的邮箱"
```

查看当前配置：

```bash
git config user.name
git config user.email
```

查看全局配置：

```bash
git config --global user.name
git config --global user.email
```

---

## 6. GitHub 登录凭据问题

有时候你明明想用 A 账号 push，但电脑却一直使用 B 账号的缓存登录信息。

这种情况下，即使你设置了：

```bash
git config user.name
git config user.email
```

也不一定有用。

因为：

```text
git config 控制的是提交记录作者
GitHub 凭据控制的是谁在登录、谁在 push
```

---

### 6.1 macOS 清除 GitHub 凭据

打开：

```text
钥匙串访问
```

搜索：

```text
github.com
```

删除旧的 GitHub 凭据。

然后重新执行：

```bash
git push origin main
```

重新登录 A 账号。

---

### 6.2 Windows 清除 GitHub 凭据

打开：

```text
控制面板
→ 凭据管理器
→ Windows 凭据
```

找到 `github.com` 相关凭据并删除。

然后重新执行：

```bash
git push origin main
```

重新登录 A 账号。

---

## 7. HTTPS 推送时不能直接输入 GitHub 密码

现在使用 HTTPS 推送 GitHub 仓库时，如果命令行提示输入密码，通常不能输入 GitHub 登录密码。

应该使用：

```text
Personal Access Token
```

也就是 GitHub 个人访问令牌。

登录时通常是：

```text
Username: A的GitHub用户名
Password: A账号生成的 Personal Access Token
```

更推荐使用：

```text
GitHub CLI
```

或者：

```text
Git Credential Manager
```

来管理登录状态。

---

## 8. 常见报错与解决方法

### 8.1 报错：Permission denied

可能原因：

```text
A 没有 B 仓库权限
A 没有接受 B 的协作者邀请
当前电脑登录的不是 A 账号
SSH key 或 HTTPS token 不属于 A 账号
```

解决：

```bash
git remote -v
```

确认远程仓库地址。

然后确认：

```text
B 是否已经添加 A 为 Collaborator
A 是否已经接受邀请
当前 GitHub 凭据是否是 A 账号
```

---

### 8.2 报错：Repository not found

可能原因：

```text
仓库地址写错
仓库是私有仓库，但当前账号没有权限
当前电脑缓存了错误账号
```

解决：

```bash
git remote -v
```

如果地址错了：

```bash
git remote set-url origin https://github.com/B用户名/仓库名.git
```

如果是账号错了，清除本地 GitHub 凭据后重新登录。

---

### 8.3 报错：Updates were rejected

完整报错可能类似：

```text
Updates were rejected because the remote contains work that you do not have locally.
```

意思是远程仓库有新内容，你本地没有同步。

解决：

```bash
git pull --rebase origin main
git push origin main
```

如果你的分支是 `master`：

```bash
git pull --rebase origin master
git push origin master
```

---

### 8.4 报错：nothing to commit

说明当前没有新的修改需要提交。

可以执行：

```bash
git status
```

如果显示：

```text
working tree clean
```

说明当前工作区是干净的。

---

## 9. 推荐协作规范

如果是课程项目、小组作业或实训项目，推荐这样约定：

### 9.1 分支规范

```text
main      稳定版本
dev       开发版本
feature   单个功能开发分支
```

例如：

```bash
git checkout -b feature/login-page
```

开发完成后：

```bash
git add .
git commit -m "完成登录页面"
git push origin feature/login-page
```

然后再合并到 `dev` 或 `main`。

---

### 9.2 commit 信息规范

不要写：

```text
修改
更新
111
fix
```

推荐写清楚：

```text
完成登录页面结构
修复首页样式错位问题
新增用户注册功能
更新 README 使用说明
```

---

### 9.3 push 前先同步

每次 push 前，建议先执行：

```bash
git pull --rebase origin main
```

然后再：

```bash
git push origin main
```

这样可以减少冲突。

---

## 10. 最常用命令速查

### 查看状态

```bash
git status
```

### 添加修改

```bash
git add .
```

### 提交修改

```bash
git commit -m "更新项目内容"
```

### 查看当前分支

```bash
git branch --show-current
```

### 查看远程仓库

```bash
git remote -v
```

### 修改远程仓库地址

```bash
git remote set-url origin https://github.com/用户名/仓库名.git
```

### 推送 main 分支

```bash
git push origin main
```

### 推送 master 分支

```bash
git push origin master
```

### 拉取远程最新代码

```bash
git pull --rebase origin main
```

### Fork 模式同步上游仓库

```bash
git fetch upstream
git pull --rebase upstream main
git push origin main
```

---

## 11. 最终推荐流程

### 如果 A 有 B 仓库权限

```text
B 添加 A 为 Collaborator
A 接受邀请
A 本地提交修改
A 直接 push 到 B 仓库
```

命令：

```bash
git status
git add .
git commit -m "更新项目内容"
git push origin main
```

---

### 如果 A 没有 B 仓库权限

```text
A fork B 的仓库
A push 到自己的 fork
A 给 B 提 Pull Request
B 审查并合并
```

命令：

```bash
git remote rename origin upstream
git remote add origin https://github.com/A用户名/仓库名.git

git add .
git commit -m "更新项目内容"
git push -u origin main
```

---

## 12. 一句话总结

GitHub 协作的核心判断是：

> A 有没有 B 仓库的写入权限？

如果有：

```text
直接 push
```

如果没有：

```text
Fork + Pull Request
```

最推荐的小组项目方式是：

```text
B 把 A 添加为 Collaborator，A 接受邀请后直接 push。
```

---

## 参考资料

- GitHub Docs：Inviting collaborators to a personal repository  
  https://docs.github.com/articles/inviting-collaborators-to-a-personal-repository

- GitHub Docs：Permission levels for a personal account repository  
  https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/repository-access-and-collaboration/permission-levels-for-a-personal-account-repository

- GitHub Docs：Fork a repository  
  https://docs.github.com/articles/fork-a-repo

- GitHub Docs：About pull requests  
  https://docs.github.com/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests

- GitHub Docs：Managing your personal access tokens  
  https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens

- GitHub Docs：About authentication to GitHub  
  https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/about-authentication-to-github
