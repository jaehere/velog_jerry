<p><a href="https://jsoup.org/"><strong>jsoup</strong>: Java HTML Parser</a></p>
<p>HTML 데이터를 분석, 조작, 그리고 웹 스크래핑할 수 있도록 설계된 Java 기반의 라이브러리이다.</p>
<p>정적 페이지를 크롤링하는 용도로 주로 사용한다.</p>
<p><strong>장점</strong>은 HTML, XML 모두 처리 가능하다는 점이 있고,</p>
<p><strong>단점</strong>은 동적 콘텐츠 처리가 불가능하다.
Jsoup은 파싱 전에 HTML소스를 가져오기 때문에 정적인 HTML 데이터를 처리한다.
JavaScript로 생성된 동적 콘텐츠는 별도로 처리해야 한다. 이런 경우 Selenium 같은 도구와 함께 사용하는 것이 좋다고 한다.</p>
<br />

<p>[build.gradle] </p>
<pre><code>implementation 'org.jsoup:jsoup:1.18.3'</code></pre><h3 id="사용-예제">사용 예제</h3>
<p>Jsoup을 활용하여 Google 뉴스 페이지를 크롤링하고, 각 기사에 포함된 썸네일 이미지를 가져왔다.</p>
<pre><code>val newsService = retrofit.create(NewsService::class.java)
        newsService.mainFeed().enqueue(object : Callback&lt;NewsRss&gt; {
            override fun onResponse(p0: Call&lt;NewsRss&gt;, response: Response&lt;NewsRss&gt;) {
                Log.e("MainActivity", "${response.body()?.channel?.items}")

                val list = response.body()?.channel?.items.orEmpty()
                val item = list.first()

                val jsoup = item.link?.let { Jsoup.connect(it).get() }
                val elements = jsoup?.select("meta[property^=og:]")
                val ogImageNode = elements?.find { node -&gt;
                    node.attr("property") == "og:image"
                }
                val imageUrl = ogImageNode?.attr("content")


                newsAdapter.submitList(response.body()?.channel?.items.orEmpty())
            }</code></pre><blockquote>
<p><strong>참고</strong>
FastCampus 강의 - 35개 프로젝트로 배우는 Android 앱 개발 feat. Jetpack Compose</p>
</blockquote>