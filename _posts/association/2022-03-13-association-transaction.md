---
layout: single
title:  "연관성분석 transactions class"
categories: association
tag: [ai, big Data, 통계분석,연관성분석]
toc: true
author_profile: false
---

연관성 분석에 사용되는 데이터 유형은 idi형태가 아닌 itl 형태의 데이타가 사용된다.

###  **데이터의 형태 비교**
<div style="display:inline-block;">
    <table style="width:100px;">
    <caption>data frame</caption>
    <thead><tr><th>Id</th><th>Data</th></tr></thead>
    <tbody><tr><td>001</td><td>apple</td></tr>
    <tr><td>001</td><td>orange</td></tr>
    <tr><td>001</td><td>banna</td></tr>    
    <tr><td>002</td><td>carrot</td></tr>
    <tr><td>002</td><td>pickle</td></tr>    
    </tbody>
    </table>
</div>
<div style="display:inline-block;">
    <table style="width:300px;">
    <caption>transaction data frame</caption>
    <thead><tr><th>Id</th><th>Data</th></tr></thead>
    <tbody><tr><td>001</td><td>{apples, orange, banna}</td></tr>
    <tr><td>002</td><td>{carrot, pickle}</td></tr>
    <tr><td>003</td><td>...</td></tr>
    <tr><td>004</td><td></td></tr>
    <tr><td>005</td><td></td></tr>        
    </tbody>
    </table>
</div>

통계 패키지 R에서는 read.transactions 형태로 읽어 들이거나, as(data,"transactions")를 가지고 변형 해서 사용한다.


###  **데이터의 형태 변형방법**

1) 첫번재 Case ( 리스트를 transaction 데이터로 변형 ) 예제

```{r}
> buylist <- list( c("우유","버터","시리얼") , c("우유","시리얼"),  c("우유","빵"), c("버터","맥주","오징어") )
> buylist <- as(buylist,"transactions")
> inspect(buylist)
    items               
[1] {버터, 시리얼, 우유}
[2] {시리얼, 우유}      
[3] {빵, 우유}          
[4] {맥주, 버터, 오징어}
```

2) 두번재 Case (  [파일첨부](../../images/2022-03-13-association-transaction/transact_ex.csv) )

<p class="notice">※ 파일형태: 세로ID/ 가로 상품 Dummy 명칭 나열 형태 </p>


```{r}
> tmpath <- file( 'transact_ex.csv' , encoding = "EUC-KR")  #Save with Encoding... 세팅은 ECU-KR 로 되어 있어야 함
> buylist <- read.transactions( tmpath , format="basket" ,sep =",", header=TRUE ) # unix에서 한글이 깨지는 경우 처리 방법
> inspect(buylist)
    items         
[1] {배, 사과}    
[2] {감}          
[3] {배, 사과}    
[4] {감, 사과}    
[5] {감, 배}      
[6] {감, 배, 사과}
[7] {감, 배, 사과}
```


3) 세번재 Case ( [파일첨부](../../images/2022-03-13-association-transaction/transact_ex2.csv) )

※ 파일형태: ID는 중복되며 상품을 세로 형태로 배치
{: .notice} 


```{r}
> buylist <- read.transactions( 'transact_ex2.csv' , format="single", cols=c(1,2) ,sep =",", rm.duplicates=T ,header=TRUE )
> inspect(buylist)
    items          transactionID
[1] {사과}         1            
[2] {배, 사과}     2            
[3] {감}           3            
[4] {배, 사과}     4            
[5] {감, 사과}     5            
[6] {감, 배}       6            
[7] {감, 배, 사과} 7            
[8] {감, 배, 사과} 8 
```



4) 네번재 Case ( [파일첨부](../../images/2022-03-13-association-transaction/transact_ex3.csv) )

※ 파일형태: ID는 unique 상품데이터를 T/F boolean 형태로 저장 , 이 경우는 read.transactions 으로 안읽어짐
{: .notice--primary} 



```{r}
> tempfile <- read.csv('transact_ex3.csv', fileEncoding = "EUC-KR", header = TRUE)
> buylist <- as(tempfile[2:4], "transactions")
> inspect(buylist)
    items          transactionID
[1] {사과}         1            
[2] {사과, 배}     2            
[3] {감}           3            
[4] {사과, 배}     4            
[5] {사과, 감}     5            
[6] {배, 감}       6            
[7] {사과, 배, 감} 7            
[8] {사과, 배, 감} 8   
``` 


**[참고] T/F 상품데이터를 리스트 형으로 변경해서 as(data,"transactions") 하는 방법**

```{r}
> totaltemp <- list()
> for( i in 1:nrow(tempfile) ) {
   x <- c()
   for(j in 2:ncol(tempfile)) { 
     if ( tempfile[i,j] == TRUE) {
       x <- c(x,names(tempfile[j]))
     }
   }
   totaltemp[[i]] = x
 }
buylist <- as(totaltemp,"transactions")
```