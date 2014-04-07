---
layout: post
title: "Django在apache中的配置"
date: 2014-04-07 20:45:40 +0800
comments: true
categories: 
- apache
---
apache配置文件（省略无关配置）：

```
<VirtualHost *:80> 
        WSGIDaemonProcess DjangoProject processes=2 threads=15 python-path=/home/andrew/hg_repo/Django

        WSGIProcessGroup DjangoProject 
        Alias /static/js/ /home/andrew/hg_repo/Django/DjangoProject/static/js/
        Alias /static/css/ /home/andrew/hg_repo/Django/DjangoProject/static/css/ 

        WSGIScriptAlias / /home/andrew/hg_repo/Django/DjangoProject/wsgi.py
        #WSGIPythonPath /home/andrew/hg_repo/Django
        <Directory "/home/andrew/hg_repo/Django/DjangoProject">
           Order allow,deny 
           Allow from all 
        </Directory>
</VirtualHost> 
```

这里让mod_wsgi工作在daemon模式下（官方推荐），python-path表明项目包的路径。WSGIPythonPath 一项在VirtualHost 内不能使用，只能放到httpd.conf中，作用与python-path相同。

注意 Alias /static/js/及css要放在WSGIScriptAlias前面，让静态文件给apache先处理。

另外，如果django的settings.py中把Debug设为了False,必须修改以下配置：


```
# Hosts/domain names that are valid for this site; required if DEBUG is False
# See https://docs.djangoproject.com/en/1.5/ref/settings/#allowed-hosts 

ALLOWED_HOSTS = []
```
改为
```
ALLOWED_HOSTS = [”*“]
``` 

当然，TIME_ZONE的值也应当首先改为'Asia/Shanghai' （没有北京的）

 

