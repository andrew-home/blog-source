---
layout: post
title: "debian下mysql主从配置"
date: 2014-05-11 22:44:26 +0800
comments: true
categories: 
- debian
- mysql
---
1. 确保master/slave只有一份/etc/mysql/my.cnf , 不要在其他地方再有my.cnf （如/etc/my.cnf  /usr/local之类）

2. 配置文件

    master配置：

    在[mysqld]节点下添加：

        server-id=1
 
    slave配置：

    在[mysqld]节点下添加： 

        server-id=2
        relay-log = /home/mysql/logs/mysqld-relay-bin
        relay-log-index = /home/mysql/logs/mysqld-relay-bin.index
        replicate-ignore-db=mysql
        replicate-ignore-db=test

3. 先后重启master、slave的mysql： /etc/init.d/mysql restart

4. 在master上用root登陆mysql: mysql -uroot -p

5. master上锁表：

        mysql> flush tables with read lock;

6. show master status; 记录binlog的名字和位置

7. slave是登陆mysql，然后：

        mysql> slave stop;
        mysql> CHANGE MASTER TO MASTER_HOST='X.X.X.X',MASTER_USER='slave',MASTER_PASSWORD='fai',MASTER_LOG_FILE='mysqld-bin.000015',MASTER_LOG_POS=106;
        mysql> slave start;
        mysql> show slave status\G;

8. master上解锁：

        mysql> unlock tables;

