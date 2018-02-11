---
title : ionic 빌드 시 에러 해결
layout: post
---
```
ionic cordova build android 
```

명령어 실행 시   

```
" You have not accepted the license agreements of the following SDK component" 
```
와 같은 에러 발생  

---
에러 메시지 확인

![errorImg](./assets/posts/180211-ionic-build-error-error.PNG){: width="100%" height="100%"}{: .center}

Android SDK platform **26**  확인


![update config.xml Img](./assets/posts/180211-ionic-build-error-updateConfig.xml.PNG){: width="100%" height="100%"}{: .center}

config.xml에서 android sdk version 변경  

![Android SDK Manager Img](./assets/posts/180211-ionic-build-error-androidSdkManagerSetting.PNG){: width="100%" height="100%"}{: .center}  

android sdk manger에서  
 Appearance & Behavior > System Settings > Android SDK  
 해당하는 API Level (26) 체크하고 OK   

![success Img](./assets/posts/180211-ionic-build-error-success.PNG){: width="100%" height="100%"}{: .center}

다시 명령어 실행하면 성공하는 것을 확인할 수 있다.  

---
참고  
1. <https://forum.ionicframework.com/t/you-have-not-accepted-the-license-agreements-of-the-following-sdk-component/69570>
2. <https://developer.android.com/studio/intro/update.html#sdk-manager>  
3. <https://developer.android.com/studio/intro/update.html#download-with-gradle>  
4. <https://zetawiki.com/wiki/%EC%9C%88%EB%8F%84%EC%9A%B0_JAVA_HOME_%ED%99%98%EA%B2%BD%EB%B3%80%EC%88%98_%EC%84%A4%EC%A0%95>  