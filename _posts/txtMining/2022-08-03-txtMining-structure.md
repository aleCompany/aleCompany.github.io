---
layout: single
title:  "텍스트 문서의 변환"
categories: txtMining
tag: [DTM,TDM,문서구조]
toc: true
author_profile: false
---

텍스트 문서를 컴퓨터가 인식할 수 있는 다양한 방법에 대해서 알아 보기로 함

### 원-핫 인코딩(one-hot encoding)
* 행에서 n번째 단어가 무엇인지 알려 주는 것으로 하나의 행은 해당열이 1이고  나머지는 0으로 구성됨
* 단어가 많아지는 경우 matrix가 너무 커져서 비효율화 됨 -> 이에 대안으로 빈도정보, 의미정보를 활용
* 예제) {james, hellow, james}
<table>
  <thead>
    <th></th><th>james</th><th>hellow</th>
  </thead>
  <tbody>
    <tr><th>james</th><td>1</td><td>0</td></tr>
    <tr><th>hellow</th><td>0</td><td>1</td></tr>
    <tr><th>james</th><td>1</td><td>0</td></tr>    
  </tbody>  
</table>

### 빈도정보를 활용
#### DTM, TDM Matrix (DTM을 transepose하면 TDM)

문서에 단의 빈도를 Matrix로 표현하여 독립변수로 활용하게 됨.

<img src="../../images/2022-08-03-txtMining-structure/pic-1.png">