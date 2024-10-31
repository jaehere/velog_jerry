<p><code>Ktlint</code> 는 Kotlin 코딩 규칙과 Android Kotlin 스타일 가이드를 검사해주는 도구 입니다.</p>
<p>제가 평소에 베이킹, 인테리어 레퍼런스를 얻기 위해 자주 이용하는 Pinterest가 제공하는 툴입니다.</p>
<p>제공되는 규칙을 하나 지정해두고 사용하면 협업 및 관리에 용이하겠습니다.</p>
<br />

<p>사용방법은 크게 두가지입니다.</p>
<ol>
<li>설치하여 사용</li>
<li>gradle 의존성 주입하여 사용</li>
</ol>
<br />

<p>1번은 ktlint 사이트나 homebrew로 설치하여 사용하면 됩니다.
<br /></p>
<p>실제 사용한 <strong>2번 gradle 의존성 주입</strong>하여 사용하는 방법을 기록합니다.</p>
<hr />
<ul>
<li>configuration 설정<pre><code>configurations {
  ktlint
}
</code></pre></li>
</ul>
<pre><code>

- 의존성 추가
</code></pre><p>dependencies {
    ktlint("com.pinterest.ktlint:ktlint-cli:1.3.1") {
        attributes {
            attribute(Bundling.BUNDLING_ATTRIBUTE, getObjects().named(Bundling, Bundling.EXTERNAL))
        }
    }
    // additional 3rd party ruleset(s) can be specified here
    // just add them to the classpath (e.g. ktlint 'groupId:artifactId:version') and 
    // ktlint will pick them up
}</p>
<pre><code>

- ktlint 스타일에 맞지 않은 코드를 알려줍니다.
명령어: `./gradlew ktlintCheck`</code></pre><p>tasks.register("ktlintCheck", JavaExec) {
    group = "verification"
    description = "Check Kotlin code style."
    classpath = configurations.ktlint
    mainClass = "com.pinterest.ktlint.Main"
    // see <a href="https://pinterest.github.io/ktlint/install/cli/#command-line-usage">https://pinterest.github.io/ktlint/install/cli/#command-line-usage</a> for more information
    args "src/<strong>/*.kt", "</strong>.kts", "!<strong>/build/</strong>"
}</p>
<p>tasks.named("check") {
    dependsOn tasks.named("ktlintCheck")
}</p>
<pre><code>

- ktlint 스타일에 맞지 않은 코드 전체를 알아서 수정해줍니다. (이 때, 도구가 수정할 수 없는 코드가 존재할 수 있습니다)
명령어: `./gradlew ktlintFormat`</code></pre><p>tasks.register("ktlintFormat", JavaExec) {
    group = "formatting"
    description = "Fix Kotlin code style deviations."
    classpath = configurations.ktlint
    mainClass = "com.pinterest.ktlint.Main"
    jvmArgs "--add-opens=java.base/java.lang=ALL-UNNAMED"
    // see <a href="https://pinterest.github.io/ktlint/install/cli/#command-line-usage">https://pinterest.github.io/ktlint/install/cli/#command-line-usage</a> for more information
    args "-F", "src/<strong>/*.kt", "</strong>.kts", "!<strong>/build/</strong>"
}</p>
<p>```</p>
<p>참고: <a href="https://pinterest.github.io/ktlint/1.3.1/install/integrations/#gradle-integration">https://pinterest.github.io/ktlint/1.3.1/install/integrations/#gradle-integration</a></p>