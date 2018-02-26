---
title : ResponseEntity 반환
layout: post
---

```java
public boolean sendMail(Mail holder, String recipient, boolean isCc, MultipartFile[] files) {
    try {
      return true;
    } catch (Exception e) {
   return false;
    }
  }
```

서비스 생성 시 try~catch 문으로 통신 성공을 판단하면 쿼리문이 제대로 작동하지 않더라도 클라이언트쪽에서는 통신 성공(status 200)으로 인식하게 되므로(data는 없음) ResponseEntity를 반환해줌으로서 에러를 확실하게 보이게 한다.  

```java
private ResponseEntity<SimpleResult> star(@RequestParam("no") String no, @RequestParam("isStarred") boolean isStarred){

    mailService.updateStarred(no, isStarred);

    SimpleResult result = new SimpleResult();
    result.setStatus(true);
        
    return new ResponseEntity<SimpleResult>(result, HttpStatus.OK);
  }
```