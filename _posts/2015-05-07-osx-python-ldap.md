---
layout: post
title: "Python - OS X 에서 python-ldap 설치시 오류"
date: 2015-05-07 00:00:00 +0900
author: Hong Wonjun
categories: Trouble-Shooting
tags: python, ldap
---

OS X 에서 python-ldap 을 설치할 때 아래와 같은 오류가 발생

{% highlight clike %}
Modules/LDAPObject.c:18:10: fatal error: 'sasl.h' file not found
 #include 
 ^
 1 error generated.
 error: command 'cc' failed with exit status 1
{% endhighlight %}

위와 같은 경우 아래와 같은 명령어를 실행하고

{% highlight clike %}
$ xcrun --show-sdk-path
{% endhighlight %}

여기에 나오는 path 를 심볼릭 링크로 연결한다.

{% highlight clike %}
$ sudo ln -s /usr/include /usr/include
{% endhighlight %}

다시 python-ldap 설치하면 된다.

{% highlight clike %}
$ sudo pip install python-ldap
{% endhighlight %}

Reference
- [(StackOverflow) Installing py-ldap on Mac OS X Mavericks (missing sasl.h)](http://stackoverflow.com/questions/22079173/installing-py-ldap-on-mac-os-x-mavericks-missing-sasl-h)
