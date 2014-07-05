---
layout: post
title: "在64位的linux上运行32位的程序"
date: 2014-05-11 22:49:25 +0800
comments: true
categories: 
- debian
- linux
---
1. 症状

    - 执行bin文件时提示：No such file or directory
    - ldd bin文件  的输出为： not a dynamic executable
    - file bin文件 的输出显示程序是32位

2. 解决

    debian上只要安装 ia32-libs这个包（apt-get install ia32-libs)就可以了。
