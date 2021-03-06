# Git

Git 的用法。

## Git 基本概念

![Git 基本概念](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015120901.png)

```
Workspace：  工作区
Index/Stage：暂存区
Repository： 仓库区（或本地仓库）
Remote：     远程仓库
```

## 配置

Git 的设置文件为 .gitconfig，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。

```bash
# 显示当前的 Git 配置
git config --list

# 编辑 Git 配置文件
git config -e --global

# 设置提交代码时的用户信息，用户名最好不要写汉字
git config --global user.name "your name"
git config --global user.email "email address"
```

## 新建代码仓库

```bash
# 在当前目录新建一个 Git 代码库
git init

# 新建一个目录，将其初始化为 Git 代码库
git init [project-name]

# 下载一个项目和它的整个代码历史
git clone [url]
```

## Git 常用操作

- git 快速上手

```bash
git init git-demo
cd git-demo
touch a
touch b
touch c

echo 11 >> a
echo 22 >> a

git add .

git commit -m "11"

echo 33 >> a
git commit -am "33"
```

- git 基本操作

```bash
# add & commit Mothed 1
git add .
git commit -m "message"

# add & commit Mothed 2
git commit -a -m "message"

# add & commit Mothed 3
git commit -am "message"

# Git 假定所有的改变都是针对同一件事情的，因此它把这些都放在了一个块里。你有如下几个选项：
#输入 y 来暂存该块
#输入 n 不暂存
#输入 e 手工编辑该块
#输入 d 退出或者转到下一个文件
#输入 s 来分割该块
git add -p <file name>

# 改变文件夹的名字：Name -> name
# 直接运行 git mv Name name，执行不成功，会搞成：Name ->name/Name
git mv Name tmp
git mv tmp name

# diff working directory with repos 
git diff

# diff staging area with repos
git diff --cached

# 拿 working directory 和 SHA 比较
git diff <SHA>

# 比较两次提交的差异 
git diff <SHA 1> <SHA 2>

# 查看某次提交变更统计数据
git diff --stat <SHA>
```

- git ssh 链接

```bash
# diff working directory with repos 
ssh-Keygen -t rsa -C "your email"

# 查看用户主目录的 .ssh/ 文件夹中创建的私钥文件（注意备份）
ls ~/.ssh
# 目录中两个文件：id_rsa （私钥）  和   id_rsa.pub (公钥)

# 打印公钥文件内容
cat ~/.ssh/id_rsa.pub
# 把文件内容复制到剪贴板中

# 在 github.com 的 Settings 中找 SSH and GPG keys, new SSH key

# 利用 SSH 协议来克隆仓库
git clone git@github.com:wangding/test

# 利用 SSH 协议来添加远程链接
git remote add origin git@github.com:wangding/test
```

- git branch 操作

```bash
# 删除分支 foo 分支，前提 foo 已经合并过
git branch foo -d

# 强制删除分支 foo
git branch foo -D

# 创建分支 foo
git branch foo

# 切换到分支 foo
git checkout foo

# 创建分支并同时切换到 foo，一步做到
git checkout -b foo

# 修改分支名字
git branch -m old_name new_name
git branch -M old_name new_name

# 列出远程分支
git branch -r

# 查看已经合并的分支
git branch --merged

# 查看没有合并的分支
git branch --no-merged

# 列出远程合并的分支
git branch -r --merged

# 取出远程的 foo 分支
git checkout -t origin/foo

# 删除远程分支 1
# <space> 是空格，<remote branch> 是远程分支的名字
# 把空内容推到远程的分支上，就是删除的意思
git push origin <space> :<remote branch>

# 删除远程分支 2
# 在 github 上可以直接删除分支
# 在本地 git pull 不能把远程分支的变化反应到本地
# git pull 和 git fetch 不会清除已经删除的远程分支
# git branch -r 还可以看到已经删除的远程分支
# sourceTree 的线图还可以看到已经删除的远程分支
# 可以执行下面的命令解决问题
git fetch -p

# 合并分支
git merge <branch name>

# 合并分支，拒绝 fast forward，产生合并 commit
git merge --no-ff
```

- git blame 逐行查看文档

```bash
# 逐行查看 <filename> 的历史
git blame <filename>

# 从第 100 行开始查看 10 行
git blame -L 100,10 <filename>
```

- git clean 砍掉 untracked 档案

```bash
# 列出打算要清除的档案
git clean -n

# 真正的删除
git clean -f

# 连 .gitignore 中忽略的档案也清除
git clean -x
```

- git tag 操作

```bash
# 给当前的 HEAD 指针处贴标签 foo
git tag foo

# 给任意的一个提交贴标签 foo
git tag foo HEAD~4

# 给当前的 HEAD 指针处贴标签 foo
git tag foo -m "message"

# 删除标签 foo
git tag -d foo

# 列出标签
git tag

# 将所有标签推送到远程仓库中
git push --tags

# 将具体某个标签推送到远程仓库中
git push origin v0.1

# 拉下来远程仓库的标签
git pull --tags

# 删除远程仓库的标签 foo
git push origin :refs/tags/foo
```

