```javascript
var changeDateFormat = function(originalDate) {
  var javaDateList = originalDate.split(' ');
  var jsDate = new Date(javaDateList[1] + " " + javaDateList[2] + " " + javaDateList[5] + " " + javaDateList[3])
  return jsDate;
}
```
```javascript
console.log(mailData.readDate);
            mailData.readDate = changeDateFormat(mailData.readDate);
      
            console.log(mailData.readDate);
```
Mon Jan 29 17:48:51 KST 2018  
->  Mon Jan 29 2018 17:48:51 GMT+0900 (대한민국 표준시)

보기에는 달라진게 별로없지만 자료형이 문자열에서 javascript에서 가공할수있는 DATE형식으로변경됨

참고 : <https://msdn.microsoft.com/ko-kr/library/ff743760(v=vs.94).aspx>
