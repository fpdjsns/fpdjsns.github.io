---
title : shell script json 문자열 내에 변수 넣기
layout: post
---

```sh
'"$변수명"'
```
형식으로 사용  


EX)
```sh
'{
  "Userid":"fpdjsns",
  "Blog":"https://fpdjsns.github.io/"
}'
```

```sh
export USERID=fpdjsns

'{
  "Userid":"'"$USERID"'",
  "Blog":"https://'"$USERID"'.github.io/"
}'
```
위의 json 문자열에서 UserId인 fpdjsns이 중복되서 나온다.  
이를 없애기 위해 환경 변수를 사용하여 아래와 같은 코드로 변경할 수 있다.

---

참고 : [Using curl POST with variables defined in bash script functions][1]

[1]: https://stackoverflow.com/questions/17029902/using-curl-post-with-variables-defined-in-bash-script-functions

