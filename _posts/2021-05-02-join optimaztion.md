---
layout: post
title:  "HiveQL 최적화"
date:   2021-05-02
excerpt: "HiveQL 최적화"

comments: false
---
#  HiveQL 성능을 최적화를 위한 방법
테이블 > 파티션 > 버킷
점차 데이터를 세분화 하는 것

* partition : 열 또는 파티션 키를 기반으로 동일한 유형의 데이터를 함께 그룹화하기 위해 테이블을 파티션으로 구성
* buket : 파티션 혹은 테이블에 열의 해시 함수를 기반으로 추가적인 구조를 제공하여 데이터를 세분화하는 것

## 1. Partition
* HDFS 테이블 디렉토리의 하위 디렉토리에 데이터를 저장
* 구분
|Static (정적) | Dynamic (동적)
   |:--------:|:--------:|
|insert문에 고정값을 전달 | insert문에 조회하는 컬럼 전달
|테이블의 큰 파일 로드시 적절| 대용량 데이터가 저장된 경우 적절
|파티션 변경 가능 | 파티션 변경 불가
||파티셔닝 되지 않는 테이블을 로드하는 것

```sql
--정적
INSERT INTO TABLE db_name.table_name(yyyymmdd='20210503')
             SELECT col1 FROM db_name.table_tmp;        
--동적
INSERT INTO TABLE db_name.table_name(yyyymmdd)
             SELECT col1 FROM db_name.table_tmp;                   
```

## 2. Bucket 버켓
* HDFS에서 분리된 파일로 데이터를 저장
* 특정 컬럼을 버켓컬럼 으로 사용하며, 해당 컬럼의 값은 사용자 정의 숫자로 컬럼에 해쉬처리 <br>
동일한 ID를 가진 레코드는 항상 동일한 버킷에 저장
* 버켓의 갯수 : 2의 N제곱 개 (추천)<br>
블록 크기가 256MB이면 각 버킷에 512MB 데이터 할당 가능

```sql
--버킷 생성
CREATE TABLE db_name.table_name
PARTITIONED BY (col1 string,
                col2 int,
                ...,
                col4 string)
CLUSTERED BY (col4, col5, …)        -- bucketing 하려는 컬럼
SORTED BY (col1 [ASC|DESC], …)]
INTO 4 BUCKETS;                     -- 버켓의 개수
```

