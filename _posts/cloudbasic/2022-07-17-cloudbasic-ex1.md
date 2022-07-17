---
layout: single
title:  "클라우드 컴퓨팅 개념"
categories: cloudbasic
tag: [클라우드 컴퓨팅]
toc: true
author_profile: false
---

클라우드 컴퓨팅은 현재 가장 Hot 기술로, 기업의 사업본질의 집중과 효율성 극대화를 위해 도입

### 개념
* 컴퓨팅 자원을 원하는 만큼 상용 하고 요금을 지불하는 형태
* 컴퓨터의 리소를 서비스 형태로 제공 받는것
* 자원을 수요에 탄력적 으로 사용 가능 하게 됨

### 4가지 특징
* On Demand ( Resource에 대한 On demand )
  * 사용한 만큼만 지불 하기 때문에 초기비용이 낮아서 Risk를 줄일 수 있다
* 대규모 확장성
* 종량제 과금
* 관리의 편리성

### Big Data Tech Stack

#### Data Store
  * Scale - up : 하드웨어 스펙을 상향하는 것 , Scale - out : 동일한 스펙을 여러개 붙이는 것
  * 최근 단일 RDBMS에서 저장이 불가능 수준의 Big Data 때문에 HDFS 기술이 발전됨(Hadoop Distributed File System)
  * NoSql : Document Base, Key/Value, Column Orinted, Graph 형태의 저장 기술
    * RDBMS에서는 Scale-out이 어렵다
    * NoSql은 태생적으로 Scale-Out이 가능
    * Ex) Aws DynamoDB , Cassendra, Cloud Spanner 등...

#### Data Processing  & Analytics
  * Data Processing
    * 전처리, Data Type 변환
    * 실시간 처리 적용 등
  * Data Analytics
    * Data로 부터 의미, 기치를 추출
  * Hadoop MapReduce : Big Data를 Split 하여 HW에 나눠서  처리하게 됨
    * MapReduce에서 코딩으로 구현하면 어렵기 때문에 Hive, Pig 등이 나오게 됨(SQL과 유사)
  * 대용량 기술을 위한 처리 언어등 : SQL , R, Python 등 (SQL을 많이 사용)

#### Data Visualization
  * Matplot, Seaborn 등
  * 오픈소스 라이브라러 직접 구현(D3.js 등), 무려 시각화(Kibana, Grafana), 상용 시작화(Redash, Tableau)

#### Data Ingestion (Collection)
  * Data가 발생하는 다수의 지점으로 부터 Data를 원하는 저장소로 옮기는 작업
  * Flumne : File 데이터를 HDFS로 적재
  * Sqoop : RDBMS의 데이터를 HDFS로 적재 
  * Kafka : 분산 Data스트리밍 플랫폼, 처리속도가 빠름


#### Workflow Management
  * 데이터 처리/분석 작업은 여러 단계를 거치는 이를 정의, 스케쥴 하여 자동화 시키는 것
  * 예시 - Apache Oozie, Apache Airflow : WorkFlow 진행

### Big Data 와 Cloud Computing

#### 클라우드 사업자가 제공하는 하는 데이터 분석
  * Amazon S3 : 대용량의 File을 저장할 수 있는 Storage 서비스 , CSV 분석 등
  * Amazon Athena :  S3에 저장된 CSV 등의 파일 분석, 쿼리 후 스캔한 데이터에 대한 비용만 지불
  * Amazon Kinesis :  
    * 처리된 데이터를 S3, Redshift, Elasticsearch 등으로 저장 가능
  
