---
title : SQL UPDATE 조건문
layout: post
---

```SQL
UPDATE 테이블 이름  
SET 속성 = IF(조건, yes인 경우, no인경우)
WHERE where조건;
```
이런 식으로 **IF(조건, 조건에 대해 yes인 경우 값, 조건에 대해 no인 경우 값)**
으로 들어간다.

```SQL
UPDATE Mail 
SET `delete` = IF(delete = 0, 1, 0) 
WHERE `no` = "t1";
``` 

---  

참고  

<https://www.w3schools.com/sql/sql_update.asp>