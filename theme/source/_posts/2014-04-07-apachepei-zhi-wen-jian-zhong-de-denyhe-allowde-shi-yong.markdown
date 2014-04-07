---
layout: post
title: "Apache配置文件中的deny和allow的使用"
date: 2014-04-07 15:27:05 +0800
comments: true
categories: apache
---
Order allow,deny # 这句话的作用是配置allow和deny的顺序，最后一个关键字代表默认策略，第一个圆  
Allow from all # allow规则，第二个圆  
deny from 192.9.200.69 # deny规则，第三个圆  
<!--more--> 
我们来看下下面的apache的一个配置，具体代码如下：
```
<Directory "D:/TRS/Apache2.2.17/cgi-bin">
    Order allow,deny  #1
    Allow from all #2
    deny from 192.9.200.69 #3
</Directory>
```

具体规律如下:

- 规律

当我们看到一个apache的配置时，可以从下面的角度来理解。一默认，二顺序，三重叠。

- 上面配置说明

    1. 一默认

        Order allow,deny ，这句话的作用是配置allow和deny的顺序，默认只有最后一个关键字起作用，这里起作用的关键字就是“deny”，默认拒绝所有请求。为了便于理解，我们可以画一个圆，圆的背景色涂上黑色，我们给这个圆起个编号，叫圆1。

    2. 二顺序

        由于上边的Order指出判断的顺序是先判断allow的规则，然后才是deny的规则。所以我们要先判断allow的请求，由于该请求中配置的是allow from all，
        所以表示该请求允许所有请求。这时我们再画一个圆，背景色涂上白色，我们给圆起个编号，叫圆2。

        我们再来看deny的判断规则，由于 deny from 192.9.200.69 ，表示拒绝来自ip地址为“192.9.200.69”，所以我们可以画出一块红色区域，表示“192.9.200.69”，我们把这块区域叫区域3。

        注意:即使把“Allow from all”写在“deny from 192.9.200.69”下面，依然是需要先判断allow规则，也就是说只有Order才能决定allow和order的优先顺序。

    3. 三重叠

        我们把上边产生的圆1、圆2和区域3依次从下往上堆叠在一起。每个层都是不透明的，这时我们可以看到最终效果是除了“192.9.200.69”这块红色区域外，其他的所有都是白色区域。也就是只有“192.9.200.69”这个ip地址没有权限访问该目录，其他的请求都有权限访问该目录。

# 看看下面的例子

也许上边没有说明白，我们再来看下面的例子，每个配置后面都有简单的说明，配置文件中的“#”号后边的数字表示配置项起作用的先后顺序。

1. 只允许192.9.200.69请求访问目录
 
        <Directory "D:/TRS/Apache2.2.17/cgi-bin">
        Order deny,allow #1.默认允许全部请求
        Allow from 192.9.200.69 #3.重叠，允许IP192.9.200.69的请求
        deny from all #2.按照顺序，先判断deny规则，拒绝所有请求
        </Directory>
 
2. 允许所有请求访问目录 

        <Directory "D:/TRS/Apache2.2.17/cgi-bin">
        Order deny,allow #1.默认允许全部请求
        Allow from all #3.重叠，允许所有请求
        deny from 192.9.200.69 #2.按照顺序，先判断deny规则，拒绝192.9.200.69的请求
        </Directory>

  
3. 拒绝所有请求访问目录 

        <Directory "D:/TRS/Apache2.2.17/cgi-bin">
        Order allow,deny #1.默认拒绝全部请求
        Allow from 192.9.200.69 #2.顺序，允许 192.9.200.69请求
        deny from  all#3.重叠，拒绝所有请求
        </Directory>

 
4. 除了192.9.200.69的请求外，其他请求都可以访问目录

        <Directory "D:/TRS/Apache2.2.17/cgi-bin">
        Order allow,deny #1.默认拒绝全部请求
        deny from  192.9.200.69#3.重叠，拒绝192.9.200.69请求
        Allow from all #2.顺序，允许所有请求
        </Directory>
 
转自：http://blog.csdn.net/wgw335363240/article/details/6362418
