<h2 id="안드로이드-16kb-페이지-크기-지원">안드로이드 16kb 페이지 크기 지원</h2>
<blockquote>
<ul>
<li>관련 문서: <a href="https://developer.android.com/guide/practices/page-sizes?hl=ko#alignment-use-tools">https://developer.android.com/guide/practices/page-sizes?hl=ko#alignment-use-tools</a>
"지금까지 Android는 4KB 메모리 페이지 크기만 지원했습니다. 시스템 메모리 성능을 최적화하여 Android 기기에는 일반적으로 그랬습니다. Android 15부터 AOSP는 16KB (16KB)의 페이지 크기를 사용하도록 구성된 기기 기기에서 지원됩니다."</li>
</ul>
</blockquote>
<p>구글에서 안드로이드 15가 출시 되면서 설정에서 16kb 지원을 선택할 수 있어졌다.</p>
<p>이때 네이티브 라이브러리는 ELEF 세그먼트 정렬 확인을 하라고 하는데 2**14보다 작을 경우 패키징을 업데이트해야 한다.</p>
<p>16kb page size 지원이 안되어 있는 공유 라이브러리에서 에러가 발생한 내역이다.</p>
<br />

<h3 id="1-16kb-page-size-지원이-안되어-있는-공유라이브러리-사용에-따른-에러-발생-내역">1. 16kb page size 지원이 안되어 있는 공유라이브러리 사용에 따른 에러 발생 내역</h3>
<p>(로그 발췌 - 일부 수정) </p>
<pre><code>*** *** *** *** *** *** *** *** *** *** *** *** *** *** *** ***

Build fingerprint: 'google/sdk_gphone16k_arm64/emu64a16k:15/AE3A.11111.019/11111111:user/dev-keys'

ABI: 'arm64'

Page size: 16384 bytes

Cmdline: com.jaehee.application

signal 11 (SIGSEGV), code 2 (SEGV_ACCERR), fault addr 0x00007adde111b1b1

#00 pc 0000000000030b00  /data/app/~~MZIoRUIqaph4mWcMqAzWWA==/com.jaehee.application-5XXXXXX-XXXXXXXXXXXXX==/lib/arm64/libxxx.so</code></pre><br />

<h3 id="2-16kb-page-size-옵션-사용">2. 16kb page size 옵션 사용</h3>
<p>공유 라이브러리에 16KB page size 지원을 설정하는 방법이다.</p>
<ul>
<li>Android NDK r27 이상
Android NDK로 16KB 정렬 공유 라이브러리 컴파일 지원 버전 r27 이상을 실행하는 경우 ndk-build, build.gradle, build.gradle.kts 또는 링커 플래그로 대체될 수 있습니다.
ndk-buildGroovyKotlin기타 빌드 시스템</li>
</ul>
<p>[Application.mk]</p>
<pre><code>APP_SUPPORT_FLEXIBLE_PAGE_SIZES := true</code></pre><br />

<h3 id="3-elf-세그먼트-정렬-확인">3. elf 세그먼트 정렬 확인</h3>
<p>check_elf_alignment 스크립트 (<a href="https://cs.android.com/android/platform/superproject/main/+/main:system/extras/tools/check_elf_alignment.sh?hl=ko">링크</a>)로 앱의 elf 세그먼트 값이 정상적으로 2**14 으로 설정된 것을 확인할 수 있다.</p>
<pre><code>check_elf_alignment.sh APK_NAME.apk</code></pre><p>출력 예시)
/lib/arm64-v8a/libxxx.so: \e[32mALIGNED\e[0m (2**14)</p>
<p>/lib/arm64-v8a/libyyy.so: \e[32mALIGNED\e[0m (2**14)</p>
<hr />
<p>결과적으로 16kb page size 옵션이 적용된 단말에서 정상 실행된다.</p>