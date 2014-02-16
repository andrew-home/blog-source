---
layout: post
title: "使用git subtree 管理博客主题"
date: 2014-02-15 16:32:31 +0800
comments: true
categories: 
- git
---

# 背景
以下是本博客源码的目录结构

```plain blog-source mark:14-15
blog-source
├── CHANGELOG.markdown
├── Gemfile
├── Gemfile.lock
├── README.markdown
├── README.md
├── Rakefile
├── _config.yml
├── _deploy
├── config.rb
├── config.ru
├── plugins
├── public
├── sass 
└── source 
```

``` ruby Discover  http://www.noulakaz.net/weblog/2007/03/18/a-regular-expression-to-check-for-prime-numbers/ start:51 mark:52,54-55
class Fixnum
  def prime?
    ('1' * self) !~ /^1?$|^(11+?)\1+$/
  end
end
```

{% codeblock Coffeescript Tricks lang:java start:51 mark:51,54-55 %}
# Given an alphabet:
alphabet = 'abcdefghijklmnopqrstuvwxyz'

# Iterate over part of the alphabet:
console.log letter for letter in alphabet[4..8]
{% endcodeblock %}

``` coffeescript Coffeescript Tricks start:51 mark:52,54-55
# Given an alphabet:
alphabet = 'abcdefghijklmnopqrstuvwxyz'

# Iterate over part of the alphabet:
console.log letter for letter in alphabet[4..8]
```
