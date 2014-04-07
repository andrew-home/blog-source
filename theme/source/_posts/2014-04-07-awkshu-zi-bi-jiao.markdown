---
layout: post
title: "awk数字比较"
date: 2014-04-07 21:57:27 +0800
comments: true
categories: 
- awk
---
# 自动转换
awk中的变量是数字还是字符串会根据上下文转换，如：
```sh linenos:true
$vi test.sh 

#!/bin/bash

aS=$1
bS=$2

awk 'BEGIN {
    aA="'$aS'"
    bA="'$bS'"
    if (aA>bA) { print  aA+bA "bigger" }
}' 
```

在命令行输入```test.sh 4 100```,会显示：

    104  bigger

原因是计算四则运算时，awk将其转换为数字，但比较时转换为字符串比较了


# 解决方法

变量需要转换为数字使用时，手动加0：

```sh linenos:true
#!/bin/bash

aS=$1
bS=$2

awk 'BEGIN {
    aA="'$aS'"+0
    bA="'$bS'"+0
    if (aA>bA) { print  aA+bA "bigger" }
}' 
```

直接比较改为变量相减后的结果与0比较：

```sh linenos:true
#!/bin/bash

aS=$1
bS=$2

awk 'BEGIN {
    aA="'$aS'"
    bA="'$bS'"
    if (aA-bA>0) { print  aA+bA "bigger" }
}'
```
