---
title: ON / WHERE 의 차이
date: 2026-04-17
categories: [CS, SQL]
tags: [sql, cs]
---

## ON / WHERE의 차이?

ON과 WHERE는 둘 다 조건을 지정할 때 사용되지만 적용되는 시점과 역할이 다르다.

- ON: JOIN을 수행할 때 두 테이블을 어떤 조건으로 연결할지 결정
- WHERE: JOIN이 끝난 결과에서 원하는 행만 필터링
기본 개념

### ON

테이블을 어떻게 연결할지 결정하는 조건
JOIN 과정에서 사용됨

### WHERE

조회 결과에서 조건에 맞는 데이터만 남김
JOIN 이후에 적용됨
예시

Customers 테이블과 Orders 테이블이 있다고 가정한다.
각 고객의 주문 정보를 조회하려고 한다.

## ON에서 조건을 주는 경우
```sql
SELECT c.CustomerName, o.OrderID
FROM Customers c
LEFT JOIN Orders o
ON c.CustomerID = o.CustomerID;
```

고객과 주문을 조인할 때 Customers테이블의 CustomerID와 Orders테이블의 CustomerID가 일치하는 경우에만 연결한다.  
주문이 없거나 조건을 만족하지 않으면 **OrderID는 NULL** 로 표시된다.

## WHERE에서 조건을 주는 경우
```sql
SELECT c.CustomerName, o.OrderID
FROM Customers c
LEFT JOIN Orders o
ON c.CustomerID = o.CustomerID
WHERE o.OrderID > 100;
```

JOIN을 먼저 수행한 뒤 OrderID가 100보다 큰 행만 필터링한다.  
**조건을 만족하지 않는 행** 은 제거된다.  


## 정리
- ON: 테이블을 어떻게 연결할지 결정하는 조건
  - JOIN방식을 유지하면서 연결
- WHERE: 연결된 결과에서 데이터를 걸러내는 조건
  - JOIN된 최종결과 자체를 필터링
