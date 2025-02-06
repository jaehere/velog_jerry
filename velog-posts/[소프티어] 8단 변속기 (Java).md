<p><code>Lv.2</code></p>
<h3 id="문제">문제</h3>
<p>현대자동차에서는 부드럽고 빠른 변속이 가능한 8단 습식 DCT 변속기를 개발하여 N라인 고성능차에 적용하였다. 관련하여 SW 엔지니어인 당신에게 연속적으로 변속이 가능한지 점검할 수 있는 프로그램을 만들라는 임무가 내려왔다.</p>
<p>당신은 변속기가 1단에서 8단으로 연속적으로 변속을 한다면 ascending, 8단에서 1단으로 연속적으로 변속한다면 descending, 둘다 아니라면 mixed 라고 정의했다.</p>
<p>변속한 순서가 주어졌을 때 이것이 ascending인지, descending인지, 아니면 mixed인지 출력하는 프로그램을 작성하시오.</p>
<h3 id="제약조건">제약조건</h3>
<p>주어지는 숫자는 문제 설명에서 설명한 변속 정도이며, 1부터 8까지 숫자가 한번씩 등장한다.</p>
<h3 id="입력형식">입력형식</h3>
<p>첫째 줄에 8개 숫자가 주어진다.</p>
<h3 id="출력형식">출력형식</h3>
<p>첫째 줄에 ascending, descending, mixed 중 하나를 출력한다.</p>
<h4 id="입력예제1">입력예제1</h4>
<pre><code>1 2 3 4 5 6 7 8</code></pre><h4 id="출력예제1">출력예제1</h4>
<pre><code>ascending</code></pre><h3 id="내-코드">내 코드</h3>
<pre><code>import java.util.Scanner;


public class Main {
    static int[] gear;
    static boolean ascending = true;
    static boolean descending = true;

    public static void main(String[] args) {
        input();
        System.out.println(ord_func());
    }

    private static String ord_func() {

        for(int i=0; i&lt;7; i++){
            ascending = ascending &amp;&amp; (gear[i] &lt; gear[i + 1]);
            descending = descending &amp;&amp; (gear[i] &gt; gear[i + 1]);
        }
        if(ascending){
            return "ascending";
        } else if(descending){
            return "descending";
        } else{
            return "mixed";
        }

    }

    public static void input(){
        gear = new int[8];

        Scanner sc = new Scanner(System.in);
        for(int i=0; i&lt;8; i++){
            gear[i] = sc.nextInt();
        }
    }
}</code></pre><p>이 방법 외에도 간단하게 배열 두개 생성해두고 input data와 비교해도 된다.
ascending 배열 {1,2,3,4,5,6,7,8}
descending 배열 {8,7,6,5,4,3,2,1} </p>
<blockquote>
<p><a href="https://softeer.ai/practice/6283#pop_user">https://softeer.ai/practice/6283#pop_user</a></p>
</blockquote>