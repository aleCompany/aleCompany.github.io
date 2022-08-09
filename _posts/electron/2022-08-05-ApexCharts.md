---
layout: single
title:  "ApexCharts 모듈 사용 (electron , Vue)"
categories: electron
tag: [ApexCharts, electron, Vue]
toc: true
author_profile: false
---
ApexCharts 이란 ? <br>
Chart를 그리는 라이브러리로 Vue, React 등 다양한 환경에 사용 가능한 라이브러임

### Electron Vue 환경에서 설치방법
```
> npm install --save apexcharts
> npm install --save vue-apexcharts
```

### Electron Vue에서 ApexCharts 사용방법

#### 1. src/main.js 에서 import

```
// main.js에 추가한다.

import VueApexCharts from 'vue-apexcharts'
Vue.use(VueApexCharts)

Vue.component('apexchart', VueApexCharts)

```

#### 2. 다양한 그래프를 그려봄

2.1 막대 그래프 예제

    ```
    // template 부분.
    <template>
        <apexcharts width="100%" height="350" type="bar" :options="chartOptions" :series="series"/>
    </template>

    // Script , Data 부분.
    data: function () {
        return {
        chartOptions: {
            chart: { id: 'basic-bar' },
            xaxis: { categories: [2015, 2016, 2017, 2018, 2019, 2020, 2021, 2022 ]
            }
        },
        series: [ { name: 'series-1', data: [38, 42, 41, 56, 50, 78, 70, 91] }]
        }
    },
    ```

2.2 캔들차트

    ```
    // template 부분.
    <template>
        <apexcharts width="100%" height="350" type="candlestick" :options="chartOptions" :series="series"/>
    </template>

    // Script , Data 부분.

    // [{ x: date, y: [Open,High,Low,Close] }] 또는
    // [Timestamp, O, H, L, C] 또는
    // [[Timestamp], [O, H, L, C]]

    data: function () {
        return {
        chartOptions: {
            chart: {
            type: 'candlestick',
            },
            title: {
            text: 'CandleStick Chart',
            align: 'left'
            },
            xaxis: {
            type: 'datetime'
            },
            yaxis: {
            tooltip: {
                enabled: true
            }
            }
        },
        series: [{
            data: [{
            x: new Date(1538778600000),
            y: [6629.81, 6650.5, 6623.04, 6633.33]
            },
            {
            x: new Date(1538780400000),
            y: [6632.01, 6643.59, 6620, 6630.11]
            },
            {
            x: new Date(1538782200000),
            y: [6630.71, 6648.95, 6623.34, 6635.65]
            }, // ...
            {
            x: new Date(1538881200000),
            y: [6603.5, 6603.99, 6597.5, 6603.86]
            },
            {
            x: new Date(1538883000000),
            y: [6603.85, 6605, 6600, 6604.07]
            },
            {
            x: new Date(1538884800000),
            y: [6604.98, 6606, 6604.07, 6606]
            }
            ]
        }]
        }
    },
    ```