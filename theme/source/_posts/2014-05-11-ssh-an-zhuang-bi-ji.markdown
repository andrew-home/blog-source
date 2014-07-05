---
layout: post
title: "ssh 安装笔记"
date: 2014-05-11 22:52:32 +0800
comments: true
categories: 
- debian
---
debian 6.0 的一台32位机器，使用```aptitude search openssh-server-x509```没结果（其他机同样源配置是有结果的），于是上内部源下载openssh-server-x509.deb，安装过程如下：


- 彻底删除之前安装失败的包及残留配置文件(用aptitude remove --purge不够彻底，安装时会报错）

        dpkg -P openssh-server-x509

- 检查是否删除干净

        dpkg -s openssh-server-x509

- 安装新包

        dpkg -i openssh-server-x509.deb 

 
