---
layout: post
title: "使用git subtree 管理博客主题"
date: 2014-02-15 16:32:31 +0800
comments: true
categories: 
- git
---

# 背景

以下是本博客源码的目录结构

```plain blog-source mark:14-15
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
theme 是另外一个repo，用于维护博客样式的变更，采用subtree的形式包含在blog-source这个repo里。在第一次添加theme子目录时，用的命令如下：

```plain mark:3
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

先切换到fortheme分支，修改完theme目录下代码之后，git commit提交，然后用以下命令push
```mark:3
语法：git subtree push --prefix=<子目录名> <远程分支名> 分支
git subtree push --prefix=theme theme 
```




