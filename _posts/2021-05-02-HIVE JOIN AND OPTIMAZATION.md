---
layout: post
title:  "HIVE JOIN AND OPTIMAZATION"
date:   2021-05-02
excerpt: "HIVE JOIN AND OPTIMAZATION"

comments: false
---
# HIVE JOIN
EQUI-JOIN(동등조인)만 제공

* 내부조인(INNER JOIN)의 경우 조인하는 모든 테이블에서 일치하는 레코드만 반환
* ON절에서 OR조건 허용 안함


# HIVE의 조인 처리
* 조인할 각각의 쌍에 별도의 맵리듀스 잡 사용
* 왼쪽에서 오른쪽 방향으로 쿼리 처리
* 3개 이상 조인시, 모든 on절에 같은 조인키 사용시 맵리듀스 잡은 1개
* 가장 큰 테이블을 마지막에 위치하도록 해야 함<br>
why? 조인시 마지막 테이블이 가장 크다고 가정하여 다른 테이블을 버퍼링하려고 시도하고 각 레코드에 대해서 조인을 수행하면서 마지막 테이블을 흘려보냄
* outer join의 경우 파티션 필터 무시

# 조인 방식 
1. Map-side joins
* join(큰테이블, 작은테이블)
* Map, Reduce에서 Map side에서 조인한 것
* 작은 테이블을 메모리에 올리고 큰 테이블을 붙이는 것

2. SMB(Sort Merge Bucket)
* join(큰테이블, 작은테이블)
* 조인할 키를 bucket으로 만들어서 통째로 조인
* 순서 유지 안됨

