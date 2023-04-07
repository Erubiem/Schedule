# AndroidManifest 오류와 FireBase 기능인 Auth, Message SDK를 추가하여 활성화 

---

<aside>
  
💡 **HEADER**

</aside>

---

# 개요

---

FireBase 기능인 Auth, Message SDK를 추가하여 활성화하기 및 오류 해결

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

 *  다운로드 링크 <br>
-> "https://github.com/googlesamples/google-signin-unity/releases" (google-sign 패키지) <br>
-> "https://firebase.google.com/download/unity?hl=ko" (Firebase 패키지)

---------------------------------------------------------------------------------------------------------------------------------------------

## 목차

- [1) 준비 작업](#1-------)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>
























#                                                      AndroidManifest 오류
```
### AndroidManifest 오류

 - 현재 오류가 난 경위는 이러하다.
 - SourceTree로 다른 사람의 프로젝트를 받고 그 위에 Firebase.Auth, google-Login, Firebase.Message를 Import를 했을 때 build를 했을 경우 오류가 생겼다.  

 - 밑에 유니티 에러를 써놓았다.

```

### 유니티 에러

### 1)
```C#
Building Library\Bee\artifacts\Android\Manifest\LauncherManifestDiag.txt failed with output:
System.NullReferenceException: Object reference not set to an instance of an object.
   at Unity.Android.Gradle.AndroidManifest.SetFixedWindowSize(String activity, Int32 defaultWidth, Int32 defaultHeight, Int32 minimumWidth, Int32 minimumHeight)
   at AndroidPlayerBuildProgram.Actions.GenerateManifests.PatchLibraryManifest(AndroidManifest manifest, ManifestDiagnostics diagnostics)
   at AndroidPlayerBuildProgram.Actions.GenerateManifests..ctor(Arguments arguments)
   at AndroidPlayerBuildProgram.Actions.GenerateManifests.Run(CSharpActionContext context, Arguments arguments)
UnityEngine.GUIUtility:ProcessEvent (int,intptr,bool&)
```

### 2)
```C#
BuildFailedException: Incremental Player build failed!
UnityEditor.Modules.BeeBuildPostprocessor.PostProcess (UnityEditor.Modules.BuildPostProcessArgs args) (at <2adcb7d86536472884d6a11c9ab8e115>:0)
UnityEditor.Modules.DefaultBuildPostprocessor.PostProcess (UnityEditor.Modules.BuildPostProcessArgs args, UnityEditor.BuildProperties& outProperties) (at <2adcb7d86536472884d6a11c9ab8e115>:0)
UnityEditor.Android.AndroidBuildPostprocessor.PostProcess (UnityEditor.Modules.BuildPostProcessArgs args, UnityEditor.BuildProperties& outProperties) (at <7e22973cbf66497e9da4d9832ba208e4>:0)
UnityEditor.PostprocessBuildPlayer.Postprocess (UnityEditor.BuildTargetGroup targetGroup, UnityEditor.BuildTarget target, System.Int32 subtarget, System.String installPath, System.String companyName, System.String productName, System.Int32 width, System.Int32 height, UnityEditor.BuildOptions options, UnityEditor.RuntimeClassRegistry usedClassRegistry, UnityEditor.Build.Reporting.BuildReport report) (at <2adcb7d86536472884d6a11c9ab8e115>:0)
UnityEngine.GUIUtility:ProcessEvent(Int32, IntPtr, Boolean&)
```
### 3)
```C#
Build completed with a result of 'Failed' in 2 seconds (2130 ms)
UnityEngine.GUIUtility:ProcessEvent (int,intptr,bool&)
```
### 4)
```C#
UnityEditor.BuildPlayerWindow+BuildMethodException: 3 errors
  at UnityEditor.BuildPlayerWindow+DefaultBuildMethods.BuildPlayer (UnityEditor.BuildPlayerOptions options) [0x002da] in <2adcb7d86536472884d6a11c9ab8e115>:0 
  at UnityEditor.BuildPlayerWindow.CallBuildMethods (System.Boolean askForBuildLocation, UnityEditor.BuildOptions defaultBuildOptions) [0x00080] in <2adcb7d86536472884d6a11c9ab8e115>:0 
UnityEngine.GUIUtility:ProcessEvent (int,intptr,bool&)
```

---------------------------------------------------------------------------------------------------------------------------------------------
 ```
 - 결과적으로 이런일은 Firebase.Massage를 임포트를 했을 경우 생겨나는 일이다.
 - AndroidManifest 속 activity에는 intent-filter 요소가 있으며, 이는 해당 activity가 메인 액티비티이자 런처 액티비티로 설정되어 있음을 나타내는데. 
 - 앱이 실행될 때, 원래라면 FireBase.Message를 실행되어야 된다. 그러나 다른 사람한테 Firebase Cloud Messaging에서 제공하는 액티비티로 설정되어 있지 않으므로, 앱이 실행될 때 Firebase Cloud Messaging과 관련된 작업이 정상적으로 처리되지 않아 오류가 발생된다.

 - 그리고 서로의 설정이 , SetFixedWindowSize() 관련 오류가 발생할 수 있는데,
 - 이 문제를 해결하기 위해서는 AndroidManifest.xml 파일에 다른 사람이 제작하던 프로젝트에 사용하던 코드들의 스타일 테마를 일치시키는 방향으로 코드를 수정한다.

 - 경로는 Assets/Plugins/Android/AndroidManifest.xml이고, 아래에는 예시이다.
``` 
```C#
<activity android:name="com.unity3d.player.UnityPlayerActivity"
          android:theme="@style/UnityThemeSelector"
          android:screenOrientation="landscape"
          android:resizeableActivity="false">
    <meta-data android:name="unityplayer.ForwardNativeEventsToDalvik" android:value="true" />
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
    <meta-data android:name="unityplayer.UnityActivity" android:value="true" />
    <meta-data android:name="android.max_aspect" android:value="2.1" />
    <meta-data android:name="android.support.PARENT_ACTIVITY"
               android:value="com.unity3d.player.UnityPlayerActivity" />
    <meta-data android:name="android.windowLayoutMode"
               android:value="fullscreen" />
</activity>

```
 - </activity> 밑에 맞는 맞는 설정과 코드를 추가한다면 오류가 사라지는 것을 볼 수 있다.









---------------------------------------------------------------------------------------------------------------------------------------------
## 1) 준비 작업

 - 1. 유니티 허브를 들어가서 에디터 설치를 클릭<br><br><br>
  <img src="image/Fire1.PNG" width="100%"><br>
 - 2. 현재 기준으로 정식 릴리스 칸에 없기 때문에 아카이브 칸으로 가서 아카이브 웹페이지로 이동한다. (현재 2023.04.05)<br><br><br>
  <img src="image/Fire2.PNG" width="100%"><br>
 - 3. 그리고 Unity 2022.1.24라고 쓰인 곳으로 가보면 Unity Hub라는 단추를 누르면<br><br><br>
  <img src="image/Fire3.PNG" width="100%"><br>
 - 4. 모듈 설치 선택상자에서 Android build Support 와 iOS build Support 꼭 체크하도록 한다.<br><br><br> 
  <img src="image/Fire4.PNG" width="100%"><br> 
---------------------------------------------------------------------------------------------------------------------------------------------

## 2) FireBase Unity3D 유저 인증

https://cafe.naver.com/sesisoftdev.cafe?iframe_url=/ArticleRead.nhn?articleid=20

먼저 네이버 블로그에 있는 Firebase Setting 과정을 모두 진행한다.
1부터 5번까지 진행하면 된다. 

진행했다면 아래 링크를 참고하여 Unity3d Client Build Setting 을 각자 프로젝트에 맞게 세팅한다.

https://cafe.naver.com/sesisoftdev/27

Assets 메뉴 - Import Package - Custom Package 로 다운로드 받은 firebase_unity_sdk 폴더로 간 후 firebase sdk 를 추가한다. <br>
                                                                                                         
---------------------------------------------------------------------------------------------------------------------------------------------

 - 먼저 FirebaseAuth.unitypackage를 추가할텐데
 <img src="image/Fire5.PNG" width="100%"><br> 

 - FirebaseAuth를 현재 유니티에 임포트한다.
그리고 다운받은 google-signnin-unity를 추가할텐데 붉은 색 원안에 있는 것을 다운을 받았으면. <br> <br><br>
 <img src="image/Fire6.PNG" width="100%"><br> 
 - 구글 로그인 패키지을 임포트를 해야 되는데 여기서 주의할 점이 있다. <br> <br><br>
 <img src="image/Fire7.PNG" width="100%"><br> 
 - 유니티에 뜨는 임포트 창에서 “Unity.Compat.dll”과 “Unity.Tasks.dll”을 제외 시켜준다.
그리고 임포트를 해주면 오류가 뜨는데 <br> <br><br>
 <img src="image/Fire8.PNG" width="100%"><br> 
 - 이럴 땐 “Google.VersionHandler”와 “Google.VersionHandlerImpl_v1.2.89.0”를 삭제해주면 된다. <br> <br><br>
 <img src="image/Fire9.PNG" width="100%"><br><br>
<img src="image/Fire10.PNG" width="100%"><br><br>
 - 그리고 나머지는 "https://cafe.naver.com/sesisoftdev.cafe?iframe_url=/ArticleRead.nhn?articleid=20" 여기서 말하는대로 세팅하고 테스트를 하면 되는데<br> <br><br>

<img src="image/Fire11.PNG" width="100%"><br><br>
- AnonyOfChange.cs를 다운 받고 유니티로 넣었을 때 이런 오류가 생길 수 있다.
 - 그럴 땐 8줄에 있는 Using Firebase.Unity.Editor;를 주석 처리하고 나면 또 다른 이슈가 생기는데
 - Login.cs에서 EmailCreatePanel이 없을 때 나타나는 오류다.<br> <br><br>

<img src="image/Fire12.PNG" width="100%"><br><br>
 - 이럴 때는 간단히 코드에 public GameObject EmailCreatePanel; 을 넣어준다면 해결이 가능하다.

그렇게 유저 인증을 마치게 된다면 푸시 기능으로 넘어가보자.

---------------------------------------------------------------------------------------------------------------------------------------------
## 3)	FireBase Unity3D 푸시 기능 사용

"https://cafe.naver.com/sesisoftdev/37"을 기반으로 제작한다.


