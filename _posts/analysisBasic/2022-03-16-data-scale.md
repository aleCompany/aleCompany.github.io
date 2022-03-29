---
layout: single
title:  "통계 자료의 스케일링"
categories: analysisBasic
tag: [정규화, 표준화]
toc: true
author_profile: false
---

데이터 분석시 많이 발생하는 것이 단위의 불일치 이다. 이에 대한 해결로는 정규화(Normalization)과 표준화(Standardization)이 있다. 

이 스케일링 방법은 단위가 다를 때 대상 데이터를 같은 기준으로 보도록 한다.

###  자료의 정규화 (normalization)

* 데이터를 특정 구간으로 변경하는 방법이다.
* Min-Max Scaling 이라고도 하는 방법으로, 최대값과 최소값을 사용하여 원데이터의 **최소값을 0, 최대값을 1** 사이에 들도록 하는 방법이다.
* 데이터 군 내에서 특정 데이터가 가지는 위치를 알아 볼때 주로 사용한다.
  
$$ x_{new} = \frac{x_{i}-x_{min}}{x_{max} - x_{min}} $$

* normalization 의 R 표현
  
  ``` {r}

  # 원자료
  > x= 1:100
  # 사용사 함수 정의 ( 또는 스케일함수 사용 zz = scale(x, center=min(x), scale=diff(range(x))) , 직접계산 zz=(x-min(x))/(max(x)-min(x)) )
  > normal = function(x) { return((x-min(x))/(max(x)-min(x))) }
  >  zz=normal(x)
  ```

###  자료의 표준화 (standardization)

* 데이터를 평균0을 중심으로 양쪽으로 데이터를 분포 시키는 방법이다.
* 즉 자료를 **평균이 0 , 표준편차가 1**이 되도록 변환시키는 과정이다.
* 표준화하여 계산된 값을 Z값(Z-value)라고도 부름
* Z값은 관측값이 자료의 평균으로 부터 몇 배의 표준편차만큼 떨어져 있는지를 나타내는 상대적 위치를 의미
* 분자와 분모의 단위가 상쇄되어 단위가 없다 ( (측정값-평균) / 표준편차).(unitless)
  
  $$ Z_{i} = \frac{x_{i}- \bar{x}}{S} $$

* standardization 의 R 표현 
  
  ``` {r}
  # 원자료
  > x= 1:100

  # scale 함수 사용 ( 또는 (x-mean(x)) / sd(x) )
  > z = scale(x)

  ```