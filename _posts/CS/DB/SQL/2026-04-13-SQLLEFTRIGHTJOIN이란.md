---
title: SQL- LEFT/RIGHT JOIN 이란
date: 2026-04-13
categories: [CS, SQL]
tags: [cs, sql]
---

## LEFT JOIN / RIGHT JOIN 이란?
한쪽 테이블의 모든 행을 기준으로, 다른 테이블에서 조건에 일치하는 행을 결합하여 반환하는 SQL 조인 방식이다.

- **LEFT JOIN***: 왼쪽(첫 번째) 테이블의 모든 행을 유지하고, 오른쪽 테이블에서 조건에 맞는 데이터를 가져온다.
- **RIGHT JOIN**: 오른쪽(두 번째) 테이블의 모든 행을 유지하고, 왼쪽 테이블에서 조건에 맞는 데이터를 가져온다.

> 조인 조건에 일치하는 데이터가 없는 경우에는 상대 테이블의 컬럼 값이 NULL로 채워진다.
{: .prompt-tip }


## 예시
### TABLE Customers 
![image](/assets/img/sqlcustomertable.png)

### TABLE Orders 
![image](/assets/img/sqlorderstable.png)

다음과 같이 Customers 테이블과 Orders 테이블이 있다.   
각 고객의 주문내역을 조회하고자 한다.   
고객이름과 주문ID를 같이 조회하고 만약 고객이 아무것도 주문하지 않았다면 NULL로 표시하고자 한다.  
이럴 경우 LEFT JOIN을 사용하여 접근할 수 있다.

### SQL 예시
```sql
SELECT c.CustomerName, o.OrderID 
FROM Customers c LEFT JOIN Orders o
on c.CustomerID = o.CustomerID;
```
다음과 같이 작성하면 Customers 테이블에 Orders 테이블을 LEFT JOIN하여 각 고객별 주문 ID를 확인할 수 있다.  
또한, 해당 고객의 ID가 Orders 테이블에 존재하지 않는 경우에는 LEFT JOIN으로 인해 주문 ID가 NULL로 표시된다.  

### 예시 결과
![image](/assets/img/sqlleftjoinresult.png)



## EXCLUSIVE LEFT JOIN
어느 특정 테이블에 레코드만 가져올 수 있다.  
별도의 EXCLUSIVE JOIN이 있지 않고 LEFT JOIN과 WHERE절의 조건을 함께 사용하여 만들 수 있다.
### 예시 코드
```sql
SELECT c.CustomerName, o.OrderID 
FROM Customers c LEFT JOIN Orders o
on c.CustomerID = o.CustomerID
WHERE o.CustomerID IS NULL;
```
다음은 이전문제에서 아무것도 주문하지 않는 고객을 조회하고 싶을 때 사용할 수 있는 SQL코드이다.
WHERE절을 사용하여 Orders테이블에 있는 CustomerID가 NULL인 것만 조인하여 조회하면 아무것도 주문하지 않는 고객만 조회할 수 있다.
### 예시 결과
![image](/assets/img/sqlexclusiveleftresult.png)


## 정리
LEFT JOIN은 기준이 되는 테이블의 모든 데이터를 유지하면서, 조건에 맞는 다른 테이블의 데이터를 함께 조회할 때 사용한다.  
조인 조건을 만족하지 않는 경우에는 상대 테이블의 값이 NULL로 표시된다.  

LEFT JOIN에 WHERE ... IS NULL 등의 조건을 추가하면 특정 테이블에만 존재하는 데이터(매칭되지 않은 데이터)만 따로 조회할 수 있다.  
