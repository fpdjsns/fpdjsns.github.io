---
title : 리눅스 crontab 테스트
layout: post
---

리눅스에서 crontab이 제대로 작동하는지 테스트하기 위함  


## /home/scripts 위치에 crontab_test.sh 생성  
> 파일 위치 : /home/scripts  
> 테스트하고자 하는 파일 이름 : crontab_test.sh  


## crontab_test.sh에 작성  
```
echo "Hello" >> /home/scripts/hello.txt  
```
Hello를 /home/scripts/hello.txt에 저장하겠다는 의미  
/home/scripts 위치에 hello.txt 파일 생성  

```
$chmod 755 crontab_test.sh
```
권한 때문에 실행 안될수도 있으므로 권한 변경 추가  

## crontab에 crontab_test.sh 등록  

```
$crontab -e
```

```
#매분 실행 
* * * * * sh /home/scripts/automatic_restart.sh
```

## 크론탭 등록되었는지 확인
```
$crontab -l
```

## 크론탭 실행되었는지 확인  

```
cat hello.txt
```  
1분 뒤 hello.txt 찍어봄

Hello 출력되면 성공

---


참고 : [1분마다 "Hello World" hello.txt 파일에 기록하기][1]  

[1]: https://zetawiki.com/wiki/1%EB%B6%84%EB%A7%88%EB%8B%A4_%22Hello_World%22_hello.txt_%ED%8C%8C%EC%9D%BC%EC%97%90_%EA%B8%B0%EB%A1%9D%ED%95%98%EA%B8%B0