<p>문자열 자료 구조</p>
<h3 id="문제">문제</h3>
<p>알파벳 소문자로만 이루어진 단어 S가 주어진다. 각각의 알파벳에 대해서, 단어에 포함되어 있는 경우에는 처음 등장하는 위치를, 포함되어 있지 않은 경우에는 -1을 출력하는 프로그램을 작성하시오.</p>
<h3 id="입력">입력</h3>
<p>첫째 줄에 단어 S가 주어진다. 단어의 길이는 100을 넘지 않으며, 알파벳 소문자로만 이루어져 있다.</p>
<h3 id="출력">출력</h3>
<p>각각의 알파벳에 대해서, a가 처음 등장하는 위치, b가 처음 등장하는 위치, ... z가 처음 등장하는 위치를 공백으로 구분해서 출력한다.</p>
<p>만약, 어떤 알파벳이 단어에 포함되어 있지 않다면 -1을 출력한다. 단어의 첫 번째 글자는 0번째 위치이고, 두 번째 글자는 1번째 위치이다.</p>
<h3 id="예제">예제</h3>
<h4 id="입력-1">(입력 1)</h4>
<pre><code>baekjoon</code></pre><h4 id="출력-1">(출력 1)</h4>
<pre><code>1 0 -1 -1 2 -1 -1 -1 -1 4 3 -1 -1 7 5 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1</code></pre><h3 id="내-코드">내 코드</h3>
<h4 id="python">Python</h4>
<p><strong># 첫번째로 푼 방법</strong></p>
<pre><code>s = input()
check = [-1] * 26

for i in range(len(s)):
  alphabet = ord(s[i]) - 97
  if(check[alphabet] == -1):
    check[alphabet] = i

for i in range(len(check)):
  print(check[i],end=' ')</code></pre><p><strong># 두번째로 푼 방법</strong></p>
<pre><code>s = input()

check = [-1] * 26

for i in range(len(s)):
  index = ord(s[i]) - ord('a')

  if(check[index] == -1):
    check[index] = i

for i in range(26):
  print(check[i],end=' ')</code></pre><h4 id="java">Java</h4>
<p><strong># 첫번째로 푼 방법</strong></p>
<pre><code>import java.util.*;

public class Main {
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        String input = sc.next();
        int[] arr = new int[26];
        Arrays.fill(arr, -1);

        for (int i = 0; i &lt; input.length(); i++) {
            if(arr[input.charAt(i) - 97] == -1) {
                arr[input.charAt(i) - 97] = i;
            }
        }
        for(int a : arr){
            System.out.print(a + " ");
        }
    }
}</code></pre><p><strong># 두번째로 푼 방법</strong></p>
<pre><code>import java.util.*;

public class Main {
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        String input = sc.next();
        int[] arr = new int[26];
        Arrays.fill(arr, -1);

        for (int i = 0; i &lt; input.length(); i++) {
            int index = input.charAt(i) - 'a';
            if(arr[index] == -1){
                arr[index] = i;
            }
        }
        for(int a : arr){
            System.out.print(a + " ");
        }
    }
}</code></pre><h3 id="느낀점">느낀점</h3>
<p>어김없이 이용되는 체크용 배열
'a'를 쓰든 97로 쓰든 메모리나 시간 상으로 큰 차이는 없었다</p>
<blockquote>
<p><a href="https://www.acmicpc.net/problem/10809">https://www.acmicpc.net/problem/10809</a></p>
</blockquote>