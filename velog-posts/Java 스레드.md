<h1 id="스레드">스레드</h1>
<ul>
<li>스레드 생성 작업은 상대적으로 무겁다.</li>
</ul>
<p>어떤 작업 하나를 수행할때마다 스레드를 매번 생성하고 실행하면, 스레드 생성 비용 때문에 이미 많은 시간이 소모됨.</p>
<p>아주 가벼운 작업이라면, 작업의 실행시간보다 스레드의 생성 시간이 . 더오래 걸릴 수도 있다.</p>
<p>스레드 재사용 하면 처음 생성할 때를 제외하고는 생성을 위한 시간이 들지 않기에, 스레드가 아주 빠르게 작업을 수행할  수있다</p>
<ul>
<li><p>Runnable 인터페이스의 불편함</p>
<pre><code class="language-java">  public interface Runnable {
      void run();
  }</code></pre>
<ul>
<li>반환 값이 없다: run() 메서드는 반환값을 가지지 않는다. 따라서 실행 결과를 얻기 위해서는 별도의 메커니즘을 사용해야 한다. 쉽게 이야기해서 스레드의 실행 결과를 직접 받을 수 없다.</li>
<li>예외 처리: run() 메서드는 체크 예외를 던질 . 수없다. 체크 예외의 처리는 메서드 내부에서 처리해야한다.</li>
</ul>
</li>
</ul>
<p><strong>해결</strong></p>
<p>스레드를 생성하고 관리하는 풀(Pool) 이 필요하다.</p>
<ul>
<li>스레드를 관리하는 스레드 풀(스레드가 모여서 대기하는 수영장 풀 같은 개념)에 스레드를 미리 필요한 만큼 만들어둔다.</li>
<li>스레드는 스레드 풀에서 대기하며 쉰다.</li>
<li>작업 요청이 온다.</li>
<li>스레드 풀에서 이미 만들어진 스레드를 하나 조회한다.</li>
<li>조회한 스레드1로 작업을 처리한다.</li>
<li>스레드1은 작업을 완료한다.</li>
<li>작업을 완료한 스레드는 종료하는게 아니라, 다시 스레드 풀에 반납한다. 스레드1은 이후에 다시 재사용 . 될수 있다.</li>
</ul>
<p>→ 스레드 풀이라는 개념을 사용하면 스레드를 재사용할 . 수있어서, 재사용시 스레드의 생성 시간을 절약할 . 수있다. 그리고 스레드 풀에서 스레드가 관리되기 때문에 필요한 만큼만 스레드를 만들 수 있고, 또 관리할 . 수있다.</p>
<p>자바가 제공하는 Executor 프레임워크 - 스레드 풀, 스레드 관리, Runnable의 문제점은 물론이고, 생상자 소비자 문제까지 한방에 해결해주는 자바 멀티스레드 최고의 도구.</p>
<h2 id="executor-프레임워크">Executor 프레임워크</h2>
<p>자바의 Executor 프레임워크는 멀티스레딩 및 병렬 처리를 쉽게 사용할 수있도록 돕는 기능 모음. 작업 실행 관리 및 스레드 풀 관리를 효율적으로 처리해서 개발자가 직접 스레드를 생성하고 관리하는 복잡함을 줄여준다.</p>
<h3 id="executor-프레임워크의-주요-구성-요소">Executor 프레임워크의 주요 구성 요소</h3>
<ul>
<li><p>Executor 인터페이스</p>
<pre><code class="language-java">  package java.util.concurrent;

  public interface Executor {
      void execute(Runnable command); //작업을 요청하는 메서드
  }</code></pre>
</li>
</ul>
<ul>
<li><p>ExecutorService 인터페이스 - 주요 메서드</p>
<pre><code class="language-java">  public interface ExecutorService extends Executor, AutoCloseable {

      &lt;T&gt; Future&lt;T&gt; submit(Callable&lt;T&gt; task);

      @Override
      default void close(){...}

      ...

  }</code></pre>
<p>  주요 메서드로는 submit(), close()가 있다</p>
<p>  ExecutorService 인터페이스의 기본 구현체는 ThreadPoolExecutor이다.</p>
</li>
</ul>
<ul>
<li><p>상태 체크</p>
<pre><code class="language-java">  public abstract class ExecutorUtils {

      public static void printState(ExecutorService executorService) {

          if(executorService instanceof ThreadPoolExecutor poolExecutor) { //구조체 캐스팅해서 사용
              int pool = poolExecutor.getPoolSize();
              int active = poolExecutor.getActiveCount();
              int queuedTasks = poolExecutor.getQueue().size();
              long completedTask = poolExecutor.getCompletedTaskCount();

              log("[pool=" + pool + ", active=" + active 
                  + ", queuedTasks=" + queuedTasks 
                  + ", completedTask="+ completedTask +"]");

          } else {
              log(executorService);
          }

      }

  }</code></pre>
<ul>
<li><p>pool: 스레드 풀에서 관리되는 스레드의 숫자</p>
</li>
<li><p>active: 작업을 수행하는 스레드의 숫자</p>
</li>
<li><p>queuedTasks: 큐에 대기중인 작업의 숫자</p>
</li>
<li><p>completedTask: 완료된 작업의 숫자</p>
<p>참고로 ExecutorService 인터페이스는 geetPoolSize(), getActiveCount() 같은 자세한 기능은 제공하지 않는다. 이 기능은 ExecutorService의 대표 구현체인 ThreadPoolExecutor를 사용해야 한다.</p>
<p>printState() 메서드에 ThreadPoolExecutor 구현체가 넘어오면 우리가 구성한 로그를 출력하고, 그렇지 않은 경우에는 인스턴스 자체를 출력한다.</p>
</li>
</ul>
</li>
</ul>
<p>es.close(); // shotdown </p>
<p>ExecutorService의 가장 대표적인 구현체는 ThreadPoolExecutor 이다.</p>
<ul>
<li>스레드 풀</li>
<li>BlockingQueue: 작업을 보관한다. 생산자 소비자 문제를 해결하기 위해 단순한 큐가 아니라, BlockingQueue를 사용한다.</li>
</ul>
<p>생산자가 es.execute(new RunnableTask(”taskA”))를 호출하면, RunnableTask(”taskA”) 인스턴스가 BlockingQueue에 보관된다.</p>
<ul>
<li>생산자: es.execute(작업)를 호출하면 내부에서 BlockingQueue에 작업을 보관한다. main 스레드가 생산자가 된다.</li>
<li>소비자: 스레드 풀에 있는 스레드가 소비자이다. 이후에 소비자 중에 하나가 BlockingQueue에 들어있는 작업을 받아서 처리한다.</li>
</ul>
<p>ThreadPoolExecutor 생성자</p>
<blockquote>
<p>안드로이드 공식문서:  자바 스레드를 사용한 비동기 작업 <a href="https://developer.android.com/develop/background-work/background-tasks/asynchronous/java-threads?hl=ko">https://developer.android.com/develop/background-work/background-tasks/asynchronous/java-threads?hl=ko</a></p>
</blockquote>