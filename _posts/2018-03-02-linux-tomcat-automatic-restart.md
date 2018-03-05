---
title : 리눅스 톰캣 자동 재기동
layout: post
---

### 톰캣 동작 확인

### automatic_restart.sh
```sh
#!/bin/sh

#해당 스크립트 실행을 위한 파일 저장 위치. (+ shutdown.sh, start.sh가 저장된 폴더 위치.) 
export DIR=/user/scripts
#상태 체크하기 위한 페이지 다운한 파일 저장되는 위치
export checkFile=$DIR/l7check
#현재 시간
export nowDate=`date +%Y-%m-%d%t%H:%M`
#모니터링 하고자 하는 IP주소
export IP=[확인하고자 하는 IP주소 적어줌]
#상태 체크하기 위한 페이지 주소
export URL=http://$IP/monitor/l7check

#재시작
restart_server() {
        echo "[crontab][$nowDate] health check fail"
        echo "[crontab][$nowDate] restart server"

        #톰캣, 아파치 종료 후 재실행
        sh $DIR/shutdown.sh
        sleep 10
        sh $DIR/start.sh
}

#1. 살아있나 확인하기 위한 페이지 다운
cd $DIR
wget $URL

#2. 파일 유무 확인
if [ -r "$checkFile" ] ; then
        echo "[crontab][$nowDate] wget success"
        #3. 다운받은 페이지 내용 저장
        export file_content="`cat $checkFile`"
        #4. 사용이 끝난 다운받은 파일 삭제
        rm -rf $checkFile

        #5. 페이지 내용이 출력되어야 하는 내용과 같은지 확인(4xx, 5xx 에러 확인)
        if [ $file_content != "ok" ] ; then
                restart_server
        else
                echo "[crontab][$nowDate] health check success"
        fi
else
        restart_server
fi
```

### start.sh
```sh
#tomcat startup
sh [톰캣 폴더 위치]/bin/startup.sh
#apache start
apachectl start
```

### shutdown.sh
```sh
#apache stop
apachectl stop
#tomcat shutdown
sh [톰캣 폴더 위치]/bin/shutdown.sh
```

---

### 환경변수 추가

```sh
#!/bin/sh
export APP_HOME=[app들이 저장되는 폴더 위치]

export JAVA_HOME=${APP_HOME}/jdk
export PATH=${JAVA_HOME}/bin:$PATH

export APACHE_HTTP_HOME=${APP_HOME}/apache
export PATH=${APACHE_HTTP_HOME}/bin:$PATH

export TOMCAT_HOME=${APP_HOME}/tomcat
export PATH=${TOMCAT_HOME}/bin:$PATH

 ~~~

```
Apach, Tomcat의 위치를 잘 못 찾아가서 실행이 안된다. (404에러가 발생)  
이를 방지하기 위해 스크립트 상단에 환경변수를 추가한다.  
환경변수 위치는 자신의 환경에 맞게 설정한다.

---

### lock 추가

```sh
restart_server() {
        #lock 생성
        touch $DIR/lock.pid

        sh $DIR/shutdown.sh
        sleep 10
        sh $DIR/start.sh
        #10분 안에 톰캣이 완전히 켜진다고 가정
        sleep 10m
        #lock 삭제
        rm $DIR/lock.pid
}

#lock.pid 가 있는 경우 아무일도 하지 않고 종료
if [ -e "'"$DIR"'/lock.pid" ]; then
        #locked된 상태
        exit
fi

```

톰캣이 재시작하는 경우 재시작 도중 크론탭이 돌아서 다시 재시작하는 경우를 방지하기 위해 lock을 거는 코드 추가.  

---

### crontab 추가

```sh
#매분 실행 
* * * * * sh [스크립트 저장된 폴더 위치]/automatic_restart.sh >> [로그가 저장될 폴더 위치]/automatic_restart.log 2>&1
```

참고 : [리눅스 crontab 테스트][3]

---

### 전체 소스

```sh
#!/bin/sh
export APP_HOME=[app들이 저장되는 폴더 위치]

export JAVA_HOME=${APP_HOME}/jdk
export PATH=${JAVA_HOME}/bin:$PATH

export APACHE_HTTP_HOME=${APP_HOME}/apache
export PATH=${APACHE_HTTP_HOME}/bin:$PATH

export TOMCAT_HOME=${APP_HOME}/tomcat
export PATH=${TOMCAT_HOME}/bin:$PATH

export DIR=/user/scripts
export checkFile=$DIR/l7check
export nowDate=`date +%Y-%m-%d%t%H:%M`
export IP=[확인하고자 하는 IP주소]
export URL=http://$IP/monitor/l7check


#재시작한다는 메시지 훅 날리는 함수
#굳이 할 필요는 없음. 
send_noti(){
        curl -H "Content-Type:application/json" -X POST -d'{"Name":"[이름]","Image":"[이미지]","text":"[crontab]['"$IP"'] restart server"}' [훅 날리는 주소]   
}

restart_server() {
        touch $DIR/lock.pid

        sh $DIR/shutdown.sh
        sleep 10
        sh $DIR/start.sh
        send_noti

        sleep 10m
        rm $DIR/lock.pid
}

if [ -e "'"$DIR"'/lock.pid" ]; then
        exit
fi

cd $DIR
wget $URL

if [ -r "$checkFile" ] ; then
        export file_content="`cat $checkFile`"
        rm -rf $checkFile
        if [ $file_content != "ok" ] ; then
                restart_server
        fi
else
        restart_server
fi
```

중간에 echo로 상태를 찍어봐서 log파일로 잘 돌아가는지 확인해보는 것도 좋다.

---

### 추가적으로 생각해봐야 할 것  
- lock을 10분 걸어놨는데 이 기간동안 톰캣이 켜지지 않으면 lock이 풀려서 껐다가 다시 시작함.  
- lock을 거는 적절한 기간은?
- 배포(deploy)시 크론탭이 동작하지 않아야 함. (동작한다면 배포 도중 크론탭이 작동하여 재시작할 수 있고 이 경우 예상치 못한 에러가 발생할 수 있으므로. 작은 서비스 같은 경우는 서버 하나하나 크론탭 끄고 배포하고 배포가 무사히 다 올라가서 작동도 되면 크론탭을 다시 켜면 되지만 크고 많은 서버가 존재하는 경우는 매우 불편한 일임. )  
  >**sol**) 반복문으로 계속 상태를 확인하면서 "Tomcat start" 같이 톰캣이 켜진경우 출력되는 문자열이 뜨는지 모니터링 한다. 톰캣이 시작됐다는 것을 확인하면 lock 파일을 삭제해서 lock을 풀어줌.
- health check로 톰캣이 살아있는지 판단하는 경우 더 고려할 경우가 있는가? (부작용이 존재는가?)
- health check와 프로세스 확인하는 방법 중 더 적절한 것은?
- 프로세스 확인으로 바꿨을 때 추가적으로 생각해야 되는 것은?
- wget 최대 횟수 제한 default는 20인데 적절 최대 횟수는 어느 정도인가?([참고: wget --tries][1])

---

참고  
1. [crontab을 이용한 Tomcat 서버 자동 재시작 shell][2]  
2. [Linux에서 tomcat 자동 재시작 하기][4]  

[1]: https://www.gnu.org/software/wget/manual/wget.html
[2]: http://mungchung.com/xe/lecture/5164?PHPSESSID=1ee8a44e02c41a27ae486c747a8c3b94
[3]: http://fpdjsns.github.io/linux-crontab-test
[4]: http://blog.naver.com/PostView.nhn?blogId=eyenein12&logNo=220387166296