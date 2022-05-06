---
layout: single
title:  "두변수의 연관성"
categories: statistics
tag: [두변수의 연관성]
toc: true
author_profile: false
---

## 두 확률 변수의 연관성

### 기대값의 성질

* 두 확률변수 X,Y에 대해, 다음식은 항상 성립
  $$E(aX+bY) = aE(X)+bE(Y)$$

* 만일 두 확률변수가 독립이라면 다음 식이 성립
  $$E(XY)=E(X)E(Y)$$

  > Ex) X의 평균은 3%, Y의 평균은 1% 인경우 , 100X와 50Y로 구성된 포트의 평균 수익률은 ?
  >    $$E(100X+50Y)=100E(X)+50E(Y) = 3.5$$


### 공분산

* 결합분포의 관계를 수치화 시킴
* 공분산은 두 확률변수 X 와 Y의 결합분포를 이용하여 X 와 Y 의 선형 (또는 직선) 연관성을 나타낸 측도 (주의: **선형관계가 아니면 관계를 찾지 못함**)
* 정의
  $$Cov(X,Y) = E[(X-\mu_{x})(Y-\mu_{Y})]$$

   $$Cov(X,Y) = \left\{\begin{matrix} \sum_{모든x}\sum_{모든y}(X-\mu_{x})(Y-\mu_{Y}) , 이산형 확률변수의 경우 \\ \int_{-∞}^{∞} \int_{-∞}^{∞} (X-\mu_{x})(Y-\mu_{Y}) f(x,y)dxdy, 연속형 확률변수의 경우 \\ \end{matrix}\right.$$


### 분산의 성질

* 두 확률변수 X,Y 에 대해여, 다음식은 항상 성립
$$Var(aX+bY) = a^{2}Var(X) + b^{2}Var(Y) + 2ab Cov(X,Y)$$

* 만일 독립이라면 $Var(aX+bY) = a^{2}Var(X) + b^{2}Var(Y)$

### 상관계수

* 공분산의 문제점
  * 공분산은 X와 Y의 단위 영향을 받음
  * 따라서, 공분산의 값을 가지고 ***연관성의 강도를 측정 할 수 없다***.
* X와 Y의 단위 영향을 제거함
  * X와 Y의 스케일을 표준화 시켜서 공분산을 계산
  * 즉 , $\sigma_{X} , \sigma_{Y}$ 로 공분산을 나누어 준다 -> (표본)상관계수
 $$ \rho = \frac{Cov(X,Y)}{ \sigma_{X} \sigma_{Y} } , r = \frac{ \sum_{i=1}^{n} [(X_i-\bar{X})(Y_i-\bar{Y})] }{ \sqrt{\sum_{i}^{n}(X_i-\bar{X})^2} \sqrt{\sum_{i}^{n}(Y_i-\bar{Y})^2} } $$
