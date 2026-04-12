---
title: SQL- JOIN에 대해서 + INNER JOIN이란
date: 2025-04-12
categories: [CS, SQL]
tags: [cs, sql]
---
## SQL JOIN(조인)이란?
한 데이터베이스 내의 여러 테이블의 레코드를 조합하여 하나의 결과 테이블로 표현한 것이다.
> 조인은 테이블로 저장되거나 그 자체로 이용할 수 없는 결과 셋을 만들어낸다.
{: .prompt-tip }

## JOIN의 종류
두 테이블을 조합하면서 집합의 개념으로 여러 JOIN의 종류가 있다.
- INNER JOIN
- LEFT JOIN (LEFT OUTER JOIN)
- RIGHT JOIN (RIGHT OUTER JOIN)
- FULL JOIN (FULL OUTER JOIN)
- CROSS JOIN
- SELF JOIN

다음과 같은 조인문을 공부해보도록 하자.


## (INNER) JOIN
조인하는 테이블의 ON 절의 조건이 일치하는 결과만 출력한다.  
원래는 INNER JOIN이 맞는 표현이지만 JOIN이라고 해도 내부 SQL엔진에서 INNER JOIN으로 해석하여 실행한다고 한다.
양쪽 테이블에 모두 존재하는 데이터만 조회가 가능하다. (교집합)  
일치하지 않는 데이터는 결과에서 제외된다.

## 예시
### 테이블 예시
# TABLE Categories 
![image](/assets/img/sqljoinsutdy.png)

# TABLE Categories 
![image](/assets/img/producttable.png)

다음과 같이 Categories 테이블과 Products 테이블이 있다.   
이 두 테이블을 JOIN하여 CategoryName과 ProductName 컬럼을 하나의 테이블에서 조회하려고 한다.

### SQL 예시
```sql
SELECT p.ProductName, c.CategoryName 
FROM Categories as c join Products as p 
on c.CategoryID = p.CategoryID;
```
다음과 같이 하면 Categories 테이블과 Products 테이블에서 CategoryID가 같은 것들만 JOIN하여 ProductName과 CategoryName을 출력할 것이다.

### 예시 결과
![image](/assets/img/sqlinnerjoinresult.png)



## 정리
INNER JOIN은 두 테이블에서 ON 조건이 일치하는 데이터만 결합하여 조회하는 방법이다. 즉, 양쪽 테이블에 모두 존재하는 데이터만 결과로 출력된다.

다음 글에서는 LEFT 및 RIGHT JOIN에 대해 알아보자.
## 다음 공부
- LEFT 및 RIGHT JOIN
- 추가 +
    - on에 대해서 공부 후 정리