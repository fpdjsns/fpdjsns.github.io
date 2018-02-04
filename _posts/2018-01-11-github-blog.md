---
title : 깃허브 블로그
layout: post
---

<p>
Jekyll(지킬) 사용해서 깃허브 블로그 생성<br>
</p>

<p>
quotes.txt -> 헤더에서 아래에 작게 나오는 명언 모음<br>
_layouts/default.html -> 헤더에서 위에 크게 나오는(글자 써지는 애니메이션 적용된 곳) 부분 모음<br>
_layouts/default.html -> 기본 틀. footer 수정<br>
_config.yml -> author, title 정보 변경. 아직 더 수정 필요<br>
</p>

<p>
포스팅들은 _posts 폴더에서 파일을 만들어서 생성(.md/ .markdown)<br>
파일명은 (년도(4자리)-월(2자리)-일(2자리)-타이틀.md)을 지켜야 함<br>
-> ex) 2018-01-11-테스트.md<br>
</p>

<p>
하나의 포스팅 파일 머리말에<br>
---<br>
title : 포스팅 이름<br>
layout: post<br>
---<br>
넣어야 함. (YAML 형식이라고 부르는 듯 하다)<br>
</p>
