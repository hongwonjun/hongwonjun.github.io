---
layout: post
title: "Redmine - Bitbucket 에 Git hook 설정"
date: 2015-09-04 00:00:00 +0900
author: Hong Wonjun
categories: Trouble-Shooting
tags: redmine, bitbucket, git, githook
---

기본적 아래의 Plugin 을 사용

- [Plugins Directory » Redmine Bitbucket](http://www.redmine.org/plugins/redmine_bitbucket)
- [steveqx / redmine_bitbucket](https://bitbucket.org/steveqx/redmine_bitbucket/overview)

설치와 설정 자체는 어렵지 않은데 중간에 아래와 같은 문구가 있다.

  > Configure SSH key for the web server user if need to pull from private repositories.
  (https://confluence.atlassian.com/display/BITBUCKET/Using+the+SSH+protocol+with+bitbucket)

이 부분 (private repositories) 에서 좀 삽질을 했는데,

{% highlight clike %}
$ sudo -u www-data ssh-keygen -t rsa
{% endhighlight %}

위와 같은 명령어로 www-data 에 공개키를 생성해서 bitbucket 에 등록해주면 된다.

키의 위치는 아래와 같다.

{% highlight clike %}
/var/www/.ssh
{% endhighlight %}