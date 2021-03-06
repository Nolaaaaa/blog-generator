---
title: 用命令行Git在本地创建一个库并上传到Github
date: 2018-04-4 19:08:02
tags: [Git]
categories: 前端
---

如何在本地创建一个仓库并上传到github？如何获取一个SSH key？
<escape><!-- more --></escape>

### 1  如何获取一个SSH key
1. 为什么要获取SSH key？—因为利用SSH key可以访问你的所有的仓库。
2. 一台电脑需要几个SSH key？—每台电脑只需要一个。
3. 怎么获取SSH key？—可参照如下步骤：
	* 登录GitHub 
	* 点击页面右上角的头像 
	* 选择Setting 
	* 选择SSH and GPG keys 
	* 点击generating SSH keys 
	* 点击Generating a new SSH key and adding it to the ssh-agent
	* 复制Generating a new SSH key的第一条黑色的命令`ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`到GitBash（或终端） 
	* 回车三次后得到一个类似泡泡的东西就说明成功了 
	* 接着输入`cat ~/.ssh/id_rsa.pub`，得到一大串英文，将这段英文复制 
	* 回到第4步的页面中，点击右上角的绿色按钮”New SSH key”
	* 将刚刚复制的东西放到Key下面的文本框，随便编辑一个Title，点击下面的绿色按钮确认添加 
	* 回到Git Bash（终端），运行`ssh -T git@github.com`测试是否成功，得到一个提示让你回复yes/no，输入yes回车 
	* 如果得到”Permission denied（publickey）”，很遗憾，你失败了，需要从第一步开始重新；如果得到的语句里有”You‘ve successfully authenticated”，那么恭喜，你成功了
4. Tips：`ls -al ~/.shh`检查本地是否已生成过shh key，如果你已有ssh key，需要重新添加，可在进行以上步骤前在Git Bash（终端）运行`rm -rf ~/.ssh/*`将现有的ssh key都删掉。
5. 已踩的坑： 
	* 在”ssh -T git@github.com”时，遇到如左括号里的代码，一般情况下，输入”ping github.com”即可解决。(错误提示：ssh: Could not resolve hostname [github.com](http://github.com): nodename nor servname provided, or not known） 
	
	* 输入”git remote add origin git@github.com:Nolaaaaa/yyy.git”时遇到如左括号里的代码，输入”git remote rm origin”后再重新按步骤输”git remote add origin git@github.com:Nolaaaaa/yyy.git””git push -u origin master”即可。（错误提示：fatal: remote origin already exists.）

### 2  如何在本地创建一个仓库并上传到github？
```js
$ mkdir blog       //在桌面上创建一个叫"blog"的目录 
$ cd blog      //"cd blog"进入目录 
$ git init       //"git init"即在目录"blog"中创建一个仓库(使用"ls -la"可查看)
// Initialized empty Git repository in /Users/nola/Desktop/blog/.git/
$ touch index.html       //"touch index.html"即在目录"blog"中创建一个叫"index.html"的文件
$ git status -sb      // "git status -sb"用于查看文件的变动，如下"？？"表示存在变动，在问你如何处理变动
// ?? index.html
$ git add index.html     //"git add index.html"把变动即新加的"index.html"文件添加到暂存区
$ git status -sb       //"git status -sb"再次查看文件的变动，绿色的"A"表示添加新加的文件到仓库
// A  index.html
$ git commit -m "我的第一次提交"     //"git commit -m"即正式将暂存区的文件提交到本地仓库，即第三步建立的".git"仓库中
// \[master (root-commit) be29eb7\] 我的第一次提交
// 1 file changed, 0 insertions(+), 0 deletions(-)
// create mode 100644 index.html  
$ git pull      //下载github的更新到本地  
$ git push      //上传到github
```

ps：
如果add错想撤销add的内容，可使用"git reset HEAD 文件名"；
如果add错又commit了,可使用"reset --hard HEAD^"；

### 3 GIT工作流
```javascript
// 其中 branchName 是自己本地的分支名字，originBranchName 是远程的分支名字（不能设置为 master/dev ，会覆盖掉远程的代码）
$ git fetch --prune    // git fetch 相当于是从远程获取最新到本地，并删除远程已经不存在的分支，不会自动merge
$ git checkout -b [branchName] origin/dev  // 基于远程的dev分支，创建一个本地分支
// 进行开发...
$ git add . 
$ git commit -m "update"
// 开发完成...
$ git fetch --prune  // 再次获取远程最新的代码
$ git merge origin/dev  // 把本地的代码和远程的dev分支代码进行合并
$ git commit -m "解决冲突"   //如果本地和远程都修改过相同代码在这一步需要先解决冲
$ git push origin [branchName]:[originBranchName]  // 把本地的 branchName 推送到远程的 originBranchName 分支上
// 最后一步，在码云上发起Pull Requeste请求，源分支是 originBranchName，目标分支是 dev
// Pull Requeste 可以把多个提交点压缩成一个提交点，如果直接使用 git push 之前需要使用 git rebase 能把多个提交点压缩成一个
```

### 4  远程仓库名称改变后修改本地的对应仓库名
```javascript
$ git remote -v    //列出远程仓库信息
//方法一
$ git remote set-url origin git@github.com:username/reponame.git  //修改远程仓库对应网址
//方法二
$ git remote rm origin    //本地仓库删除远程仓库
$ git remote add origin git@github.com:username/reponame.git   //添加新的远程仓库
```

### 5 命令行统计自己的代码行数
```bash
$ git log --author="yourname" --pretty=tformat: --numstat | awk '{ add += $1; subs += $2; loc += $1 - $2 } END { printf "added lines: %s, removed lines: %s, total lines: %s\n", add, subs, loc }' -
```


### 6 修改git 用户名和邮箱
```js
//修改当前project：
$ git config user.name '用户名'
$ git config user.email '邮箱名'

修改全局：
$ git config  --global user.name '用户名'
$ git config  --global user.email '邮箱名'

// 打开.gitconfig文件：
vi ~/.gitconfig
```

### 7 撤销 commit
```js
git reset --soft HEAD^
```

### 8 解决github文件夹灰色不能点开的问题
```js
// 本地库中，删掉灰色文件夹，保存在别的地方，提交本地文件：
$ git add .
$ git commit -m 'update'
$ git push 
// 把文件夹重新添加进去并重新提交本地文件：
$ git add *
$ git commit -m 'update'
$ git push 
```

### 9 git add . 和 git add * 的区别：
```js
$ git add *  git会自动把你当前目录所有修改过的文件添加
$ git add .  git会递归地将你执行命令时所在的目录中的所有文件添加上去
```

### 检查已有的配置信息
```js
$ git config --list
```

### 给git配置别名
```js
// 给 commit 起一个别名 ci
$ git config --global alias.ci commit
```