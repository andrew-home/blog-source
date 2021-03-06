---
layout: post
title: "使用git subtree管理本博客"
date: 2014-04-07 16:49:18 +0800
comments: true
categories:
- git
---

# 背景

以下是本博客源码的目录结构

```mark:14-15
blog-source
├── CHANGELOG.markdown
├── Gemfile
├── Gemfile.lock
├── README.markdown
├── README.md
├── Rakefile
├── _config.yml
├── _deploy
├── config.rb
├── config.ru
├── plugins
├── public
├── sass -> theme/sass
├── source -> theme/source
└── theme
```
theme 是另外一个repo，用于维护博客样式的变更，采用subtree的形式包含在blog-source这个repo里。  
<!--more-->
在第一次添加theme子目录时，用的命令如下：

```mark:3
语法：git remote add -f <子仓库名> <子仓库地址>
解释：其中-f意思是在添加远程仓库之后，立即执行fetch。
git remote add -f theme git@github.com:andrew-home/octopress-theme.git
```

```mark:3
语法：git subtree add --prefix=<子目录名> <子仓库名> <分支> --squash
解释：--squash意思是把subtree的改动合并成一次commit，这样就不用拉取子项目完整的历史记录。--prefix之后的=等号也可以用空格。
git subtree add --prefix=theme theme fortheme --squash
```

# 样式更新

由于文章目录是位于source之下，而source又属于主题，为了不让主题这个repo不含有文章目录，所以新建了个fortheme分支，这个分支的source目录下是不含有_posts目录的。

更新样式时，先切换到fortheme分支，修改完theme目录下代码之后，再切换到master分支（因为fortheme分支没有文章目录，看不到效果），在master分支下review完效果后，再切换回fortheme分支，git commit提交，然后用以下命令push提交到远程

```mark:2
语法：git subtree push --prefix=<子目录名> <远程分支名> 分支
git subtree push --prefix=theme theme master
```

最后切换回master分支，用以下命令合并修改、提交
```
git merge fortheme --squash
git push origin master
```

# octopress 代码更新

如果有人修改了octopress的代码，提交了pull request，比如以下这个：
https://github.com/imathis/octopress/pull/1485

此修改使得octopress 2.0(master)版本也能用上代码渲染时的linenos，start，mark选项。

使用这种pull request，是把它check out到本地的一个新分支，然后再merge到master实现。
首先将.git/config里的octobpress源取消注释
```
[remote "octopress"]
   url = git://github.com/imathis/octopress.git
   fetch = +refs/heads/*:refs/remotes/origin/*
```

然后fetch这个pr
```mark:2
语法：git fetch <远程仓库名> refs/pull/<pull request序号>/head:<本地分支名>
git fetch octopress refs/pull/1485/head:update
```
上述命令会在本地新建一个分支来保存pr的代码，然后就可以merge到master来应用pr了。
