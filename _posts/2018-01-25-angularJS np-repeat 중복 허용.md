---
title : AngularJS ng-repeat 중복허용
layout: post
---

## track by $index

개발 시 ng-repeat으로 반복적으로 사람 이름을 출력하다가 에러가 발생했다.  
에러를 보니 AngularJS에서 ng-repeat에서 사용하는 리스트에 중복값이 발생하면 안되는 것 때문이었다.  
**track by $index** 를 사용하여 해결하였다.

```javascript
<태그 ng-repeat = "리스트이름 **track by $index**">
```

예를 들어
```javascript
<div ng-repeat="ListName">
//에서

<div ng-repeat="ListName track by $index">
로 변경
``` 

---  

>참고  
><https://stackoverflow.com/questions/39640160/what-is-track-by-in-angularjs-and-how-does-it-work> 
><https://docs.angularjs.org/api/ng/directive/ngRepeat>
