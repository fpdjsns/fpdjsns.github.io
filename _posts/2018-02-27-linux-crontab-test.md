---
title : 리눅스 crontab 테스트
layout: post
---

리눅스에서 crontab이 제대로 작동하는지 테스트하기 위함  


## /home/scripts 위치에 crontab_test.sh 생성  
> 파일 위치 : /home/scripts  
> 테스트하고자 하는 파일 이름 : crontab_test.sh  


## crontab_test.sh에 작성  
```script
echo "Hello" >> /home/scripts/hello.txt  
```
Hello를 /home/scripts/hello.txt에 저장하겠다는 의미  
/home/scripts 위치에 hello.txt 파일 생성  

```script
$chmod 755 crontab_test.sh
```
권한 때문에 실행 안될수도 있으므로 권한 변경 추가  

## crontab에 crontab_test.sh 등록  

```script
$crontab -e
```

```script
#매분 실행 
* * * * * sh /home/scripts/automatic_restart.sh
```

## 크론탭 등록되었는지 확인
```script
$crontab -l
```

## 크론탭 실행되었는지 확인  

```script
cat hello.txt
```  
1분 뒤 hello.txt 찍어봄

Hello 출력되면 성공