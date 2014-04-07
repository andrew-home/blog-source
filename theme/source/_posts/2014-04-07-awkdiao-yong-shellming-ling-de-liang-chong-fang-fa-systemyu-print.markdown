---
layout: post
title: "awk调用shell命令的两种方法system与print"
date: 2014-04-07 22:07:27 +0800
comments: true
categories: 
- awk
---
awk获取执行shell命令后的结果：
``` 
awk 'BEGIN{
　　i=1;while(i<=5){
　　　　system("date > date.tmp")
　　　　getline < "date.tmp"
　　　　print $1
　　　　system("sleep 2")
　　　　close("date.tmp")
　　　　i++
　　}
}'
```
或者
```
awk 'BEGIN{
　　i=1;while(i<=5){
　　　　system("date > date.tmp")
　　　　getline a< "date.tmp"
　　　　print a
　　　　system("sleep 2")
　　　　close("date.tmp")
　　　　i++
　　}
}'
```
或者
```
awk 'BEGIN{
　　i=1;while(i<=5){
　　　　"date" | getline
　　　　print $1
　　　　system("sleep 2")
　　　　close("date")
　　　　i++
　　}
}'
```
注意：close("date.tmp")或close("date")一句必不可少，否则每次循环从管道拿到的都是已经打开的文件或命令的数据
