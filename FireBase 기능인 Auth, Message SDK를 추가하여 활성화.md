# FireBase 기능인 Auth, Message SDK를 추가하여 활성화

---

<aside>
  
💡 **HEADER**

</aside>

---

# 개요

---

FireBase 기능인 Auth, Message SDK를 추가하여 활성화하기 

<aside>
⚠️ 작성시기 2023년 04월

</aside>

<aside>
⚠️ Visual Studio 2022, Unity에서 진행되었습니다.

</aside>

---

* 사용 목적
  1. FireBase 기능인 Auth, Message SDK를 추가하여 활성화를 하는 것과 오류를 고치기. 

* 구성 요소
    1.  FirebaseAuth.package, FirebaseMessaging.package, google-signin-plugin-1.0.4.package (최신 버전으로 준비 권장)
    2. 유니티 2022.1.24f1 버전
    3.	Android 버전으로 개발 중인 게임 프로젝트 (상관 없음)

다운로드 링크 <br>
-> "https://github.com/googlesamples/google-signin-unity/releases" (google-sign 패키지) <br>
-> "https://firebase.google.com/download/unity?hl=ko" (Firebase 패키지)


## 1) 준비 작업

 - 1. 유니티 허브를 들어가서 에디터 설치를 클릭
  <img src="image/Fire1.PNG" width="100%"><br>
 - 2. 현재 기준으로 정식 릴리스 칸에 없기 때문에 아카이브 칸으로 가서 아카이브 웹페이지로 이동한다. (현재 2023.04.05)
  <img src="image/Fire2.PNG" width="100%"><br>
 - 3. 그리고 Unity 2022.1.24라고 쓰인 곳으로 가보면 Unity Hub라는 단추를 누르면 
  <img src="image/Fire3.PNG" width="100%"><br>
 - 4. 모듈 설치 선택상자에서 Android build Support 와 iOS build Support 꼭 체크하도록 한다. 
  <img src="image/Fire4.PNG" width="100%"><br> 
---------------------------------------------------------------------------------------------------------------------------------------------

