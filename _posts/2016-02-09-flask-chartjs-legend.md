---
layout: post
title: "Python - Flask 에서 Chart.js Legend 구현하기"
date: 2016-02-09 00:00:00 +0900
author: Hong Wonjun
categories: Python
tags: python, flask, chart.js
---

웹에 그래프를 그리는 Javascript Library 중에 [Chart.js](http://www.chartjs.org) 가 있다.
꽤 심플해서 사용하는데 legend 이 없어서 검색을 해보니 아래와 같은 방식으로 legend 을 구현한다.
Chart Options 에 아래 내용을 추가한다.

{% highlight javascript %}
//String - A legend template
legendTemplate : "&lt;ul class="\"&lt;%=name.toLowerCase()%"&gt;-legend\"&gt;&lt;li&gt;&lt;span style="\"background-color:&lt;%=datasets[i].strokeColor%"&gt;\"&gt;&lt;/span&gt;&lt;/li&gt;&lt;/ul&gt;"
{% endhighlight %}

그래프 생성 후 아래 코드를 호출한다.

{% highlight javascript %}
myLineChart.generateLegend();
// =&gt; returns HTML string of a legend for this chart
{% endhighlight %}

위와 같이 호출하는 경우 HTML 코드가 반환되기 때문에 아래와 같이 정해진 id 에 반환된 HTML 코드를 넣으면 된다.

{% highlight markup %}
&lt;div id="legend"&gt;&lt;/div&gt;
{% endhighlight %}

{% highlight javascript %}
document.getElementById('legend').innerHTML = myLineChart.generateLegend();
{% endhighlight %}

이 때 css 를 아래와 같이 입력해두면 작은 사각형으로 그려낼 수 있다.

{% highlight css %}
#legend li span {
  display: inline-block;
  width:11px;
  height:11px;
  margin-right:7px;
}
{% endhighlight %}

하지만 이를 Flask 에서 실행하는 경우 아래와 같은 에러가 발생한다.

{% highlight clike %}
"TemplateSyntaxError: tag name expected"
{% endhighlight %}

jinja2 와의 문법 충돌 때문인데, &#123;% 와 %&#125; 를 &#123;\% 와 \%&#125; 로 수정해야 한다. 즉, 위 코드를 아래와 같이 수정하면 된다.

{% highlight javascript %}
legendTemplate : "&lt;ul class="\"&lt;%=name.toLowerCase()%"&gt;-legend\"&gt;&lt;li&gt;&lt;span style="\"background-color:&lt;%=datasets[i].lineColor%"&gt;\"&gt;&lt;/span&gt;&lt;/li&gt;&lt;/ul&gt;"}
{% endhighlight %}

Reference
  * [Chart.js|Documentation](http://www.chartjs.org/docs/)
  * [Chart.js Legend Customization](http://jsfiddle.net/vrwjfg9z/)
  * [jinja2 - TemplateSyntaxError: tag name expected](http://stackoverflow.com/questions/26509287/templatesyntaxerror-tag-name-expected)

