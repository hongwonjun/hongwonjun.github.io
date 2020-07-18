---
layout: post
title: "Data Quality Dashboard 구동하기"
date: 2020-07-15 23:45:00 +0900
author: Hong Wonjun
categories: Develop
tags: develop, ohdsi
---

이번에는 DataQualityDashboard 를 구동해보자.

https://github.com/OHDSI/DataQualityDashboard

Github 에는 설치 관련 내용이 없고 아래 링크에서 확인할 수 있다.

https://ohdsi.github.io/DataQualityDashboard/articles/DataQualityDashboard.html


사실 설치 방법 자체는 굉장히 간단한 편이다.

{% highlight r %}
R Installation
install.packages("devtools")
devtools::install_github("OHDSI/DataQualityDashboard")
{% endhighlight %}

다만 그 이후로 메뉴얼에 따라 진행시 중간중간 아래와 같은 에러가 발생했다.

{% highlight r %}
Error: package 'xxx' was installed by an R version with different internals; it needs to be reinstalled for use with this R version.
{% endhighlight %}

이 떄는 아래와 같이 해당 package 를 재설치한다.

{% highlight r %}
install.packages("xxx", dependencies = TRUE)
{% endhighlight %}

실제로 아래 package 들을 재설치하였다.

- htmltools
- httpuv
- promises
- fastmap
- urltools
- triebeard

위 설치 과정에서 실제로 같은 오류가 발생하는 경우가 있기 떄문에 설치시 오류가 나는 경우 그 오류 메세지를 잘 읽어봐야 한다.
(의존성이 있는 다른 package 의 문제인 경우가 있으니 그럴 떄는 해당 package 를 먼저 설치해보자.)

기존에 구축한 Synpuf5 로 위 메뉴얼에 따라 실행하는 경우 아래와 같은 메세지가 발생한다.

{% highlight r %}
Processing check description: measurePersonCompleteness
[Level: TABLE] [Check: measurePersonCompleteness] [CDM Table: VISIT_DETAIL] [CDM Field: NA] Error executing SQL:
org.postgresql.util.PSQLException: ERROR: relation "synpuf5.visit_detail" does not exist
  Position: 332
An error report has been created at  dataquality2/Synpuf5/errors/TABLE_measurePersonCompleteness_VISIT_DETAIL_NA.txt
{% endhighlight %}

위 설명 페이지에는 없지만 cdmVersion 변수를 사용해야 한다. synpuf5 를 사용중이라면 cdmVersion = "5.2.2" 를 추가한다.

진행한 후 Shiny App 을 이용해서 그 결과를 볼 수 있다.

{% highlight r %}
DataQualityDashboard::viewDqDashboard(jsonPath = file.path(getwd(), outputFolder, cdmSourceName, sprintf("results_%s.json", cdmSourceName)))
{% endhighlight %}

![Data_Quality_Dashboard_00.png](../../assets/post-images/2020-07-18/Data_Quality_Dashboard_00.png)

![Data_Quality_Dashboard_01.png](../../assets/post-images/2020-07-18/Data_Quality_Dashboard_01.png)

현재는 구동한 결과를 파악하고 있다.
Achilles 와 어느 정도 중복되는 느낌이 있으나 Acheilles 와는 다르게 Data Quality 만 조금 더 자세하게 파악하는 툴로 파악된다.
