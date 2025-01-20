<p>NAS 웹 서버에 파일을 업로드하기 위해 스크립트를 실행하는 중에 2001 에러가 발생했습니다. </p>
<p>파일 업로드까지 완료되었고, 그 이후 단계로 진행되지 않아서 확인해보니
공유 링크의 개수가 최대 한도에 도달한 문제가 원인이였습니다.</p>
<p>만료되거나 사용하지 않는 공유 링크를 삭제하여 문제를 해결하였습니다.</p>
<p><strong>1001개로 조회되던 공유 링크 중, 필요 없는 항목들을 제거한 후 125개로 줄어들면서 문제가 해결되었습니다.</strong></p>
<h3 id="에러-내용">에러 내용</h3>
<pre><code>FTP Upload done..
Create Synology File Sharing Link..
...

&gt;&gt;&gt; fileSharingResponse {"error":{"code":2001},"success":false}
&gt;&gt;&gt; shared_link_url error {'code': 2001}
&gt;&gt;&gt; shared_link_url success False
Traceback (most recent call last):
...
    print('NAS_LINK %s %s %s' %(shared_link_url['data']['links'][0]['url'], nasPassword, os.path.basename(nasFilePath)))
                                ~~~~~~~~~~~~~~~^^^^^^^^
KeyError: 'data'
sh-error
</code></pre><p>chatGPT 가 알려준 에러 원인 및 공유링크 삭제 방법을 기록합니다.</p>
<h3 id="나스-서버-에러코드-2001">나스 서버 에러코드 2001</h3>
<p>나스 서버의 에러 코드 체계는 제조사와 제품에 따라 다를 수 있습니다. 일반적으로, 에러 코드 2001은 특정 오류를 나타내며, 그 의미는 다음과 같습니다:
    •    Synology File Station API: <strong>에러 코드 2001</strong>은 <strong>“공유 링크를 생성할 수 없습니다. 너무 많은 공유 링크가 존재합니다.“</strong>를 의미합니다.  </p>
<h3 id="공유-링크-삭제-방법-synology-nas-기준">공유 링크 삭제 방법 (Synology NAS 기준)</h3>
<ol>
<li>DSM (DiskStation Manager)에 로그인
웹 브라우저에서 NAS의 IP 주소를 입력하고 관리자 계정으로 로그인합니다.</li>
<li>File Station 열기
DSM 홈 화면에서 File Station 아이콘을 클릭하여 파일 관리 화면으로 이동합니다.</li>
<li>공유 링크 관리
•    화면 왼쪽 상단에서 공유 링크 탭을 클릭합니다. 이 탭에서 생성된 모든 공유 링크를 확인할 수 있습니다.
•    사용하지 않는 링크를 선택하여 삭제할 수 있습니다.
<img alt="" src="https://velog.velcdn.com/images/jaehere/post/725667ca-3190-4bf1-b232-15d5722966de/image.png" /></li>
<li>삭제
•    삭제할 공유 링크를 마우스 오른쪽 버튼으로 클릭하고 삭제를 선택합니다.
•    여러 개의 링크를 한 번에 삭제할 수도 있습니다.</li>
</ol>
<p>공유 링크 삭제 후
    •    공유 링크를 삭제하면 해당 링크를 통해 접근할 수 없게 됩니다. 따라서, 더 이상 사용하지 않는 링크는 삭제하여 새 링크 생성에 공간을 확보할 수 있습니다.</p>
<h3 id="나스서버-공유링크-최대한도">나스서버 공유링크 최대한도</h3>
<p>Synology NAS 서버에서 공유 링크의 최대 개수는 모델과 설정에 따라 다를 수 있지만, 
기본적으로 <strong>1,000개</strong>의 공유 링크가 최대 한도로 설정되어 있습니다. 이는 DSM 7.x 버전을 기준으로 한 정보입니다.</p>
<blockquote>
<p>정보 출처: OpenAI의 ChatGPT (<a href="https://chat.openai.com">https://chat.openai.com</a>)
해당 정보는 ChatGPT를 통해 제공된 답변을 바탕으로 작성되었습니다.</p>
</blockquote>