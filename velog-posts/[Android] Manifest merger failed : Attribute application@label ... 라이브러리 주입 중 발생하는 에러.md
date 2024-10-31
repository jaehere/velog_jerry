<p><strong>라이브러리 주입하여 빌드할 때 에러 발생</strong></p>
<p>Manifest merger failed : Attribute application@</p>
<pre><code>Manifest merger failed : Attribute application@label value=("xxx") from AndroidManifest.xml:78:9-36
is also present at [주입한 라이브러리] AndroidManifest.xml:9:9-41 value=(@string/app_name).
Suggestion: add 'tools:replace="android:label"' to &lt;application&gt; element at AndroidManifest.xml:4:5-37:19 to override.</code></pre><p>Android Manifest 파일 병합 시 충돌이 발생.</p>
<p>본 프로젝트 Manifest 파일에서는 application 태그의 label 속성을 "xxx"로 설정하고 있지만, 
주입한 라이브러리에서는 같은 속성인 label을 @string/app_name으로 설정하고 있다.</p>
<p>오류 메시지에서 제안하는대로 tools:replace="android:label" 속성을  요소에 추가하면 충돌 해결이 가능하다.</p>
<pre><code>    &lt;application
        android:label="xxx"
        tools:replace="android:label"
        ...                           
     /&gt;</code></pre><p>tools:replace="android:label"은 Manifest 파일을 병합할 때 동일한 속성에 대해 충돌이 발생할 때, 어떤 속성을 사용할 것인지를 지정하는 역할을 해준다.
해당 속성 추가할 경우 Manifest 병합 과정에서는 android:label="xxx"가 우선적으로 사용되고, 주입한 라이브러리에서 제공하는 android:label="@string/app_name"이 우선순위에서 밀려서 사용되지 않는다.</p>