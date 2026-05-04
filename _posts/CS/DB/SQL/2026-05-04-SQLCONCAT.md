---
title: SQL CONCAT함수란?
date: 2026-05-04
categories: [CS, SQL]
tags: [cs, sql]
---

## SQL CONCAT함수란?
CONCAT은 Concatenate(연결시키다)의 약자로  
**둘 이상의 문자열을 하나로 합칠 때 사용하는 함수 입니다.**

## 예시
```sql
SELECT CONCAT(last_name, first_name) AS user_name
FROM users;
```
```sql
SELECT 
    product_name, 
    CONCAT(price, '원') AS 가격_표기,
    CONCAT('잔여 수량: ', stock, '개') AS 재고_현황
FROM products;
```

합치고 싶은 두 컬럼이나 문자열을 다음과 같이 `,`(쉼표)로 구분하여 간단하게 문자열을 합칠 수 있다.

## CONCAT_WS
```sql
SELECT CONCAT_WS('-', '2026', '05', '04') AS date;
```
```sql
SELECT CONCAT_WS(' ', '서울특별시', '강남구', '테헤란로 123') AS 전체주소;
-- 결과: 서울특별시 강남구 테헤란로 123
```
다음과 같이 쉽표나 공백과 같은 구분자를 문자열에 반복해서 넣어야할 경우 CONCAT을 쓰면 코드가 지저분해진다.  
이때 다음예시와 같이 `CONCAT_WS`(With Separator)를 사용하여 처리할 수 있다.  

> 만약 CONCAT 인자값으로 하나라도 NULL데이터가 들어가면 결과값 전체가 NULL이 되기에 조심하도록 하자.  
해결: IFNULL함수로 NULL데이터 기본값 지정  
{: .prompt-tip }
