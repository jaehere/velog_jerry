<p><code>Lv. 2</code></p>
<blockquote>
<p><a href="https://softeer.ai/practice/9657">https://softeer.ai/practice/9657</a></p>
</blockquote>
<h3 id="첫번째-코드">첫번째 코드</h3>
<pre><code>import java.io.*;
import java.util.*;

public class Main {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();

        int[][] grid = new int[n][m];

        for(int i=0; i&lt;n; i++){
            for(int j=0; j&lt;m; j++){
                grid[i][j] = sc.nextInt();
            }
        }

        int L1 = sc.nextInt() - 1;
        int R1 = sc.nextInt() - 1;

        attack(grid, L1, R1, m);

        int L2 = sc.nextInt() - 1;
        int R2 = sc.nextInt() - 1;

        sc.close();

        attack(grid, L2, R2, m);

        System.out.println(countRemaining(grid, n, m));

    }

    public static void attack(int[][] grid, int l, int r, int m) {
        for(int i=l; i&lt;=r; i++) {
            for(int j=0; j&lt;m; j++) {
                if(grid[i][j] == 1){
                    grid[i][j] = 0;
                    break;
                }
            }
        }
    }

    public static int countRemaining(int[][] grid, int n, int m) {
        int cnt = 0;
        for(int i=0; i&lt;n; i++){
            for(int j=0; j&lt;m; j++){
                if(grid[i][j] == 1) {
                    cnt++;
                }
            }
        }
        return cnt;
    }


}
</code></pre><p><img alt="" src="https://velog.velcdn.com/images/jaehere/post/f12645d0-114d-420f-b262-156768aecae9/image.png" /></p>
<p>시간단축이나 메모리 절약 측면도 고려해봐야겠다.</p>
<hr />
<h3 id="두번째-코드">두번째 코드</h3>
<pre><code>import java.io.*;
import java.util.*;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());

        int[][] grid = new int[n][m];

        for (int i = 0; i &lt; n; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j &lt; m; j++) {
                grid[i][j] = Integer.parseInt(st.nextToken());
            }
        }
        st = new StringTokenizer(br.readLine());
        // 첫번째 공격
        int L1 = Integer.parseInt(st.nextToken()) - 1;
        int R1 = Integer.parseInt(st.nextToken()) - 1;

        attack(grid, L1, R1, m);

        st = new StringTokenizer(br.readLine());
        // 두번째 공격
        int L2 = Integer.parseInt(st.nextToken()) - 1;
        int R2 = Integer.parseInt(st.nextToken()) - 1;
        attack(grid, L2, R2, m);

        System.out.println(countRemaining(grid, n, m));


    }


    private static int countRemaining(int[][] grid, int n, int m) {
        int sum = 0;
        for (int i = 0; i &lt; n; i++) {
            for (int j = 0; j &lt; m; j++) {
                if (grid[i][j] == 1) {
                    sum++;
                }
            }
        }
        return sum;
    }

    private static void attack(int[][] grid, int L1, int R1, int m) {
        for (int i = L1; i &lt;= R1; i++) {
            for (int j = 0; j &lt; m; j++) {
                if (grid[i][j] == 1) {
                    grid[i][j] = 0;
                    break;
                }
            }
        }
    }

}</code></pre><p><img alt="" src="https://velog.velcdn.com/images/jaehere/post/7c0caea8-cdf4-41c1-bde5-8ccf97b28fe7/image.png" /></p>
<p>첫번째 코드에서 Scanner 사용부분만 BufferedReader,StringTokenizer 조합으로 변경해보았다. 실행시간이나 메모리가 많이 줄어드는 효과가 컸다.</p>
<p>BufferedReader는 내부적으로 8KB 버퍼(기본값)를 사용하여 한꺼번에 데이터를 읽어온다.
반면, Scanner는 한 글자씩 읽어서 처리하기 때문에 입출력 속도가 느려진다.
특히 반복문에서 Scanner로 여러 번 읽는 경우 차이가 더 심해진다고 한다.</p>
<p>대신 BufferedReader는 의 readLine()은 반환타입이 String 으로 고정되어 있다. 
꼭 사용용도에 맞는 형변환을 해주어야 한다.</p>
<p>예외처리도 필요하다.<code>try/catch</code> 나 <code>throws IOException</code> 처리를 해주어야 한다. </p>
<ul>
<li><p>클래스 import 추가
import java.io.IOException; </p>
</li>
<li><p>main 클래스 옆에 throws IOException 처리
public static void main(String[] args) throws IOException {}</p>
</li>
</ul>
<p>🧚 같은 로직이라도 사용하는 클래스와 메서드에 따라 성능 차이가 크므로, 최적의 방법을 선택하는 것이 중요하군</p>