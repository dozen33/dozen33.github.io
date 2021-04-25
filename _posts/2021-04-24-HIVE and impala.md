---
layout: post
title:  "HIVE and impala"
date:   2021-04-24
excerpt: "HIVE and impala"

comments: false
---

# HIVE
* 데이터 웨어하우징용 솔루션
* 유사 ANSI SQL을 사용할 수 있어 기존 분석가가 쉽게 사용할 수 있음
*  Data Warehouse 시스템보다  sql실행에 오래걸림 
    * why? SQL을 MapReduce 작업으로 변환하는 작업 때문에
* java 

# impala
* 하둡 기반의 데이터를 SQL을 사용해서 실시간 질의를 가능하게 해주는 시스템
*  맵-리듀스 같은 별도 계층을 거치지 않고 HDFS(Hadoop Distributed File System)와 직접 통신
* C++ 

#  실시간성
* HIVE : MapReduce 프레임워크 이용
* Impala : 클러스터 내 모든 데이터 노드에 설치된 고유 분산 질의 엔진을 사용하여 응담시간 최소화


# impala 호환 가능 파일 포멧
* Avro, Parquet, Text, RCFile, SequenceFile 등 대부분의 파일 형식을 사용할 수 있음

# 비교
* 최대 차이는 성능
* 비교할 수 없는 속도 차이

| 구분 |HIVE |impala | 
|:--------|:-------:|--------:|
|mepreduce | 따름 | 따르지않음|
|From| 필요 |없어도 쿼리 가능|
|숫자타입간 암묵적 형변환|불가능|가능|
|문자열타임스태프간 암묵적 형변환 | 불가능 | 가능|
