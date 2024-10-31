<hr />
<h2 id="neterr_name_not_resolved">"net::ERR_NAME_NOT_RESOLVED"</h2>
<hr />
<h3 id="🧨-에러-설명">🧨 에러 설명</h3>
<p>브라우저가 특정 도메인 이름을 IP 주소로 변환하지 못할 때 발생하는 오류.
즉, 웹사이트의 주소를 찾아서 연결할 수 없다는 뜻입니다.
<br /></p>
<h3 id="🔦-에러-요인">🔦 에러 요인</h3>
<pre><code>1.    DNS 설정 문제: 네트워크의 DNS 서버가 도메인 이름을 올바르게 해석하지 못할 수 있습니다. 이 경우, DNS 서버를 구글의 8.8.8.8 또는 8.8.4.4로 변경하면 도움이 될 수 있습니다.
2.    인터넷 연결 문제: 장치의 인터넷 연결 상태가 불안정하거나 연결이 끊어졌을 때도 이 오류가 나타날 수 있습니다.
3.    DNS 캐시 문제: 시스템 또는 브라우저의 DNS 캐시에 문제가 생겨서 이전에 저장된 잘못된 정보가 사용될 수 있습니다. 이를 해결하려면 DNS 캐시를 비우는 것이 좋습니다. Windows의 경우 ipconfig /flushdns 명령어로 캐시를 비울 수 있습니다.
4.    도메인 서버 문제: 특정 웹사이트가 서버 측의 문제로 인해 일시적으로 접근이 불가할 수도 있습니다.
5.    브라우저 또는 네트워크 설정 문제: 문제가 특정 브라우저에서만 발생한다면, 브라우저의 캐시 및 쿠키를 지우거나 다른 브라우저로 시도해 보는 것도 방법입니다.</code></pre><br />

<h3 id="🛠️-해결한-방법-참고">🛠️ 해결한 방법 (<a href="https://stackoverflow.com/questions/42736038/android-emulator-not-able-to-access-the-internet">참고</a>)</h3>
<p>Mac OS에서 DNS를 구글링한 대로 설정하고(8.8.8.8), 에뮬레이터를 콜드 부팅했지만 동일한 오류가 발생했습니다.</p>
<p>원인은 기존에 등록된 DNS 서버가 목록 상단에 있어, 추가한 8.8.8.8이 맨 아래에 위치했던 것입니다.</p>
<p>이에 8.8.8.8을 목록 상단으로 올려 우선순위를 조정했더니 오류가 해결되었습니다.</p>
<br />


<blockquote>
<p>참조
<a href="https://stackoverflow.com/questions/42736038/android-emulator-not-able-to-access-the-internet">https://stackoverflow.com/questions/42736038/android-emulator-not-able-to-access-the-internet</a></p>
</blockquote>