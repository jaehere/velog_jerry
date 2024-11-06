<h2 id="하나의-컴퓨터에서-git계정을-여러개-설정-하기">하나의 컴퓨터에서 git계정을 여러개 설정 하기</h2>
<p>global 계정을 메인계정,
특정 directory의 파일은 다른계정으로 설정하였습니다.</p>
<h3 id="1-ssh-key-생성">1) SSH key 생성</h3>
<pre><code>
// .ssh 폴더 생성
mkdir ~/.ssh

// 폴더 이동
cd ~/.ssh

// 개인용 ssh key 생성
ssh-keygen -t rsa -C "깃허브personal이메일주소" -f "personal"

// 회사용 ssh key 생성
ssh-keygen -t rsa -C "깃허브company이메일주소" -f "company"

// 파일 생성여부 확인
ls -l</code></pre><p>(뒤에 .pub 확장자가 붙은것은 public key)</p>
<p>Your identification has been saved in personal
Your public key has been saved in personal.pub
The key fingerprint is: <del>~</del></p>
<br />

<h3 id="2-config-파일-생성-및-작성">2) config 파일 생성 및 작성</h3>
<pre><code>// ssh config 설정
vim config

// 안에 아래와 같이 작성
Host github.com-personal // 개인 계정
    HostName github.com
    User 개인깃헙이메일@email.com
    IdentityFile ~/.ssh/personal

Host github.com-company // 회사 계정
    HostName github.com
    User 회사깃헙이메일@email.com
    IdentityFile ~/.ssh/company

//저장 종료
es + :wq

</code></pre><br />

<h3 id="3-ssh-agent-등록">3) SSH agent 등록</h3>
<pre><code>// ssh agent 등록
ssh-add personal //개인 계정 등록
ssh-add company //회사 계정 등록

// 잘 등록되었는지 확인
ssh-add-l</code></pre><br />

<h3 id="4-깃헙에-public-key-등록">4) 깃헙에 public key 등록</h3>
<p>(* .pub 확장자 파일 내용 전체를 복붙할 것. ssh-rsa로 시작해서 내가 설정한 이메일로 끝난다.)깃헙 사이트 방문 &gt; 프로필 &gt; Setting &gt; SSH and GPG keys &gt; New SSH key</p>
<br />

<h3 id="5-특정-디렉토리에서-특정-계정-사용할-수-있게-분리">5) 특정 디렉토리에서 특정 계정 사용할 수 있게 분리</h3>
<p>나는 global을 company 계정으로 설정하고,
특정 디렉토리에서 personal 계정을 설정했다.</p>
<ul>
<li>gitconfig 설정</li>
</ul>
<pre><code>// 특정 폴더에서는 특정 계정을 사용할 수 있게 분리하고 싶을 때
vim ~/.gitconfig


// 아래 내용 입력

[includeIf “gitdir: ~/특정디렉토리경로”]
path = .gitconfig-personal
</code></pre><ul>
<li>gitconfig-personal 생성 및 계정정보추가<pre><code>// 위의 path 이름으로 된 gitconfig 파일 생성하기
vim ~/.gitconfig-personal

</code></pre></li>
</ul>
<p>// 아래의 내용 입력
[user]
name = 계정이름
email = 계정 이메일</p>
<p>// 저장
esc + :wq</p>
<p>```</p>
<blockquote>
<p>참고: <a href="https://nareunhagae.tistory.com/71">https://nareunhagae.tistory.com/71</a> </p>
</blockquote>