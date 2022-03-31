---
layout: single
title:  "관계분석"
categories: statRelation
tag: [관계분석]
toc: true
author_profile: false
---

관계분석은 그룹간의 차이를 검정하는 차이검정과 변수와 변수간 관계가 있는지 없는지를 검정하는 관계검정으로 나눠진다.

### 관계분석 종류

* 차이검정
  
    |분류|검정방법|비고|
    |-|-|-|
    |T-test<br>(2집단)|One Sample-test<br>(1표본)||
    ||Independent Sample-test<br>(2표본)||
    ||Paired Sample-test<br>(1표본, 시간이다른경우)||
    |ANOVA<br>(3집단)|One Way Anova<br>(1요인)||
    ||Two Way Anova<br>(2요인)||
    ||Repeated measured Anova<br>(시간이다른경우)||
    ||Two Way Repeated measured Anova<br>(요인, 시간이다른경우)||

* 관계검정
  
    |분류|검정방법|비고|
    |-|-|-|
    |교차분석(카이검정)<br>(두변수 범주형-범주형))|동질성검정|집단 간 분포의 동질성을 통해 두 집단간의 차이 검정<br>사전실험설계 : 사전에 그룹의 수를 결정해서 연구할 때 사용|
    ||독립성검정|변수들 간의 독립성/관련성 검정<br>사후사례대조 : 사후 결과를 토대로 연구할 때 사용|
    |Collelation<br>(1:1)||두변수간에 선형적인 관계를 분석-원인결과가 없고, 독립의미와는 다름|
    |Regression<br>(1:N)|Multiple Linear  Regression<br>(종속변수가이연속)|
    ||Logistic Regression<br>(종속변수가이명목)|
    ||Hierarchical Regression<br>(조절효과)|    
    ||Structural Equation Model<br>(매개효과)|