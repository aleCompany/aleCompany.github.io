---
layout: single
title:  "의사결정나무 분석절차 및 R 패키지"
categories: decisiontree
tag: [인공지능,의사결정나무]
toc: true
author_profile: false
---

## 1. 의사결정나무 분석절차 

### A. 데이터의 준비

  ```{r}
     # Data의 구성은 종속변수가 있어야 한다.
     > result <- read.csv("mydata.csv", header=TRUE)
     > head(result)
  ```

### B. 데이터의 구분
  의사결정나무 모델의 성능 평가를 위해서 train과 test 데이터로 구분한다.

  ```{r}
     # 7:3 비율을 랜덤으로 나눈다.
     set.seed(1234) 
     # Traing Data 70%, Test Data 30%로 구부한다. 
     # replace = TRUE 중복을 허용하는지, 허용하지 않는지 일반적으로는 허용한다.TRUE 허용
    > resultsplit <- sample(2, nrow(result), replace=TRUE, prob=c(0.7, 0.3))

    > trainD <- result[resultsplit==1,] # train Data
    > testD <- result[resultsplit==2,] # test Data
  ```

### C. 의사결정  나무의 형성
  **Training Data**를 가지고 예측모델  형성을 한다.

  ``` {r}
      # rpart 를 사용시 ctree, tree - ctree(response~. , data=trainD) , tree(response~. , data=trainD)
    > trainModel <- rpart(respoose(종속변수)~. , data=trainD) # response 종속변수
  ```
### D. Fitting 값을 확인 (Coverage, 설명에 대한 분석)
  **Training Data** 를 가지고 검증 해본다.
  ``` {r}
      # Coverge 즉 설명에 대한 부분을 확인 해본다.
    > table(predict(trainModel, newdata=trainD, type="class"), trainD$response) 
  ```
  ```  
    ##       high  low  nr
    ## high    31    1   0
    ## low      3   37   0
    ## nr       0    0  40    
  ```
  
  위 예시 결과 에서 Fitting Ratio = (112-4) / 112 = 0.964 가 된다. (모델의 우수성 검증)

### E. Accuracy of Model (Test 데이터로 예측률을 확인함)

  **Test Data**를 가지고 검증해 본다

  ``` {r}
    > table(predict(trainModel, newdata=testD , type="class"), testD$response)
  ```
  ```  
    ##       high  low  nr
    ## high    14    0   0
    ## low      2   12   0
    ## nr       0    0  10    
  ```  
위 예시 결과 에서 Accuracy Ratio = (38-2) / 38 = 0.947 가 된다.

### E. 의사결정나무를 확인

  ``` {r}
      # rpart 함수를 사용 했을때는 rpart.plot 으로 그리고
      # ctree 함수를 사용 했을때는 plot(trainModel) or plot(trainModel, type='simple') 로 확인 가능하다.
    > rpart.plot(trainModel)
  ```

## 2. 의사결정 나무 생성 관련 R 패키지

|함수|내용|사용법|package|
|--|--|--|--|
|rpart()|CART(Classification And Regression Trees) 방법을 사용|rpart(formula, data)|install.packages("rpart")|
|ctree()|Unbiased recursive partitioning based on permutation test 방법을  사용|ctree(formula, data)|install.packages("party")|

2가지 방법의 차이는 가지치기를 하는데 통계 방법이 다르다는 것.
* ctree()는 rpart() 함수의 2가지 문제점(통계적 유의성에 대한 판단없이 노드분할에 대한 과적합, 다양한 값으로 분할 가능한 변수가 다른 변수에 비해 선호 되는 문제)를 해결 해줌
