---
layout: post
title:  "HIVE UPSERT"
date:   2021-07-13
excerpt: "HIVE UPSERT"

comments: false
---

# HIVE UPSERT

## 정의
* UPDATE와 INSERT의 조합
* 기존 테이블에 새로운 데이터를 추가할 때, 조건을 만족하는 row가 있다면 update를 하고 없다면 insert를 하는 것을 의미
* RDBMS에서는 Merge로 진행하기도 함


## 상황
- 큰 테이블(Table1)에 작은 테이블(Table2)의 데이터를 upsert해야하는 경우
- Table1과 Table2는 범주화하기에 엄청 큰 범위를 가지고 있음
- col3은 작업 날짜로 작업의 용이성을 위해 partition이 필요함
- 기존에는 PK값으로 partition을 만들어서 진행했지만, col3 외에는 partition을 적용하지 않고 진행해야 함
![pk_table](/assets\img/post_img/pk_table.jpg)

## 기존 해결 방식
-- 작업 순서 설명을 위해 간략히 작성

``` sql
create if not exists ods_table    -- 새로 추가하는 테이블
create if not exists stg_table    -- 작업용 테이블
create if not exists svc_table    -- upsert 완료될 테이블

truncate table stg_table 

insert stg_table        
    select * 
    from svc_table 
    where svc_table.pk1 not exists (select * from ods_table)

insert stg_table 
    select * from ods_table

ALTER TABLE svc_table RENAME TO svc_table_back;
ALTER TABLE stg_table RENAME TO svc_table;

drop svc_table_back
```
![pk_table2](/assets\img/post_img/pk_table2.jpg)

## 해결 아이디어
1. PK 파티션을 대신할 수 있는 테이블을 생성







