---
layout: post
title: "apache安装及配置"
date: 2014-04-07 20:57:57 +0800
comments: true
categories: 
- apache
---
# 安装apache

1. 下载apache-2.4.3源码，解压到/usr/local/apache/httpd-2.4.3
2. 下载依赖包apr及apr-util，解压到/usr/local/apache/httpd-2.4.3/srclib/apr 及/usr/local/apache/srclib/apr-util
3. 下载pcre源码并安装到/usr/local/pcre
4. 在/usr/local/apache/httpd-2.4.3目录下执行命令

        ./configure --prefix=/usr/local/apache --with-included-apr --with-pcre=/usr/local/pcre

5. 执行命令make && make install

# cgi配置
 
首先需要在httpd.conf中取消```#LoadModule cgid_module modules/mod_cgid.so``` 的注释。

可以有两种方法指定cgi的目录

1. 将cgi程序放到指定目录（如/usr/local/apache/cgi-bin），然后在httpd.conf中添加

        ScriptAlias /cgi-bin "/usr/local/apache/cgi-bin" 

    语句，但这时/usr/local/apache/cgi-bin中的静态文件（.html）将无法访问，会报Server Internal Error的错误，因为 ScriptAlias 是告诉apache 该目录下的都是script

2. 如果需要将cgi与静态文件一起放在一个文件夹（如 /usr/local/apache/www），则在httpd.conf中添加

        Alias /www "/usr/local/apache/www"
        <Directory "/usr/local/apache/www">
            AllowOverride None
            Options ExecCGI
            Require all granted #重要，否则报403错误
            AddHandler cgi-script .cgi
        </Directory>

    就可以让cgi及html都可以访问了
