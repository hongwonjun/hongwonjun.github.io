---
layout: post
title: "Python 에서 Tajo 사용하기 &#35;2 Query 결과값 중 숫자를 연산에 사용하기"
date: 2016-02-01 00:00:00 +0900
author: Hong Wonjun
categories: Python
tags: data, tajo, python
---

### Query 를 실행했다 그리고 ###

-----

이전 포스팅 마지막에 실행한 쿼리를 보자.

{% highlight python %}
query = "select * from table1"
cur.execute(query)
result = cur.fetchall()
{% endhighlight %}

결과를 python 에서 사용해보자.

{% highlight python %}
for each in result:
  print each[0] / each[1]
{% endhighlight %}

이런 경우 아래와 같은 Error 문구를 볼 수 있다.

{% highlight python %}
TypeError: unsupported operand type(s) for /: 'java.lang.Long' and 'java.lang.Long'
{% endhighlight %}

### 문제는 ###

-----

문제는 query 의 결과값이 숫자인 경우 아래와 같은 형식으로 리턴되기 때문이다.

{% highlight python %}
print type(each[2])
<class 'jpype._jclass.java.lang.Long'>
{% endhighlight %}

### 해결해보자 ###

-----

사실 해결책은 간단하다. 결국 TAJO 의 결과 값이 JDBC 를 통해서 오기 때문인데 .value 로 그 값을 받아올 수 있다.

{% highlight python %}
print type(each[2].value)
<type 'long'>
{% endhighlight %}

첫 코드를 수정해보자.

{% highlight python %}
for each in result:
  print each[0].value / each[1].value
{% endhighlight %}
  
-----

  * 이 글은 현재 일하고 있는 라인웍스의 블로그 에 기고한 글로 원문의 주소는 아래와 같다.
  * http://linewalks.com/archives/1104


