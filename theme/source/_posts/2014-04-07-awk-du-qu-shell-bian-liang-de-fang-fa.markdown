---
layout: post
title: "awk 读取 shell 变量的方法"
date: 2014-04-07 22:02:41 +0800
comments: true
categories: 
- awk
---

1. 在awk中用"'$var'" 

        #!/bin/bash

        var=test
        awk 'BEGIN { print "'$var'" }' 


    如果var有空格、转义字符等特殊字符，最好在$var外再用一个双引号括住： ```"'"$var"'"```

2. awk -v awk中的变量名= shell中的变量名

        #!/bin/bash

        varS=test
        awk -v varA="$varS"  'BEGIN { print  varA}' 

 
