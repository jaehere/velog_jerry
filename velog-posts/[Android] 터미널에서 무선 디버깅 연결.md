<p>Android Studio-Pair new devices over Wi-Fi 에서
QR code/pairing code로 무선디버깅이 안 될 때,</p>
<p>터미널에서 무선디버깅 연결하는 방법입니다.</p>
<br />

<p><strong>1. platform-tools 경로로 이동</strong></p>
<pre><code>cd /Users/ijaehui/Library/Android/sdk/platform-tools</code></pre><br />

<p><strong>2. adb connet</strong></p>
<pre><code>#adb connect {IP 주소 및 포트}

adb connect 192.168.1.239:35533</code></pre><br />

<p><strong>3. adb pair</strong></p>
<ul>
<li><p>페어링 코드로 기기 페어링-IP 주소 및 포트 정보를 입력</p>
<pre><code>#adb pair {페어링 코드로 기기 페어링-IP 주소 및 포트}
adb pair 192.168.1.239:42245</code></pre></li>
<li><p>페어링 코드로 기기 페어링-Wi-Fi 페어링 코드 6자리 입력</p>
</li>
</ul>
<pre><code>#Enter pairing code: 페어링 코드
Enter pairing code: 442477</code></pre><ul>
<li>성공메세지<pre><code>Successfully paired to 192.168.1.239:42245 [guid=adb-R3CX707R7EE-gAtEee]</code></pre></li>
</ul>
<br />

<blockquote>
<p>터미널 첨부
<img alt="" src="https://velog.velcdn.com/images/jaehere/post/f9821c00-5893-43a7-aee7-084434074ade/image.png" /></p>
</blockquote>