---
title: Git操作手册
top: false
cover: false
toc: true
mathjax: true
date: 2021-03-24 10:09:52
img:
password:
summary:
tags:
- Git
categories:
- 工具

---

> Git 常用操作示例

<!--more-->

### 添加远程仓库
```
git remote add origin git@github.com:michaelliao/learngit.git  
git push -u origin master  向master 分支推送
```

### 删除本地分支|远程分支
- 删除远程分支(推送一个空分支)
```
git push origin :issue_101 删除远程分支
```

### 远程分支管理
```
git branch -a # 查看所有本地分支和远程分支
git branch -r # 只查看远程分支
git remote prune origin # 发现很多在远程仓库已经删除的分支在本地依然可以看到,删除远程不存在的分支
git remote show origin # 查看remote地址，远程分支，还有本地分支与之相对应关系等信息
```

### 设置默认推送远程分支

```
# 将本地分支与远程分支进行关联
# 设置git push/pull 默认推送远程分支 
git branch --set-upstream-to=origin/<远端分支> <本地分支>  
```



### 删除远程不存在的分支
使用`git branch -a`命令可以查看所有本地分支和远程分支（`git branch -r`可以只查看远程分支）
发现很多在远程仓库已经删除的分支在本地依然可以看到。
```
$ git branch -a
  feature-account-resell
  feature-code-refactor
  feature-crontab
  feature-data-source
  feature-utf8
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/feature-account-resell
  remotes/origin/feature-add-faka
  remotes/origin/feature-code-refactor
  remotes/origin/feature-crontab
  remotes/origin/feature-data-source
  remotes/origin/feature-ias
  remotes/origin/feature-utf8
  remotes/origin/master
  remotes/origin/zengjinping
```
使用命令 `git remote show origin`，可以查看`remote`地址，远程分支，还有本地分支与之相对应关系等信息。
```
$ git remote show origin
* remote origin
  Fetch URL: git@wangqiao.threathunter.cn:alert/SaleCardScript.git
  Push  URL: git@wangqiao.threathunter.cn:alert/SaleCardScript.git
  HEAD branch: master
  Remote branches:
    feature-add-faka                           tracked
    feature-ias                                tracked
    master                                     tracked
    refs/remotes/origin/feature-account-resell stale (use 'git remote prune' to remove)
    refs/remotes/origin/feature-code-refactor  stale (use 'git remote prune' to remove)
    refs/remotes/origin/feature-crontab        stale (use 'git remote prune' to remove)
    refs/remotes/origin/feature-data-source    stale (use 'git remote prune' to remove)
    refs/remotes/origin/feature-utf8           stale (use 'git remote prune' to remove)
    zengjinping                                tracked
  Local branches configured for 'git pull':
    feature-account-resell merges with remote feature-account-resell
    feature-code-refactor  merges with remote feature-code-refactor
    feature-crontab        merges with remote feature-crontab
    feature-data-source    merges with remote feature-data-source
    feature-utf8           merges with remote feature-utf8
    master                 merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```
此时我们可以看到那些远程仓库已经不存在的分支，根据提示，使用 `git remote prune origin` 命令
```
$ git remote prune origin
Pruning origin
URL: git@wangqiao.threathunter.cn:alert/SaleCardScript.git
 * [pruned] origin/feature-account-resell
 * [pruned] origin/feature-code-refactor
 * [pruned] origin/feature-crontab
 * [pruned] origin/feature-data-source
 * [pruned] origin/feature-utf8
(venv) [user_00@test-env /data/server/SaleCardScript]$ 
(venv) [user_00@test-env /data/server/SaleCardScript]$ git branch -a
  feature-account-resell
  feature-code-refactor
  feature-crontab
  feature-data-source
  feature-utf8
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/feature-add-faka
  remotes/origin/feature-ias
  remotes/origin/master
  remotes/origin/zengjinping
```
这样就删除了那些远程仓库不存在的分支。


### 删除已提交的(需要被忽略)的文件
```
1.在终端输入;

git rm -r --cached 要删除的文件名,如:

git rm -r --cached .idea

--cached 只删远程仓库的文件,不会删除本地的

2.提交操作记录描述

git commit -m '删除XX文件'

3.推送到远程仓库

git push -u origin master

```


### 更新被拒绝，因为您当前分支的最新提交落后于其对应的远程分支
```
$ git push
To github.com:amuqiao/amuqiao.github.io.git
 ! [rejected]        hexo -> hexo (non-fast-forward)
error: 推送一些引用到 'github.com:amuqiao/amuqiao.github.io.git' 失败
提示：更新被拒绝，因为您当前分支的最新提交落后于其对应的远程分支。
提示：再次推送前，先与远程变更合并（如 'git pull ...'）。详见
提示：'git push --help' 中的 'Note about fast-forwards' 小节。
```

解决方案：需要先获取远端更新并与本地合并,再git push
```
git remote add origin https://github.com/miaoihan/weibo.git  
$git fetch origin    //获取远程更新
$git merge origin/master //把更新的内容合并到本地分支
```

### 将本地已有的项目完整的提交到github上
`~/NutstoreFiles/project/grafana-tutorial`项目为示例进行操作

在GitHub新建一个仓库

```
$ cd ~/NutstoreFiles/project/grafana-tutorial
# 本地仓库初始化
$ git init
# 把全部文件添加到暂存区
git add .
# 把暂存区的文件提交到本地仓库
git commit -m "初始化提交"

# 连接远程仓库并推送
# git remote add origin 远程仓库地址
# origin是给远程仓库取的一个别名，远程仓库地址是GitHub仓库地址。该命令作用是让本地仓库和远程仓库相连接。
git remote add origin git@github.com:amuqiao/grafana-tutorial.git

# 把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。由于远程库是空的，
# 我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，
# 还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令（直接用git pull或者git push）。
# 向master分支进行推送
git push -u origin master

# 强行推送，本地仓库代码强行覆盖远程仓库：
$ git push -f origin master
```
- 参考 [Git：将本地项目推送到GitHub](https://blog.csdn.net/qq_42780289/article/details/102512463)
- 


### git branch 结果不在当前页显示
某些git命令的输出内容不是直接显示在命令下面，而是会额外打开一个窗口来显示，按q才能退出该窗口
终端执行
```
git config --global core.pager ''
```