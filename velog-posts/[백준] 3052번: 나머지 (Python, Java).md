<p>*카운트 배열 이용</p>
<h3 id="문제">문제</h3>
<p>두 자연수 A와 B가 있을 때, A%B는 A를 B로 나눈 나머지 이다. 예를 들어, 7, 14, 27, 38을 3으로 나눈 나머지는 1, 2, 0, 2이다.</p>
<p>수 10개를 입력받은 뒤, 이를 42로 나눈 나머지를 구한다. 그 다음 서로 다른 값이 몇 개 있는지 출력하는 프로그램을 작성하시오.</p>
<h3 id="입력">입력</h3>
<p>첫째 줄부터 열번째 줄 까지 숫자가 한 줄에 하나씩 주어진다. 이 숫자는 1,000보다 작거나 같고, 음이 아닌 정수이다.</p>
<h3 id="출력">출력</h3>
<p>첫째 줄에, 42로 나누었을 때, 서로 다른 나머지가 몇 개 있는지 출력한다.</p>
<h3 id="내-코드">내 코드</h3>
<h4 id="python">Python</h4>
<pre><code>check = [0] * 42

for i in range(10):
  n = int(input())
  check[n % 42] = 1

sum = 0
for i in range(42):
  sum += check[i]

print(sum)</code></pre><h4 id="java">Java</h4>
<pre><code>import java.util.Scanner;

public class Main {
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        int[] arr = new int[42];
        for (int i = 0; i &lt; 10; i++) {
            arr[sc.nextInt() % 42] = 1;
        }

        int sum = 0;
        for (int a : arr) {
            sum += a;
        }
        System.out.println(sum);
    }
}</code></pre><blockquote>
<p><a href="https://www.acmicpc.net/problem/3052">https://www.acmicpc.net/problem/3052</a></p>
</blockquote>