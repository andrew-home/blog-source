---
layout: post
title: "wsgi配置"
date: 2014-04-07 20:50:00 +0800
comments: true
categories: 
- apache
---
在/etc/apache2/sites-enabled/000-default 中添加如下配置

```
WSGIScriptAlias /wsgi-bin /usr/lib/wsgi-bin/index.py
<Directory "/usr/lib/wsgi-bin"> 
    Order allow,deny 
    Allow from all 
</Directory>
``` 

WSGIScriptAlias 一句表示所有```http://XXX/wsgi-bin/```下的请求都由/usr/lib/wsgi-bin/index.py这个wsgi app处理。

注意：如果写成 WSGIScriptAlias /wsgi-bin/  /usr/lib/wsgi-bin/index.py则表示仅 ```http://XXX/wsgi-bin/``` 这个url是由index.py处理，如果请求 ```http://XXX/wsgi-bin/other``` 则报404 
