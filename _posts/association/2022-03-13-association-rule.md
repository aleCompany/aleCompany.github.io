---
layout: single
title:  "연관성분석 이론"
categories: association
tag: [ai, big Data, 통계분석]
toc: true
author_profile: false
---

연관성분석은 장바구니분석(Market Basket Analysis) 또는 서열분석(Sequence Analysis) 이라고도 한다 <br>기업의 데이터에서 상품구매, 서비스 등 일련의 거래 또는 사건들 간의 규칙을 발견 할 목적으로 사용된다.
<br>주요 응용은 Market basket analysis, cross-marketing, catalog design, loss-leader analysis, clustering, classification 등


###  **연관규칙 형태**

조건과 반응의 형태(if then)로 이루어져 있음

- if A then B : 만일 A가 일어나면, B가 일어남
- 아메리카노를 마시는 손님 중 80%가 브라우니를 먹음
- 샌드위치를 먹는 고객의 30%가 탄산수를 마심

###  **연관규칙측도**
1. 지지도, 신뢰도, 향상도를 사용하여 측정하며, 산업 분석분야에 따라 기준값을 잘 설정해서 선택한다.

   - 지지도(Support)
      - A와 B가 포함되어 있을 확률
      - 전체 거래중 항목 A와 항목 B를 동시에 포함하는 거래의 비율
      - $support(A => B) = P(A \cap B)$
   - 신뢰도(Confidence, strength)
      - A를 포함하였는데, B가 포함되어 있을 확률
      - **연관성의 정도를 파악할 수 있다**
      - $confidence(A => B) = \frac {support(A,B)}{support(A)} = \frac {P(A \cap B)}{P(A)} = P(B \| A)$
   - 향상도 (lift)
      - A가 구매되지 않을 때 B구매확률에 비해, A가 구매 되었을 때 B의 구매확률의 증가 비
      - $lift(A => B) = \frac {confidence} {P(B)} = \frac {P(B \| A)} {P(B)} = \frac {P(A \cap B)} {P(A)P(B)}$
      - lift > 1면 관련도 상관계수가 높고, < 1이라면 A를 구매한 사람은 B를 구매하지 않는다.
      - 서로 독립이면 1이다
      - 즉 1보다 크면 보완재, 1보다 작으면 경쟁제 (맥주,소주)
  
2. Rule 형태는 "Body -> Head [support, confidence]"
   - ex) buys(x,"diapers")->buys(x,"beers") [0.5%, 60%]

3. 해석은 아래와 같이 할 수있다.
   - buys(x,"A항목, B항목")->buys(x,"C항목") [16.7%, 84.425%]
   - A항목, B항목, C항목 구매한 확률은 전체중에서 16.7%를 차지 하고 있다 (지지도 해석).
   - 신뢰도는 전항을 구매했을때, 후항을 구매할 비율 즉 A와B항목을 구매 했을때 C항목을 구매할 비율이 84.425%로 높다 라고 해석 (신뢰도 해석)
   - 전항값(left-hands side,lhs,조건)과 후항값(right-hands side,rhs,결과) 두가지로 나누어 이해 해야 한다. (A,B->C라고 표시할때, P(A,B) : lhs, P(C) : rhs로 표시  )

4. 용어의 해석

   - 8개 바스켓에서 A,B,C 3개 아이템은 아래와 같이 담겨있는 경우  
  
     |#|Basket|
     |-|-|
     |1|A|
     |2|B|
     |3|C|
     |4|A,B|
     |5|A,C|
     |6|B,C|
     |7|A,B,C|
     |8|A,B,C|
   - 기본 확률은 아래와 같이 표시할 수 있다.

     |#|A->B|(A,B)->B|
     |-|-|-|
     |LHS|P(A) = 5/8 = 0.625|P(A,B) = 3/8 = 0.375|
     |RHS|P(B) = 5/8 = 0.625|P(C) = 5/8 = 0.625|
     |Coverage|LHS = 0.625|LHS = 0.375|
     |Support|P(A∩B) = 3/8 = 0.375|P((A,B)∩C)) = 2/8 =0.25|
     |Confidence<br>(strength)|P(B\|A)=0.375/0.625=0.6|P(C\|(A,B))=0.25/0.375=0.7|
     |Lift|0.375/(0.625*0.625)=0.96|0.25/(0.375*0.625)=1.07|
     |Leverage|0.375 - 0.390 = -0.015|0.25 - 0.234 = 0.016|

     $$ 
        lift = \frac {P(A \cap B)} {P(A) \times P(B)}
      , Leverage = P(A \cap B) - P(A)\times P(B)
     $$

     leverage는 0을 기준으로 책정되는 것이며, 0 보다 크면 보완재 0 보다 작으면 경쟁재로 볼수 있다 


###  **데이터의 형태**

   |idi(identifier-item file)|itl(item list file|
   |-|-|
   |001, apples|apples, orange, banna|   
   |001, oranges|carrot|   
   |001, banna||   
   |002, carrot||

   itl 형태를 주로 사용한다