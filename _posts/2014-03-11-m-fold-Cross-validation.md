---
layout: post
title: "m-fold Cross-validation"
date: 2014-03-11 00:00:00 +0900
author: Hong Wonjun
categories: Data
tags: data, statistics
---

통계를 낼 때 데이터를 m 개의 set 으로 나눈 후에 m-1 개의 set 을 training data 로 사용, 나머지 1개의 set 을 testing data 로 사용하여 error rate 를 계산한다.

testing data 를 1번째부터 m 번째의 set 으로 선택하여 m 번의 testing 을 하고 각 testing 에서의 error rate 의 평균값을 구하여 전체의 error rate 를 계산하는 방법.

이를 이용하는 이유는 overfitting 을 피하기 위해서로 볼 수 있다.

참고 사이트  
- [Cross-validation (statistics) – WIKIPEDIA](http://en.wikipedia.org/wiki/Cross-validation_(statistics))
