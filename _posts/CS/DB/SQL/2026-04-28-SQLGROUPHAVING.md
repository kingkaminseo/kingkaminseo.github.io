---
title: SQL GROUP BY와 HAVING 공부하기
date: 2026-04-28
categories: [CS, SQL]
tags: [cs, sql]
---

## GROUP BY란 무엇인가?
SQL에서 GROUP BY는 특정 컬럼을 기준으로 데이터를 그룹화 시켜주는 역할을 한다.  

### GROUP BY 특징
- 개별 행들이 뭉쳐지기 때문에 주로 집계함수와 같이 사용된다.  
- COUNT, SUM, AVG 

### GROUP BY 예
```sql
SELECT job, SUM(sal) AS total_sal
FROM emp
GROUP BY job;
```
emp 테이블에서 job(직무)별로 데이터를 묶고, 각 직무별 sal(급여) 합계를 계산하는 예제이다.


## HAVING

HAVING은 GROUP BY로 묶인 결과물에 대해 조건을 걸 때 사용한다.  
그럼 HAVING 말고 WHERE을 사용할 순 없을까?
  
GROUP BY를 사용할 때 HAVING과 WHERE에는 명확한 차이가 있어 WHERE로 대체할 수 없다.

|구분|WHERE|HAVING|
|--------|--------------|----------------|
|적용 대상|개별 행 (Individual Rows)|그룹화된 결과 (Grouped Rows)|
|사용 시점|데이터가 그룹화되기 전|데이터가 그룹화된 후|


### HAVING 예시
```sql
SELECT deptno, AVG(sal) AS avg_sal
FROM emp
GROUP BY deptno
HAVING AVG(sal) >= 2000;
```

부서별 평균 급여가 2,000 이상인 부서만 조회한다.  
그룹화된 결과에 대한 조건은 WHERE가 아닌 HAVING을 사용하였다.  
이렇게 보니 HAVING과 WHERE의 차이를 명확히 알 수 있었다.    


## 문제풀이
[LeetCode 문제](https://leetcode.com/problems/duplicate-emails/description/) 

GROUP BY와 HAVING을 공부하여 다음문제를 풀어보았다.
다음 문제는 Person 테이블에서 중복되는 이메일만 출력하는 문제였다.

```sql
SELECT email AS Email
FROM Person
GROUP BY email
HAVING COUNT(email) > 1;
```
다음과 같이 email로 그룹화 후 같은 email 데이터가 1개 초과라면 조회하도록 하였다.
