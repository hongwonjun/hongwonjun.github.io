---
layout: post
title: "Ruby - migrate_from_mantis 실행시 undefined method `set_inheritance_column' for MantisMigrate&#58;&#58;MantisCustomField(Table doesn't exist)&#58;Class 에러"
date: 2015-09-05 00:00:00 +0900
author: Hong Wonjun
categories: Trouble-Shooting
tags: ruby, mantis, redmine
---

Mantis 에서 Redmine 으로 Migration 을 진행할 때 아래와 같은 에러가 발행하였다.

{% highlight ruby %}
(in /usr/share/redmine)
rake aborted!
NoMethodError: undefined method `set_inheritance_column' for MantisMigrate::MantisCustomField(Table doesn't exist):Class
{% endhighlight %}

코드를 아래와 같이 수정

{% highlight ruby %}
self.inheritance_column = :none instead of set_inheritance_column :none
{% endhighlight %}

Reference
- [Trac Migrate Script with Redmine 3.0](https://www.redmine.org/issues/19173)