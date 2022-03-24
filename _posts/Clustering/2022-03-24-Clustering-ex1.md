---
layout: single
title:  "군집분석 예제 - 1 "
categories: Clustering
tag: [Ai bigData, 군집분석 사례, K-means Clustering]
toc: true
author_profile: false
---

K-means Clustering Method 방법을 활용한 군집 분석 사례 (간단분석)

### A. 데이터 준비

|total|price|period|variety|
|--|--|--|--|
|5.1|1.4|0.2|3.5|
|...|...|...|...|
|5.9|5.1|1.8|3|

* 전체 150개 데이터,  [파일첨부](../../images/2022-03-24-Clustering-ex1/clustering_ex1.csv)
* total : 상품을 한번 구매시 지출금액
* price : 평균가격
* period : 구매간격 Mth
* 종속변수가 없는 그냥 Data Set 이다


### B. 필요 라이브러리 로드 및 데이터 확인

``` {r}

> library(cluster)
> result <- read.csv("clustering_ex1.csv", header=TRUE)
> View(result)

```


### C. 군집수 K 결정
``` {r}

> result2 <- hclust(dist(result), method="ave")

# hang= -1 옵션은 bottom을 맞춘다. 
> plot(result2, hang=-1, labels=result2$ID) 

```

![](../../images/2022-03-24-Clustering-ex1/Cluster_1.png)<!-- -->

통상 K 개수를 정하는 결정은 해당분야 전문가에 맡기지만, 위 그림을 보면 3개 ,4개의 군집을 형성하는 것을 볼수 있다.
일단 여기에서 3개로 정하기로 한다.

### D. 군집분석 실행 : K-means Cluster
``` {r}

> set.seed(1234)
> result3 <- kmeans(result, 3)

# price by total 에대한 plot 확인
> plot(result[c("price", "total")], col=result3$cluster)

# pch - 포인트 모양,  cex - 포인트 크기, result3$centers - 군집의 중심 , col : color code 또는 이름
> points(result3$centers[,c("price", "total")], col=1:3, pch=8, cex=2)

```



![](../../images/2022-03-24-Clustering-ex1/Cluster_2.png)<!-- -->

### E. 군집의 특성 설명

``` {r}
> result3
```

```
K-means clustering with 3 clusters of sizes 50, 62, 38

Cluster means:
     total    price   period  variety
1 5.006000 1.464000 0.244000 3.418000
2 5.901613 4.393548 1.433871 2.748387
3 6.850000 5.742105 2.071053 3.073684

Clustering vector:
  [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 3 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
 [78] 3 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 3 2 3 3 3 3 2 3 3 3 3 3 3 2 2 3 3 3 3 2 3 2 3 2 3 3 2 2 3 3 3 3 3 2 3 3 3 3 2 3 3 3 2 3 3 3 2 3 3 2

Within cluster sum of squares by cluster:
[1] 15.24040 39.82097 23.87947
 (between_SS / total_SS =  88.4 %)

Available components:

[1] "cluster"      "centers"      "totss"        "withinss"     "tot.withinss" "betweenss"    "size"         "iter"         "ifault"      
```
3개의 Cluster 크기와 Cluster의 평균이 나온다. 
상기 분석사례는 표준화된 데이터로 하지 않았기 때문에 (군집분석은 변수 척도에 민감하기 때문에 scale로 표준화가 필요) 사용했던 변수에 대한 평균을 나타내고 있다.

* Clustering vector: 의 나열을 보고 Clustering에 대한 특성을 확인 할 수 있다.

* Within cluster sum of squares by cluster (군집내 군집제곱합): k-means는 그룹내 분산을 최소화 하고 그룹간 분산을 최대화 한다. n샘플수 가 k cluster에 새플이 할당됨에 따라 88.4%의 제곱합의 감소를 달성 했다는 의미이다.