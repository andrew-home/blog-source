---
layout: post
title: "ssh登陆失败"
date: 2014-05-11 22:56:26 +0800
comments: true
categories: 
- debian
---
用ssh key登陆不上某台机A的某个账号xy1，查看A的/var/log/messages，看到有这么句：
```
User xy1 not allowed because account is locked 
```
原因是该账号没设密码，被锁住了，用以下命令解决：
```
passwd -u xy1
usermod -p -u xy1 
```
