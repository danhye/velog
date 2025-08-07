<h3 id="문제">문제</h3>
<p>달팽이는 1부터 N*N까지의 숫자가 시계방향으로 이루어져 있다.</p>
<p>다음과 같이 정수 N을 입력 받아 N크기의 달팽이를 출력하시오.</p>
<p><strong>[제약사항]</strong>
달팽이의 크기 N은 1 이상 10 이하의 정수이다. (1 ≤ N ≤ 10)</p>
<p>``` java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;</p>
<p>public class Solution {</p>
<pre><code>//이동방향 (우 -&gt; 하 -&gt; 좌 -&gt; 상)
//행
static int[] dr = {0, 1, 0, -1};
//열
static int[] dc = {1, 0, -1, 0};


public static void main(String[] args) throws NumberFormatException, IOException {
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

    int testCase = Integer.parseInt(br.readLine());
    for (int t = 0; t &lt; testCase; t++) {

        int n = Integer.parseInt(br.readLine()); 
        int[][] snail = new int[n][n];

        int r = 0, c = 0, dir = 0;
        for(int i = 1; i &lt;= n * n; i++) {
            snail[r][c] = i;

            //다음 위치 계산
            int nr = r + dr[dir];
            int nc = c + dc[dir];

            //배열을 벗어날 경우
            if(nr &lt; 0 || nr &gt;= n || nc &lt; 0 || nc &gt;= n || snail[nr][nc] != 0 ) {
                // 방향 전환
                dir = (dir + 1) % 4;
                nr = r + dr[dir];
                nc = c + dc[dir];
            }
            r = nr;
            c = nc;
        }

        System.out.println(&quot;#&quot; + (t+1));
        for (int i = 0; i &lt; snail.length; i++) {
            for (int j = 0; j &lt; snail.length; j++) {
                System.out.print(snail[i][j] + &quot; &quot;);
            }
            System.out.println();
        }

    }//테스트케이스 끝
}</code></pre><p>}</p>