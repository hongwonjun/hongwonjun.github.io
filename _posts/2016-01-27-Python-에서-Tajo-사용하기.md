---
layout: post
title: "Python 에서 Tajo 사용하기"
date: 2016-01-27 00:00:00 +0900
author: Hong Wonjun
categories: Python
tags: tajo, python, data
---

라인웍스에서는 정적인 데이터를 분석하는 경우 데이터레이크(Data Lake) 로 [TAJO](http://tajo.apache.org/) 를 사용하고 있다. 회사 내에서 분석하고 서비스로 만들어내는 모든 데이터는 여기에서 시작된다. 우선 TAJO 에 Query 를 이용하여 데이터를 쪼개보고, 붙여보고, 나열해본 후에는 이를 csv 파일로 만든 후 에 [Python](https://www.python.org/) 으로 load 하여 그래프를 그리거나 머신러닝 알고리즘을 수행하는 형식이다.  
*(Python 의 경우 직접 실행하는 방법도 있고 [IPython Notebook](http://ipython.org/notebook.html) 을 사용하기도 한다.)*

결국 Data - TAJO - CSV - Python - RESULTS 와 같은 단계를 거치게 되는데 이 단계를 단축시켜보자는게 이 포스팅의 목적이다.

## TAJO 에 대해서 간단히 알아보자. ##

-----

> Apache Tajo is a robust big data relational and distributed data warehouse system for Apache Hadoop. Tajo is designed for low-latency and scalable ad-hoc queries, online aggregation, and ETL (extract-transform-load process) on large-data sets stored on HDFS (Hadoop Distributed File System) and other data sources. By supporting SQL standards and leveraging advanced database techniques, Tajo allows direct control of distributed execution and data flow across a variety of query evaluation strategies and optimization opportunities. ([TAJO 공식 홈페이지](http://tajo.apache.org/))

TAJO 공식 홈페이지에서 가져온 소개글인데, 간단히 정리하면 Hadoop 기반의 Data warehouse 로 SQL 질의(query) 를 통하여 데이터를 다룰 수 있는 시스템이다.

## Python 에서 TAJO 를 사용해보자 ##

-----

간단하게 이야기해서 Python 에서 TAJO 를 호출하여 사용할 수 있도록 하면된다. 

우선 TAJO 가 JDBC 를 지원하기 때문에 [Tajo JDBC Driver](http://tajo.apache.org/docs/current/jdbc_driver.html) 를 이용하도록 한다.

Python 에서 JDBC 를 사용하기 위해서 [JayDeBeApi](https://pypi.python.org/pypi/JayDeBeApi/) 을 설치한다.

설치는 pip 를 이용해서 할 수 있다.

{% highlight bash %}
$ pip install JayDeBeApi
{% endhighlight %}

JayDeBeApi 기본 Tutorial 과 다른 부분이 있는데 JPype 를 이용해서 JVM 을 올려줘야 한다. (jar 파일의 위치는 tajo 가 설치된 위치를 이용하여 바꿔주자.

{% highlight python %}
import jaydebeapi, jpype
jar = '${TAJO_HOME}/share/jdbc-dist/tajo-jdbc-x.y.z.jar'
args = '-Djava.class.path=%s' % jar
jvm_path = jpype.getDefaultJVMPath()
jpype.startJVM(jvm_path, args)
{% endhighlight %}

이제 tajo 에 connect 요청을 할 수 있다.

{% highlight python %}
conn = jaydebeapi.connect("org.apache.tajo.jdbc.TajoDriver","jdbc:tajo://host:port/database")
curs = conn.cursor()
{% endhighlight %}

아래와 같이 query 를 실행할 수 있다.

{% highlight python %}
query = "select * from table1"
cur.execute(query)
cur.fetchall()
{% endhighlight %}

-----

* 이 글은 현재 일하고 있는 [라인웍스의  블로그](http://linewalks.com/blog) 에 기고한 글로 원문의 주소는 아래와 같다.
* http://linewalks.com/archives/1085

