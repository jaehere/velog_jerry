<p>Tombstone 무덤: 나 죽어 여기 기록 되다..</p>
<p>앱 비정상 종료 시 logcat에 기본 크래시가 기록되고, 더 자세한 내용은 Tombstone 파일은 /data/tombstones/에 기록된다. </p>
<p>native crash가 발생한 경우 더 상세한 내용을 보고자 tombstone 을 찾게 되었다.</p>
<blockquote>
<p>Tombstone은 비 정상 종료 프로세스에 대한 추가 데이터가 포함된 파일입니다. 여기에는 신호를 포착한 스레드가 아니라 정상 종료 프로세스 내 모든 스레드, 전체 메모리 및 특별히 있는 모든 파일 설명자의 그리드 트레이스가 포함되어 있습니다.</p>
</blockquote>
<h3 id="툼스톤-확인-경로">툼스톤 확인 경로</h3>
<p>루팅 단말인 경우는 Android studio - Device explorer- /data/tombstones/ 경로로도 쉽게 접근이 가능하다.
일반적으로 /data/tombstones/ 경로는 루팅단말이 아닌 이상 아래와 같이 접근이 어렵다.</p>
<pre><code>/system/bin/sh: cd: /data/tombstones: Permission denied</code></pre><p>bugreport를 추출해서 tombstones 파일을 조회하는 방법을 택했다.</p>
<pre><code>adb bugreport ./bugreport.zip</code></pre><p>추출된 bugreport 압축을 해제하면 bugreport-FS-data-tombstones 에서
tombstone_00 과 같은 파일을 조회할 수 있다.
<img alt="" src="https://velog.velcdn.com/images/jaehere/post/cf11cf7c-7e0b-4965-a7c1-384dcdbeff25/image.png" /></p>
<p>파일을 열어보면 logcat 보다 자세하게 에러로그가 기록되어 있다.</p>
<h3 id="backtrace-분석">backtrace 분석</h3>
<pre><code>backtrace:
      #00 pc 000000000005b730  /apex/com.android.runtime/lib64/bionic/libabc.so (abort+168) (BuildId: 37f537c2ba9dcbb262a0a68f41a21da4)
      #01 pc 000000000076fde0  /apex/com.android.art/lib64/libart.so (art::Runtime::Abort(char const*)+904) (BuildId: 7ece79c15d80914c83e60c9e93ac1684)</code></pre><ul>
<li>터미널 명령어
jMac:~ ijaehui$ cd /Users/ijaehui/Library/Android/sdk/ndk/27.2.12479018/toolchains/llvm/prebuilt/darwin-x86_64/bin
jMac:bin ijaehui$ ./llvm-addr2line -C -f -e /Users/ijaehui/Documents/bugreport/libabc.so 0005b73
??
??:0</li>
</ul>
<ul>
<li><p>???? 왜 물음표만 반환하나요 선생님 ???
libabc.so 에 디버그 심볼이 없어서 그럴 수 있다. (strip 처리됨)
libabc.so가 strip 되어 디버그 심볼이 제거된 경우, addr2line이 파일 내의 정확한 라인 정보를 찾지 못함.</p>
</li>
<li><p>디버그심볼 유무 조회 명령어</p>
<pre><code>file libabc.so</code></pre></li>
</ul>
<p>1) 디버그 심볼이 있는 경우 결과
ELF 64-bit LSB shared object, ARM aarch64, not stripped</p>
<p>2) 디버그 심볼이 없는 경우 결과
ELF 64-bit LSB shared object, ARM aarch64, stripped</p>
<p>공유 라이브러리 빌드할 때,  디버그 심볼 포함해준다 (-g 옵션 활성화)
ndk-build NDK_DEBUG=1</p>
<blockquote>
<p>참고</p>
</blockquote>
<ul>
<li>안드로이드 공식문서: <a href="https://source.android.com/docs/core/tests/debug?hl=ko">네이티브 Android 플랫폼 코드 디버깅</a></li>
<li>스택오버플로우 글: <a href="https://stackoverflow.com/questions/28105054/default-tombstones-location-in-android">Default tombstones location in android</a></li>
</ul>