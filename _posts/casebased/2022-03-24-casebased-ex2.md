---
layout: single
title:  "사례기반추론 예제 - 2"
categories: casebased
tag: [사례기반추론 예제]
toc: true
author_profile: false
---

간단한 예제를 R을 통하여 알아본다

### 데이터 설명

* 기존데이터 : [파일첨부](../../images/2022-03-24-casebased-ex2/old_data.csv)
  
* 신규데이터 : [파일첨부](../../images/2022-03-24-casebased-ex2/new_data.csv)


### R분석

* 분석
  
    ```{R}

    # 1) Old Case Read
    oCR = read.csv('old_data.csv', header = T , stringsAsFactors=F )

    # 2) New Case read
    nCR = read.csv('new_data.csv', header = T , stringsAsFactors=F )

    # 3) 가중치부여여
    wg_list = list(wg_n1=0.05, wg_n2=0.05, wg_n3=0.15, wg_n4=0.15, wg_n5=0.20, wg_n6=0.15, wg_c1=0.10 , wg_c2=0.15)

    # 4) 거리 및유사도 계산 - 사용자 함수
    df = cbrUsrFunction(oCR, nCR , getDistVar)

    # 5) 결과
    df[df$sel_sim=='bestSimularity',]

    ```
    ※ 위에서 사용된 사용자함수 링크 [link text](2022-03-24-casebased-ex1.md)

* 결과 - 기존과 가장 유사사례

    ```
        x.n1 y.n1 diff.n1 x.n2 y.n2 diff.n2 x.n3 y.n3 diff.n3 x.n4 y.n4     diff.n4 x.n5 y.n5 diff.n5 x.n6 y.n6 diff.n6 x.c1 y.c1 diff.c1 x.c2 y.c2 diff.c2
    1   0.00 0.00    0.00 0.23 0.23       0 0.23 0.23       0 0.55 0.54 0.002307692 0.04 0.04       0 0.00 0.00       0    f    f       0    1    1       0
    12  0.10 0.00    0.01 0.38 0.38       0 0.38 0.38       0 0.55 0.54 0.002307692 0.06 0.06       0 0.06 0.06       0    m    m       0    1    1       0
    23  0.00 0.00    0.00 0.08 0.08       0 0.08 0.08       0 0.55 0.54 0.002307692 0.48 0.48       0 0.04 0.04       0    f    f       0    1    1       0
    34  0.25 0.25    0.00 0.23 0.23       0 0.38 0.38       0 0.50 0.50 0.000000000 0.34 0.34       0 0.09 0.09       0    f    f       0    1    1       0
    45  0.00 0.00    0.00 1.00 1.00       0 1.00 1.00       0 0.55 0.54 0.002307692 0.33 0.33       0 0.01 0.01       0    m    m       0    1    1       0
    56  0.10 0.00    0.01 1.00 1.00       0 1.00 1.00       0 0.55 0.54 0.002307692 0.33 0.33       0 0.01 0.01       0    f    f       0    1    1       0
    67  0.25 0.25    0.00 0.46 0.46       0 0.46 0.46       0 0.53 0.53 0.000000000 0.19 0.19       0 0.08 0.08       0    f    f       0    1    1       0
    78  0.00 0.00    0.00 1.00 1.00       0 1.00 1.00       0 0.65 0.65 0.000000000 0.19 0.19       0 0.00 0.00       0    m    m       0    1    1       0
    89  0.00 0.00    0.00 0.62 0.62       0 0.62 0.62       0 0.53 0.53 0.000000000 0.19 0.19       0 0.00 0.00       0    m    m       0    1    1       0
    100 0.10 0.00    0.01 0.62 0.62       0 0.69 0.69       0 0.55 0.54 0.002307692 0.32 0.32       0 0.10 0.10       0    m    m       0    1    1       0
        x.target y.target  diff.total Simularity x_idx y_idx        sel_sim
    1          0        0 0.002307692  0.9976976    o1    n1 bestSimularity
    12         0        0 0.012307692  0.9878419    o2    n2 bestSimularity
    23         0        0 0.002307692  0.9976976    o3    n3 bestSimularity
    34         0        0 0.000000000  1.0000000    o4    n4 bestSimularity
    45         0        0 0.002307692  0.9976976    o5    n5 bestSimularity
    56         0        0 0.012307692  0.9878419    o6    n6 bestSimularity
    67         1        1 0.000000000  1.0000000    o7    n7 bestSimularity
    78         0        0 0.000000000  1.0000000    o8    n8 bestSimularity
    89         0        0 0.000000000  1.0000000    o9    n9 bestSimularity
    100        0        0 0.012307692  0.9878419   o10   n10 bestSimularity
    ```

