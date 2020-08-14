---
layout: post
title: "ATLAS 에서 Achilles 결과 확인시 trouble shooting"
date: 2020-08-15 02:01:00 +0900
author: Hong Wonjun
categories: Develop
tags: develop, ohdsi
---

지난 [Achilles 구동하기](https://hongwonjun.github.io/2020-05-19/Achilles-%EA%B5%AC%EB%8F%99) 를 좀 더 살펴보도록 하자.

ATLAS 에 접속하면 좌측 상단에 Data Sources 라는 메뉴를 볼 수 있다.

여기에서 Achilles 에서 추출한 정보(구축된 CDM 의 데이터 통계) 를 확인할 수 있다.

해당 메뉴에서 제공하는 그래프는 다음 포스트에서 하나씩 확인해보시로 하고,  
몇몇 메뉴 (Dashboard, Data Density, Person, Visit, Death, Achilles Heel) 는 결과가 잘 보이나  
Condition Occurrence, COndition Era, Procedure, Drug Exposure, Drug Era, Measurement, Observation 메뉴에서는 아래와 같은 오류가 발생한다.

![2020-08-15-01.png](../../assets/post-images/2020-08-15/2020-08-15-01.png)

우선 WebAPI 로그를 확인해보자.

{% highlight java %}
 nested exception is org.postgresql.util.PSQLException: ERROR: relation "results.concept_hierarchy" does not exist
  Position: 840
{% endhighlight %}

concept_hierarchy table 이 없다고 한다.

[Achilles 프로젝트에서 찾아보면 이슈도 올라와있다.](https://github.com/OHDSI/Achilles/issues/469)

label 이 `wontfix` 가 달려있고, 수동으로 테이블을 만들어줘야 한다.

{webapi URL}/ddl/results?schema=ohdsi&dialect=postgresql 로 접속하면 코드를 받을 수 있다.

이 중에 Create hierarchy lookup table for the treemap hierarchies 를 검색해서 아래의 코드를 찾아보자.
기존에 Synpuf5 를 기준으로 생성했을 경우 아래의 코드를 사용해도 된다.

이를 Database 에서 실행한다.

<script src="https://gist.github.com/hongwonjun/d26e0d0e6c677663144dfda9e6a63f43.js"></script>

다시 접속을 해보면 아래와 같이 적상작동 하는 것을 볼 수 있다.

![2020-08-15-02.png](../../assets/post-images/2020-08-15/2020-08-15-02.png)

