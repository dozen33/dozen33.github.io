---
layout: post
title:  "HIVE and impala"
date:   2021-04-24
excerpt: "HIVE and impala"

comments: false
---
# HIVE AND impala
* HIVE : 데이터 웨어하우징용 솔루션
* impala : 하둡 기반의 데이터를 SQL을 사용해서 실시간 질의를 가능하게 해주는 시스템

#  실시간성
* HIVE : MapReduce 프레임워크 이용
* Impala : 클러스터 내 모든 데이터 노드에 설치된 고유 분산 질의 엔진을 사용하여 응담시간 최소화

# impala 호환 가능 파일 포멧
* Avro, Parquet, Text, RCFile, SequenceFile 등 대부분의 파일 형식을 사용할 수 있음
* 
# 비교
| 구분 |HIVE |impala | 
|:--------|:-------:|--------:|
|mepreduce | 따름 | 따르지않음
|From| 필요 |없어도 쿼리 가능
|숫자타입간 암묵적 형변환|불가능|가능
|문자열타임스태프간 암묵적 형변환 | 불가능 | 가능
