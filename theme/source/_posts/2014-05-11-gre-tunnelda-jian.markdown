---
layout: post
title: "gre tunnel搭建"
date: 2014-05-11 23:00:00 +0800
comments: true
categories: 
- debian
---
 客户端（client）与服务器A在同一个运营商网络，应用部署在服务器B，服务器A 、B之间建立tunnel，A设置dnat，client通过访问A的8000端口来访问服务器B，B返回的响应通过tunnel到达A，A再返回给client。在dnat之上使用tunnel的原因是通过tunnel后，B接收到的请求源IP为client，而不是A。

```

+----------+          +------------------------+                +------------------------+
|          |          |                        |     tunnel     |                        |
|  client  +--------> |       server A         | <------------> |       server B         |
|          |          |                        |                |                        |
|          |          +------------------------+                +------------------------+
|          |          |                        |                |                        |
+----------+          | ip:192.168.1.1         |                | ip:192.168.2.2         |
                      |                        |                |                        |
                      | tunnel_ip:10.111.111.1 |                | tunnel_ip:10.111.111.2 |
                      |                        |                |                        |
                      +------------------------+                +------------------------+
 
``` 

## 在A(linux) 搭建 gre tunnel过程

1. 创建gre tunnel： 

        /sbin/modprobe ip_gre
        /sbin/iptunnel add tun0 mode gre remote 192.168.2.2 local 192.168.1.1
        /sbin/ifconfig tun0 10.111.111.1
        /sbin/route add 10.111.111.2 tun0

2. 设置iptables规则：

        #这个规则很重要，某些特殊的网络会影响mtu的协商，所以要用这个规则来指写mtu
        /sbin/iptables -t mangle -A FORWARD -p tcp -m tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu

        #到apache端口的映射
        /sbin/iptables -t nat -A PREROUTING -i eth0 -p tcp -m tcp --dport 8000 -j DNAT --to-destination 10.111.111.2:8000 

        #SNAT规则
        /sbin/iptables -t nat -A POSTROUTING -o eth0 -p tcp -j SNAT --to-source 192.168.1.1 

## 当B是bsd时，在B搭建 gre tunnel过程：

1. BSD 6.2的默认内核不支持ipfw的forward功能，需要重编一下kernel。找一个默认的SMP配置文件，在最后加上 

        options IPFIREWALL
        options IPDIVERT
        options IPFIREWALL_DEFAULT_TO_ACCEPT
        options IPFIREWALL_VERBOSE
        options IPFIREWALL_VERBOSE_LIMIT=10
        options IPFIREWALL_FORWARD
        options DUMMYNET

    编译内核重启（或者直接找一个编译好的来用）
 

2. 做一个到napt服务器的tunnel。在BSD 4.x下，ipip tunnel工作良好，但在6.2下会有一些古怪的问题，小包通讯正常，大包就无法通过，因此要改用gre tunnel。通过下面的命令来创建tunnel，同时可以把这些命令写到/etc/rc.local，服务器启动的时候自动创建： 

        /sbin/ifconfig gre0 create
        /sbin/ifconfig gre0 tunnel 192.168.2.2 192.168.1.1
        /sbin/ifconfig gre0 10.111.111.2/32 10.111.111.1

3. 做转发规则。实现类似linux下ip route的功能，将从10.111.111.2发出的包，都转发给tunnel另一端的napt服务器。 

        /sbin/ipfw add 100 fwd 10.111.111.1 ip from 10.111.111.2 to any


    也可以使用ipf来实现源地址路由的功能（不用重新编译内核）

    1. 创建tunnel跟上面的第2步一样

    2. 在ipf的rule文件添加规则： 
            
            pass out quick on bge0 to gre0:10.111.111.2 from 10.111.111.1 to any
        
        然后```/etc/rc.d/ipfilter restart```即可


## 当B是linux时，在B搭建 gre tunnel过程：

1. 装一个iproute的包，修改/etc/iproute2/rt_tables，添加自定义的table

        201 mytunnel
     
2. 做一条tunnel，步骤不再重复，假设得到tun0

3. 添加ip route规则 

        #设置mytunnel表的默认路由，都指到tun0 
        /sbin/ip route add default dev tun0 table mytunnel mtu 1400
        
        #添加一个规则，让所有从10.111.111.2发出的包都到mytunnel表里面去找路由
        /sbin/ip rule add from 10.111.111.2 table mytunnel

 

 

 
