<h1 id="firebase">Firebase</h1>
<p><strong>애플리케이션 개발자가 더 나은 애플리케이션을 개발할 수 있도록 도와주는 개발 플랫폼</strong></p>
<p>구글에서 만든 모바일 및 애플리케이션 개발 플랫폼, 다양한 플랫폼을 지원한다.
(Android, iOS 뿐만 아니라, Web, Flutter, C++, 게임, 서버 기타 등등)</p>
<hr />
<h2 id="1-firebase-제공-서비스-종류">1. Firebase 제공 서비스 종류</h2>
<ul>
<li><code>빌드</code> (개발과정, 더 빠르게 시장에 진출하고 사용자에게 가치를 전달)<ul>
<li>서버 관리 없이 백엔드 가동일반적인 앱 개발 문제를 쉽게 해결</li>
<li>일반적인 앱 개발 문제를 쉽게 해결</li>
<li>손쉽게 확장하여 수백만명의 사용자를 지원<ul>
<li>Cloud Firestore (클라우드에 데이터 저장, 동기화, 명시적인 쿼리 작성, 서버리스 앱 빌드)</li>
<li><strong>Realtime Database (실시간 동기화 JSON 데이터를 저장하는 DB, (서버리스 앱 빌드))</strong></li>
<li>Remote Config (동작 제어)</li>
<li>Cloud Fundtions (서버 (를 따로 관리 안할 수 있도록 파이어베이스에서 서버 로직을 작성, 실행))</li>
<li><strong>Cloud Messaging (푸시 메시지)</strong></li>
<li>Cloud Storage (사진 및 동영상 저장)</li>
<li>Firebase ML (머신러닝)</li>
<li>등등</li>
</ul>
</li>
</ul>
</li>
</ul>
<br />    

<ul>
<li><code>출시 및 모니터링</code>  (짧은 시간에 훨씬 수월하게 앱 품질을 향상)<ul>
<li>테스트, 분류, 문제 해결 프로세스 간소화</li>
<li>기능을 신중하게 출시하고 도입을 모니터링하세요</li>
<li>문제를 조기에 정확하게 파악하여 우선순위를 정하고 안정성 및 성능 문제를 해결<ul>
<li>Google Analytics</li>
<li>Remote Config</li>
<li>Performance Monitoring</li>
<li>Test Lab</li>
<li>App Distribution</li>
</ul>
</li>
</ul>
</li>
</ul>
<br />    


<ul>
<li><code>참여</code> (앱 경험 최적화 및 고객 만족도 유지)<ul>
<li>사용자를 파악하여 더 효과적으로 지원하고, 사용자층을 유지</li>
<li>실험을 실행하여 아이디어를 테스트하고 새롭게 유용한 정보를 파악</li>
<li>다양한 세그먼트에 맞게 앱을 맞춤 설정<ul>
<li>Reomote Config</li>
<li>Google Analytics</li>
<li>A/B Testing</li>
<li><strong>Authentication</strong></li>
<li><strong>Cloud Messaging</strong></li>
<li>Crashlytics</li>
<li>Dynamic Links</li>
<li>In-App Messaging</li>
</ul>
</li>
</ul>
</li>
</ul>
<br />
<br />


<hr />
<h2 id="2-프로젝트-생성--android-앱-추가">2. 프로젝트 생성 &amp; Android 앱 추가</h2>
<h3 id="1-프로젝트-만들기">1) 프로젝트 만들기</h3>
<p><a href="https://firebase.google.com/">https://firebase.google.com/</a> 
로그인 후 프로젝트 생성
<img alt="" src="https://velog.velcdn.com/images/jaehere/post/44b95695-9001-4445-9152-032546973147/image.png" /></p>
<h3 id="2-앱추가">2) 앱추가</h3>
<h4 id="1-firebase를-이용할-android-프로젝트의-패키지-이름-입력">(1) Firebase를 이용할 Android 프로젝트의 패키지 이름 입력</h4>
<h4 id="2-구성-파일-다운로드-후-추가">(2) 구성 파일 다운로드 후 추가</h4>
<p>google-services.json 파일 다운로드 후 Android 프로젝트의 app 수준 루트 디렉토리에 위치시킨다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/jaehere/post/2254278f-f13f-4d96-9802-8b5132ddd514/image.png" /></p>
<h4 id="3-firebase-sdk-추가">(3) Firebase SDK 추가</h4>
<p>Kotlin DSL(build.gradle.kts) 기준</p>
<ul>
<li><p>루트 수준(프로젝트 수준) <code>build.gradle.kts</code></p>
<pre><code>plugins {
// ...

// Add the dependency for the Google services Gradle plugin
id("com.google.gms.google-services") version "4.4.2" apply false
</code></pre></li>
</ul>
<p>}</p>
<pre><code>
- 모듈(앱 수준) `build.gradle.kts`</code></pre><p>plugins {
  id("com.android.application")</p>
<p>  // Add the Google services Gradle plugin
  id("com.google.gms.google-services")</p>
<p>  ...
  }</p>
<p>dependencies {
  // Import the Firebase BoM
  implementation(platform("com.google.firebase:firebase-bom:33.8.0"))
}</p>
<pre><code>
&lt;br/&gt;&lt;br/&gt;

---

## 3. 실습


### 1) Firebase Realtime Database
빌드 - Realtime Database

데이터베이스 옵션 - 미국기준
보안 규칙 - 테스트모드에서 시작

데이터베이스 url이 생성된다.

![](https://velog.velcdn.com/images/jaehere/post/bf78852a-49eb-4345-9862-138f2adc6737/image.png)





### 2) Firebase Authentication

빌드 - Authentication
로그인 제공업체 선택 - 기본 제공업체 이메일/비밀번호
![](https://velog.velcdn.com/images/jaehere/post/a5e6a48c-eff2-4204-94f8-cd09576697d5/image.png)




Android 프로젝트에 Firebase Authentication 추가
[공식문서링크](https://firebase.google.com/docs/auth/android/start?hl=ko&amp;_gl=1*d1ofbg*_up*MQ..*_ga*NzYwMDM4NTk1LjE3MzgzMTEwNjA.*_ga_CW55HF8NVT*MTczODMxMTA2MC4xLjAuMTczODMxMTA2MC4wLjAuMA..&amp;gclid=Cj0KCQiAhvK8BhDfARIsABsPy4ivwd8y-_IAFp_KBvJJSryIPXmV8Al4i8nzAMVAocsEkjkOg5Qj84saAlzcEALw_wcB&amp;gclsrc=aw.ds)






### 3) Firebase Cloud Message (일명 FCM)
실행 - Messaging
push 메시지용

![](https://velog.velcdn.com/images/jaehere/post/a60280cb-6ee0-48e3-b52d-428a4a0ec181/image.png)


### 라이브러리 종속 항목 추가

- 모듈(앱 수준) `build.gradle.kts`</code></pre><p>dependencies {
    ...
    implementation(platform("com.google.firebase:firebase-bom:33.8.0"))
    implementation("com.google.firebase:firebase-auth")  // Authentication
    implementation("com.google.firebase:firebase-database") // Realtime Database
    implementation("com.google.firebase:firebase-messaging") // Cloud Message
}</p>
<p>```</p>
<hr />
<blockquote>
<p>** 참고**
FastCampus 강의 - 35개 프로젝트로 배우는 Android 앱 개발 feat. Jetpack Compose</p>
</blockquote>