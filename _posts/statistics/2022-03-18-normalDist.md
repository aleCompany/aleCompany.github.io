---
layout: single
title:  "연속형 확률분포 Ⅰ(정규분포)"
categories: statistics
tag: [연속형 확률분포,정규분포]
toc: true
author_profile: false
---

### 정규분포(normal distribution)

* 연속형 확률분포에서 가장 대표적인 확률분포 : $X \sim N(\mu,\sigma^2)$
* 확률밀도 함수

  $$f(x)= \frac {1} {\sqrt{2 \pi \sigma^2 }} e^{ - \frac{(x-mu)^2}{2\sigma^2}}$$

* 평균, 분산
  $$E(X)= \mu, Var(X)= \sigma^2 $$

* 특징
  * 평균을 중심으로 좌우 대칭, 평균 = 중앙값 = 최빈값
  * 평균에서 멀어질수록 확률밀도함수 값은 점차 작아진다
  * 분산이 클수록 확률밀도함수의 꼬리가 두꺼워진다.
  * 정규분포의 pdf 직접 적분하여 확률을 구하는 것은 쉽지 않다

* 모양
    <center><img src="../../images/2022-03-18-normalDist/pic-1.png" /></center>  

### 표준정규분포(standiard normal distribution)

* 표준정규분포: 평균이 0이고 분산이 1인 정규분포, $X \sim N(0,1)$
  * 표준정규분포를 따르는 확률변수를 일반적으로 Z 로 표현

* 확률밀도 함수
  $$f(x)= \frac {1} {\sqrt{2 \pi }} e^{ - \frac{1}{2} x^2 }$$

* 정규분포의 표준화
  * 모든 정규분포는 표준정규분포로 변환할 수 있다.

    $$ Z값 = \frac{X-\mu}{\sigma} , 표본의 경우 = \frac{X-\bar{X}_{n}}{\sigma}$$

* 표준정규분포의 누적분포함수
  * 파이라고 읽는다
    $$ Ø(x) = F(x) = P(Z≤x) = \int_{-∞}^{x} \frac {1} {\sqrt{2 \pi }} e^{ - \frac{1}{2} z^2 }dz $$  

  <center><img src="../../images/2022-03-18-normalDist/pic-2.png" /></center>

  $$Ø(1.98) = 0.9761, Ø^{-1}(0.9761) = 1.98, z_{0.0239} = 1.98$$