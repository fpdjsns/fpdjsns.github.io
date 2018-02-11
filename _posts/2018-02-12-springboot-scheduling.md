---
title : spring boot 스케줄링
layout: post
---

# 스케줄링

## @EnableScheduling 어노테이션

```java
@EnableScheduling
@SpringBootApplication
public class Application{
  ---
}
```  

## @Scheduled 어노테이션  

### Fixed Delay

```java
@Scheduled(fixedDelay = 1000)
public void schedulingFixedDelay(){
  System.out.println("작업이 끝난 뒤 1초 뒤 다시 실행");
}
```

### 크론 표현식  

cron = "초 분 시 일 월 년"

```java
@Scheduled(cron = "0 30 7 * * *")
public void schedulingCrontab() {
  System.out.println("매일 AM 7시 30초에 실행");
}
```

```java
@Scheduled(cron = "0 */50 * * * *")
public void schedulingCrontab() {
  System.out.println("50분 마다 실행 (단, 초는 0초일 때)");
}
```

---
참고 : <http://www.baeldung.com/spring-scheduled-tasks>
