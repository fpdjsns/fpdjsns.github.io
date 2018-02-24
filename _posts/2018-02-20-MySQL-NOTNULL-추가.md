---
title : MySQL NOT NULL 추가
layout: post
---

MySQL에서 열의 한 속성을 변경할 수는 없으며 속성들의 설정값들을 다시 정의해야한다.

```sql
ALTER TABLE [테이블이름] 
MODIFY [속성명] [속성타입] NOT NULL DEFAULT [지정하고 싶은 디폴트 값]
```

EX)
```sql
ALTER TABLE mail 
MODIFY is_cc tinyint NOT NULL DEFAULT 0; 
```
---  
참고 : <https://code.i-harness.com/ko/q/9beb0c>