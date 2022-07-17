---
layout: single
title:  "AWS 서비스 소개"
categories: cloudbasic
tag: [클라우드 컴퓨팅, AWS]
toc: true
author_profile: false
---

### 빅데이터를 위한 AWS 서비스

##### 1.EMR(Elastic MapReduce)
* 대용량 데이터 분석을 위한 빅데이터 플랫폼 (Petabyte 수준 데이터 분석시)
* Apache Hadoop, Spark, Hive, HBase, Presto 사용 가능
* 수백, 수천개의  EC2 인스턴스(가상서버) 또는 컨테이너로 확장 가능 (Auto Scaling 가능)

##### 2.RedShift
* 데이터 웨어하우스 서비스
  * OLAP(Online analystical processing) 데이타베이스 ( 분석을 위한 데이타 베이스 )
  * 실시간 처리를 위한 데이터 베이스 OLTP( Online Transactional Processing )
  * 표준 SQL을 활용하여 데이터 분석 (PostgreSQL 기반)
  * 다양한 서비스들과 결합
    * 분석:EMR, 수집 : Kinesis, BI : QuickSight
  
* 데이타 분석에 중심에 있음
  
##### 3.Kinesis
* 스트림 데이터를 실시간으로 수집, 처리 위한 완전 관리형 서비스
* 로그파일, 클릭 스트림, IOT 디바이스 등 다양한 소스와 연동
* 스트림 데이터를 S3, RedShift, Elasticsearch 등으로 저장 가능
* Kinesis family
  * Kinesis Data Streams -> Kinesis Data Analystics -> Kinesis Firehose

##### 4.Athena
* S3에 저장되어 있는 데이터를 SQL로 분석하는 서버리스 서비스 (별도의 인프라 구성이 필요 없음)
* 쿼리 시 스캔한 데이터에 대해서만 비용 지불
* CSV, JSON, Avro, Parquet 등의 다양한 표준 데이터에 대한 쿼리 실행 가능


##### 5.QuickSight Dashboard
* Dashboard 형태로 쉽게 만들어 줌


##### 6.Glue
* 완전 관리형 데이터 ETL(Extract trasfer load - 추출,변환, 적재) 서비스 (OLEP를 OLAP 형태로 옮기는 기능)
* S3, RedShift 등과 통합과능


#### ML 위한 AWS 서비스

##### 1.SageMaker
* MLOps를 위한 완전 관리형 서비스
* MLOps를 파이프라인의 스테이지 별 다양한 기능 제공
  * 데이터 레이블링
  * 데이터 전 처리 코드 관리
  * 모델학습
  * 모델버전관리
  * 모델실험관리
  * 모델배포
  * 모델 마켓 플레이스

##### 2.Rekognition
* ML기반의 이미지, 비디오 분석 자동화 서비스
* 다양한 분석 기능 제공
  * 객체인식
  * 텍스트 검출
  * 얼굴 검출
  * 유명인 인식
  * 레이블링 자동화

##### 3.Transcribe / Polly
* Transcribe
  * 음성을 텍스트로 변환하는 서비스
  * ASR (Automatic Speech Recognition)
  * 한국어 지원
* Polly
  * 텍스트를 음성으로 변환하는 서비스
  * 다양한 언어(한국어 포함)와 여성, 남성 음성 제공
  * 낮은 지연 시간으로 실시간 스트리밍 가능

##### 4.Translate
* 딥러닝 기반의 기계번역 서비스

##### 5.Lex
* 챗봇 서비스
* 음성, 텍스트의 의도 인식 및 대화 흐름 관리
* 주요 단어 인식(NER)

##### 6.Comprehend
* 자연어 처리 서비스
* 기능
  * 핵심 문장 추출
  * 감성분석
  * 구문분석
  
##### 7.Forecaset
* 시계열 데이터 예측위한 ML 서비스
  
##### 8.Personalize
* ML 기반 개인 추천 서비스


#### AWS를 사용하는 3가지 방법
* AWS Management Console
* CLI - Command Line interface
* SDK - Software Development Kit
