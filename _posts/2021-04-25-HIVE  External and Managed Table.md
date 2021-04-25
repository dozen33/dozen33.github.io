---
layout: post
title:  "HIVE - External and Managed Table"
date:   2021-04-25
excerpt: "HIVE - External and Managed Table"

comments: false
---
# HIVE table
* HDFS 상에 저장된 파일과 디렉토리 구조에 대한 메타 정보

 

   | 구분  | EXTERNAL | MANAGED |
   |:--------:|:--------:|:-------:| 
   | 데이터 | 관리 X|소유 |
   |querry chaing | 미존재| 존재|
   | ACID|  X|O|
   | TRUNCATE|  X|O|
   
 <br>

# External Table
* 테이블의 형태만 존재하며, HDFS의 위치를 Location으로 잡아 해당 위치에서 데이터를 직접 부름
* drop시 파일 유지되므로 파일 삭제와 같은 휴먼 에러 방지하기 좋음
* QUERRY CACHING 미존재


``` sql
CREATE EXTERNAL TABLE IF NOT EXISTS db_name.table_name        --테이블 명
 (
    col1 string,                 --컬럼 선언
    col2 int
 )
 PARTITIONED BY (col3 String)    --파티션 설정
 ROW FORMAT DELIMITED            --라인 및 필드 설정
 FIELDS TERMINATED BY ','
 STORED AS FILE_FORMAT           --저장옵션
 OUTPUTFORMAT
 LOCATION '/path';               --파일 경로
```
## ROW FORMAT
* delimeter 지정 : 컬럼 단위
   ```sql
   ROW FORMAT DELIMITED FIELDS TERMINATED BY 'char'   -- , /t " 
   ```
* SerDe(서데) : 데이터를 해석하는 방법

   ```sql
   ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.RegexSerDe'    --정규식
   ROW FORMAT SERDE 'org.apache.hive.hcatalog.data.JsonSerDe'     --JSO
   ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'   --CSV
   ```
## FILE_FORMAT 
   * TEXTFILE(default)
   * CFILE
   * ORC
   * PARQUET
   * AVRO

<br>

# PARTITIONED BY
   * 폴더 구조로 데이터 분할하여 저장
   * Full Scan 방지
      ``` sql
      ALTER TABLE db_name.table_name 
      ADD IF NOT EXISTS PARTITIONED BY ( col3 ='value')
      location '/path/col3=value'
      ```


# Manged Table
* HIVE 기본 테이블
* 테이블 및 파티션 삭제 시 테이블 및 관련된 메타데이터가 HDFS에서 삭제
* ACID 및 TRANACTION 작동
* QUERRY CACHING 존재
* 데이터 insert 필요



# HIVE IMPALA MEATSOTRE 동기화
* IMPALA와 HIVE는 HIVE METADATA 공유
* HIVE 메타데이터 변경시 IMPALA에 적용 필요
* 하기 코드 입력 혹은 HUE에서는 캐시 증분 버튼 존재
``` sql
 INVALIDATE METADATA db_name.table_name;
 REFRESH db_name.table_name;     --COST가 더 적은 코드
 ```