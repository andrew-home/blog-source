---
layout: post
title: "awk 修改 shell 变量方法"
date: 2014-04-07 21:09:55 +0800
comments: true
categories: 
- awk
---
1. 使用文件重定向：
        
        #!/bin/bash 

        var=1
        awk 'BEGIN {print 2>"tmp"}'
        var=$( cat tmp ) 

2. 使用eval

        #!/bin/bash

        eval $(awk 'BEGIN {print "var=2"}')
        echo $var
