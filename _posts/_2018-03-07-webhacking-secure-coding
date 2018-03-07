---
title : 성능 테스트
layout: post
---
## 웹 해킹이 쉬운 이유
- 80 port가 항상 오픈되어 있음.   
- 해킹 예제와 싼 툴이 많다.  
- 공격의 반응이 바로 온다.  

유효성 체크는 클라이언트와 서버 측 둘 다 에서 해줘야 한다.  
클라이언트에서 유효성체크에 걸러져 서버측으로 들어올 수 없는 값이라도 해커들이 임의로 값을 개조하여 서버측으로 보낼 수 있기 때문이다.  

## HTTP response code
- 1xx (정보 전송) : 실험적 용도를 제외하고는 서버 측 응답 없음.
- 2xx (성공) : 요청이 성공적으로 이루어짐.  
- 3xx (리다이렉션) : 요청을 마치기 위해 클라이언트의 추가 동작이 필요.  
- 4xx (클라이언트 측 에러) : 클라이언트 측에 에러가 있는 경우.
- 5xx (서버 측 에러) : 서버가 요청을 수행하지 못함. -> 해커가 자신의 공격이 먹혔는지 알 수 있다.  

참고 : [MDN HTTP 상태 코드][1]  

## Cookie, Session
- Cookie : 클라이언트 측에 정보를 저장. 생성 시 만료시기 정함. 용량 제한 有. ex) 장바구니
- Session : 서버 측에 정보를 저장. cookie 바탕으로 동작. ex) 로그인

## 피싱(phishing)  
네이버나 구글 등과 같은 사이트들과 유사한 사이트를 만들어서 그것인 양 하는 것. 해당 사이트로 로그인을 하게해서 로그인과 비밀번호를 알아낸다.  

## XSS(Cross Site Scripting) 취약점  
작성된 스크립트가 서버를 통해 다른 사용자에게 전달되어 실행되는 것.   
다른 사람의 쿠키 값을 해커의 IP주소로 전달할 수 있다.   
참고 : [네이버 지식백과 - XSS 취약점][2]

## 웹 해킹 툴
- Fiddler  
    웹 디버깅 툴.  
  <https://www.telerik.com/download/fiddler>  
- Burp Suite  
    프록시 툴. 패킷 조작 가능.  
<https://portswigger.net/burp>  
- Cooxie Toolbar  
    cookie 값 확인, 변조 가능.  
  <http://download.cnet.com/Cooxie-Toolbar-for-Microsoft-Internet-Explorer/3000-2144_4-10268044.html>

## 웹 해킹 기법

1. SQL Injection  
입력창, 주소창 등에 SQL문장을 삽입하여 악의적인 공격 가능.  
ex) id 창에 **'or 1=1--** 입력 시 '이 쿼리문에 있었을 터인 id=\'을 닫아주고, **1=1** 때문에 항상 참이된다. 그리고 --로 인해 뒷 부분은 모두 주석처리가 되어서 최상위에 있는 ID(보통 admin계정)으로 로그인 된다.  
- 해결방법 : PreparedStatement 사용. 바인딩 변수 이용. 바인딩 변수 이용 시 쿼리 문법 처리과정이 선 수행되어 SQL 문법적 의미를 가질 수 없어 SQL인젝션을 방지해준다.  
    ex) SELECT content FROM user WHERE ID=?
    
참고 : [Prepared Statement를 쓰면 SQL 인젝션 공격이 되지 않는 이유는?][3]


[1]: https://developer.mozilla.org/ko/docs/Web/HTTP/Status  
[2]: http://terms.naver.com/entry.nhn?docId=3431916&cid=58437&categoryId=58437
[3]: https://blog.naver.com/skinfosec2000/220482240245
