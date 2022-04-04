---
layout: single
title:  "기계학습 개요"
categories: mushineLearn
tag: [기계학습 개요]
toc: true
author_profile: false
---

### 개요
* 데이터를 통해 기계가 스스로 학습하게 하는 방법을 의미한다.
* 일반 프로그램은 규칙을 사람이 직접 입력하지만, 머신러닝은 컴퓨터가 직접 규칙을 스스로 찾아서 하는 방법을 의미한다.

### 유형

* 지도학습(Supervised Learning)
  * 정답/오답을 알려주며 학습시키는 지도 학습 방식
  * 입력값(X data)이 주어지면 입력값에 대한 Label(Y data)를 주어 학습시키면 대표적으로 분류,회귀 문제가 있다.
  * 종류
    1. 분류(Classification)
         - 주어진 데이터를  정해진 카테고리(라벨)에 따라 분류하는 문제를 의미.
         - 분류 문제는 **이진분류(O,X)** 또는  사과, 바나나, 포도 등의 **다중분류** 문제가 있다
         - 스팸 분류 등 ...

    2. 회귀 (Classification)
         - 데이터의 특징을 기준으로 연속된 값을 예측, 트렌드나 경향을 예측할 때 사용됨
         - 예를 들어 아파트 가격 예측 등


* 비지도학습(Unsupervised  Learning) : 
  * 정답/오답이라는 답이 없는 데이터들을 자동으로 군집하여 규칙을 스스로 발견하게 하는 학습 방식
  * 라벨링되지 않는 데이터로 부터  패턴이나 형태를 찾기 때문에 조금 더 난이도가 있음
  * Clustering, Dimentionality Reduction, Hidden Markov Model, GAN(generative Adversarial Network)모델 등
  * 지도/비지도 학습모델(Semi-Supervised Learning)을 섞어서 사용할 수 있음.
  
* 강화학습(Reinforcement Learning)
  * 실패와 성공의 과정을 반복하며 학습, 지속적인 시행착오를 통해 보상을 극대화 하는 방법으로 진행 ( 자율주행, 로봇제어, 바둑을 두는 AI 등 )
  * 강화학습개념 ( 에이전트(Agent)/환경(Environment)/상태(State)/행동(Action)/보상(Reward) )
    * 바둑을 학습하는 경우 에이전트(Agent)가 게임 환경(environment)에서 현재 상태(state)에서 높은 점수(reward)를 얻는 방법을 찾아가며 행동(action)하는 학습 방법으로 특정 학습 횟수를 초과하면 높은 점수(reward)를 획득할 수 있는 전략이 형성, **행동을 위한 행동목록은 사전정의** 필요

  
### 대표적 알고리즘

   |구분|종류|알고리즘|
   |-|-|-|
   |지도학습(Supervised Learning)|Classification|kNN|
   |||Naive Bayes|
   |||Support Vector|
   |||Machine Decision|
   ||Regression|Linear Regression|
   |||Locally Weighted Linear|
   |||Ridge|
   |||Lasso|
   |비지도학습(Unsupervised Learning)|| Clustering|
   |||K Means|
   |||Density Estimation|
   |||Exception Maximization|
   |||Pazen Window|
   |||DBSCAN|
   |강화학습(Reinforcement Learning)|| DQN|
   ||| A3C|


### 기계학습의 3단계

1. 데이터의 정리와 이해: 속성 추출(Feature Extraction), 전처리(Preprocessing)
2. 데이터를 이용해 모델 학습 
3. 학습한 모델의 평가 (Model Evaluation): 교차 검증(cross-validation), 파이프라인(pipeline) 등이 있음