- git stash 操作

```bash
# 保存进度
git stash

# 弹出进度
git stash pop

# 查看 stash 列表
git stash list

# 删除所有进度
git stash clear
```

- git rebase 操作

```bash
# -i 是交互操作
git rebase -i <SHA>

# 把当前分支在 master 的位置接上
git rebase master
```

- Git 其他操作

```bash
# 查看某个文件的提交记录
git log <file name>

# 把 upstream 代表的远程仓库的 master 分支拽到本地
git pull upstream master

# 撤销上一个 commit，前提是没有 push 到远程仓库
git add <something>
git commit --amend -m "some comment"

# 弹出 vim 输入多行 message
git commit

# 查看 log 日志，并过滤需要的信息
git log --grep <filter word>
```

## 提升 GitHub 访问速度

- win10 系统

复制下面的域名解析数据到记事本，在[站长工具](http://tool.chinaz.com/dns)中，对所有域名测试最快的 IP 地址，即 TTL 值最小的 IP 地址，把这个 IP 地址贴到记事本中，换掉原来的 IP 地址。所有域名搞完一遍后。在 `C:/Windows/system32/drivers/etc/hosts` 找到 hosts 文件，在 hosts 文件最下面复制粘贴下面的内容：

```
140.82.113.4      github.com
185.199.109.153   assets-cdn.github.com
185.199.109.153   documentcloud.github.com
69.171.245.53     github.global.ssl.fastly.net
203.98.7.65       gist.github.com
185.199.108.154   help.github.com
54.251.140.56     nodeload.github.com
151.101.108.133   raw.github.com
52.205.36.92      status.github.com
151.101.229.194   github.global.ssl.fastly.net
151.101.108.133   avatars0.githubusercontent.com
151.101.108.133   avatars1.githubusercontent.com
```

然后立刻刷新系统，刷新方法是：cmd 打开控制台窗口，直接输入：ipconfig /flushdns

**注意：**
hosts 文件有权限限制不能编辑保存，先找到 notepad.exe 程序，鼠标右键用管理员身份运行。然后再打开 Hosts 文件，就可以保存了。

- CentOS 7 系统

终端命令行模式，输入 `sudo vi /etc/hosts`，打开 hosts 文件，粘贴上面的 IP 地址和域名的数据。

## 常见问题

- [git 存储凭证](http://www.cnblogs.com/volnet/p/git-credentials.html)
```bash
$ git config --global credential.helper wincred
```
用这一行命令搞定，参考网址：https://help.github.com/articles/caching-your-github-password-in-git/

- [git 撤销远程仓库的提交](http://www.cnblogs.com/chucklu/p/4661149.html)

- [Git 文件换行问题](http://www.cnblogs.com/flying_bat/archive/2013/09/16/3324769.html)

- 如何在 Github 的 pull request 中进行 code review
  - https://github.com/wangding/Sample/pull/1
  - https://github.com/wangding/seIDE/pull/6
  - https://github.com/wangding/seIDE/pull/11

- issue 过滤

  ![issue filter.png](http://upload-images.jianshu.io/upload_images/3058932-fbc953aaadb6cdf0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 参考资料

- [常用 Git 命令清单](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)
- [Git 使用规范流程](http://www.ruanyifeng.com/blog/2015/08/git-use-process.html)
- [Git 远程操作详解](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)
- [Git 工作流程](http://www.ruanyifeng.com/blog/2015/12/git-workflow.html)
- [Git 分支管理策略](http://www.ruanyifeng.com/blog/2012/07/git.html)
- [Git 使用规范](http://www.ruanyifeng.com/blog/2015/08/git-use-process.html)
- [Commit message 和 Change log 编写指南](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)
- [Git 命令参考手册（中文）](http://www.open-open.com/lib/view/open1401433488824.html)，网站不错，全是开源方面的资料，平时可以多看看，会有很多简单、有趣的东西
- [专为设计师写的 github 资料](http://www.ui.cn/detail/20957.html)，可以参考一下吧，职业方向还是有差异的
- [git-it 课程资料](http://jlord.us/git-it/index-zhtw.html)，闯关练习软件的教程文字
- [git 心法（张文细）](https://blog.alphacamp.co/2015/01/13/git/)，一个简单、明了的博客文章
- [张文细的所有 Git 资料](https://ihower.tw/git/)，带视频，有幻灯片，讲的很到位，可以模仿
- [git ready](http://gitready.com/)
- [git 命令图解](http://blog.csdn.net/yangwen123/article/details/9084007)，配图清楚，比较多，够啰嗦
- [猴子都能懂的 Git 入门](http://backlogtool.com/git-guide/cn/intro/intro1_1.html)，配图还算可以，有点儿卡通幼稚，寓教于乐
- [Learn Git](https://www.atlassian.com/git)，排版简洁、大方，配图时尚、漂亮，内容专业权威

## 资源

- 勋章：http://shields.io/
- 进度：https://github.com/fehmicansaglam/progressed.io
- Git 教学软件：https://onlywei.github.io/explain-git-with-d3/
