<p>카카오맵 v1의 지원 종료로 인해 지도 표출 쿼터 수가 점차 줄어들었고, 결국 현재는 지도가 전혀 보이지 않는 상황이다.</p>
<p>v2로 마이그레이션을 해봅시다.  </p>
<blockquote>
<p>참고 링크: <a href="https://apis.map.kakao.com/android_v2/docs/getting-started/quickstart/">KakaoMaps SDK v2 for Android</a></p>
</blockquote>
<p>현재 작업은 기존 앱에서 카카오맵 SDK 버전만 교체하는 작업으로, 이미 Kakao Developers에서 사용 중인 키가 있으므로, 
    •    애플리케이션 추가
    •    APP KEY 발급
    •    Key Hash 추가
와 같은 작업은 생략 가능하다.</p>
<p>기존에 v1 사용을 위해 발급해둔 키 그대로 사용하면 된다. </p>
<p><img alt="" src="https://velog.velcdn.com/images/jaehere/post/08ff3445-328b-4cf8-938e-d6217cf43954/image.png" /></p>
<h3 id="1-카카오맵-v1-사용부분-제거">1. 카카오맵 v1 사용부분 제거</h3>
<ul>
<li>기존에 적용되어 있던** libDaumMapAndroid.jar **의존성 및 파일 제거했다.</li>
<li>카카오맵 v1 관련 패키지명이 import되어 사용되던 부분을 끊어냈다.</li>
</ul>
<br />

<h3 id="2-카카오맵-v2-sdk-의존성-추가">2. 카카오맵 v2 SDK 의존성 추가</h3>
<ul>
<li><strong>Maven 저장소 추가</strong>
원래처럼 .jar 파일로 적용할까 하고 찾아봤는데, 카카오지도 SDK v2 는 Maven 저장소에 배포된다고 한다.
따라서 카카오 원격저장소를 추가해주었다.</li>
</ul>
<p>[settings.gradle]</p>
<pre><code>dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
        maven {
            url 'https://devrepo.kakao.com/nexus/repository/kakaomap-releases/'
            // url 'https://devrepo.kakao.com/nexus/repository/kakaomap-snapshots/'
        }
    }
}</code></pre><ul>
<li>앱 프로젝트 모듈의 build.gradle 에 카카오지도 SDK 에 대한 의존성을 추가</li>
</ul>
<p>2025.01.23 오늘날짜 기준 최신 릴리즈 버전 적용</p>
<pre><code>implementation 'com.kakao.maps.open:android:2.12.11' </code></pre><br />

<h3 id="3-mapview-동적-추가">3. MapView 동적 추가</h3>
<p>xml에 정적으로 구현해두고 사용하는 방법도 있지만, 리소스 관리 및 유연성을 위해 동적으로 생성했다.</p>
<ul>
<li><p><strong>xml 레이아웃</strong></p>
<pre><code>&lt;RelativeLayout
         android:id="@+id/mapview_layout"
         android:layout_marginTop="@dimen/xxhdpi_11"
         android:layout_marginHorizontal="@dimen/xxhdpi_16"
         android:layout_width="match_parent"
         android:layout_height="@dimen/xxhdpi_153" &gt;
&lt;/RelativeLayout&gt;</code></pre></li>
<li><p><strong>mapView 생성</strong></p>
<pre><code>mapView = mapView ?: MapView(activity)
if (mapView.parent == null) {
  mBinding.mapviewLayout.addView(mapView)
}</code></pre><br />

</li>
</ul>
<h3 id="4-지도-시작-및-kakaomap-객체-가져오기">4. 지도 시작 및 KakaoMap 객체 가져오기</h3>
<pre><code> mapView?.start(object : MapLifeCycleCallback {
            override fun onMapDestroy() {
                // 지도 API가 정상적으로 종료될 때 호출
            }

            override fun onMapError(error: Exception) {
                // 인증 실패 및 지도 사용 중 에러 발생 시 호출
                Log.e("MapFragment", "Map error: ${error.message}")
            }
        }, object : KakaoMapReadyCallback {
            override fun onMapReady(kakaoMap: KakaoMap) {
                // 지도 API가 인증 후 정상적으로 실행될 때 호출

            }
        })</code></pre><h3 id="🚨-fragment-dialog에서-지도-사용하면서-발생한-문제">🚨 Fragment Dialog에서 지도 사용하면서 발생한 문제</h3>
<p>DialogFragment에서 MapView를 사용하는 과정에서, onCreateView에서 MapView를 바로 생성할 경우 일부 Android 기기에서 Dialog가 보이지 않는 문제가 발생했다. 
비교적 최신 폰 및 최신 os에서는 문제가 확인되지 않았다.</p>
<h4 id="문제-재현-기기"><strong>문제 재현 기기</strong></h4>
<pre><code>•    LG V50S ThinQ (Android 11)
•    Galaxy S10 (Android 12)
•    Galaxy Note 10+ (Android 13)</code></pre><p>Dialog가 보이지 않는 상황에서도 Kakao Map의 onMapReady 콜백이 정상적으로 호출되는 점을 통해, DialogFragment의 생명주기와 MapView 초기화 타이밍 간의 싱크 문제로 보였다.</p>
<p>이를 해결하기 위해 두 가지 방식을 모두 적용해보았고, Dialog와 Kakao Map이 정상적으로 표시되는 것을 확인했다.</p>
<p><strong>1)    MapView 초기화를 onResume에서 지연 실행</strong>
    DialogFragment가 완전히 생성된 이후 MapView를 초기화하도록 변경.</p>
<p><strong>2)    MapView.start 호출 시 Handler를 사용한 지연 처리</strong>
    화면이 준비된 이후에 MapView가 초기화되도록, Handler를 통해 실행 시점을 조정.</p>