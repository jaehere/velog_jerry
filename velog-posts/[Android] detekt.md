<h2 id="detekt"><code>Detekt</code></h2>
<ul>
<li>A static code analysis for Kotlin</li>
<li>Kotlin의 대표적인 CODE CONVENTION TOOL</li>
<li>정적 프로그램 분석 툴</li>
</ul>
<hr />
<h3 id="ktlint-vs-detekt">ktlint VS detekt</h3>
<ul>
<li><code>ktlint</code> 는 코딩 컨벤션을 중점적</li>
<li><code>detekt</code> 는 코드의 전체적인 퀄리티를 높이기 위한 분석 수행. 메서드의 길이가 너무 길다거나, 메서드의 depth가 너무 깊다거나 등의 분석 수행.</li>
</ul>
<hr />
<h3 id="detekt-사용하기링크">detekt 사용하기(<a href="https://detekt.dev/docs/gettingstarted/gradletask">링크</a>)</h3>
<p>build.gradle 설정</p>
<ul>
<li><p>configurations 추가</p>
<pre><code>configurations {

  detekt
}</code></pre></li>
<li><p>dependencies 주입</p>
<pre><code></code></pre></li>
</ul>
<p>dependencies {</p>
<pre><code>detekt 'io.gitlab.arturbosch.detekt:detekt-cli:1.23.7'</code></pre><p>}</p>
<pre><code>
- detekt 정적분석 수행
`./gradlew detekt`
</code></pre><p>task detekt(type: JavaExec) {
    mainClass.set("io.gitlab.arturbosch.detekt.cli.Main")
    classpath = configurations.detekt</p>
<pre><code>def input = "$projectDir"
def exclude = ".*/build/.*,.*/resources/.*"
def params = [ '-i', input, '-ex', exclude]

args(params)</code></pre><p>}</p>
<p>```</p>
<p>detekt task 수행하면 어떤 line에서 스타일 문제가 있는지 알려준다.</p>
<blockquote>
<p>예시: is not ending with a new line. [NewLineAtEndOfFile]</p>
</blockquote>
<hr />
<h3 id="configuration-파일-설정링크">configuration 파일 설정(<a href="https://github.com/detekt/detekt/blob/v1.23.7/detekt-core/src/main/resources/default-detekt-config.yml">링크</a>)</h3>
<p>config 파일 설정을 통해서 내가 원하는 스타일 제어할 수 있다.</p>
<p>링크에서 default-detekt-config.yml 코드를 복사한다.
프로젝트 수준에서 detekt.yml 파일 생성한다.</p>
<br />



<blockquote>
<p>참조 </p>
</blockquote>
<ol>
<li><a href="https://fastcampus.co.kr/dev_online_androidappfinal">fastcampus 온라인 강의) 35개 프로젝트로 배우는 Android 앱 개발 feat. jetpack Compose</a></li>
<li><a href="https://detekt.dev">https://detekt.dev</a></li>
</ol>