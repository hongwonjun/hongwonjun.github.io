---
layout: post
title: "Flask - Template page 에서 숫자 세자리마다 comma (,) 추가하기"
date: 2016-02-09 00:00:00 +0900
author: Hong Wonjun
categories: Python
tags: python, flask
---

Flask 에서 view page (template) 에서 숫자를 출력하는 경우 3자리마다 comma(,) 를 표시해줘야 하는 경우가 있다.

미리 서버쪽에서 string 으로 변환하는 경우도 있으나 template page 에서도 format function 을 사용하여 변환할 수 있다.

{% highlight python %}
&#123;&#123;'{0:,.1f}'.format(value)&#125;&#125;
{% endhighlight %}

위와 같이 사용하는 경우 3자리마다 comma(,) 를 출력하고 소수점 1자리까지 출력한다.