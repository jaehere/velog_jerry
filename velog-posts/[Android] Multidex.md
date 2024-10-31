<h2 id="error">error</h2>
<p>dex 파일 컴파일 시 제한 메서드 수(65536개) 초과해서 발생하는 빌드 오류</p>
<h2 id="how-to">how to?</h2>
<p>minSdk 20 이하에서 multidex 라이브러리 사용하여 dex파일을 쪼개주어 제한을 피할 수 있다.</p>
<ul>
<li><p>minSdkVersion 21 이상: multidex 기본으로 사용됨</p>
</li>
<li><p>minSdkVersion 20 이하: multidex 사용에 대해서 명시 필요</p>
</li>
</ul>
<blockquote>
<p><a href="https://developer.android.com/build/multidex?hl=ko">Ref.</a></p>
</blockquote>