<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/c74929f4-3514-4480-9e47-cfe8c1f5063c/image.png" />
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/5389c8f9-8bb9-4fc6-95af-6835e7def009/image.png" /></p>
<p>파라미터로 값을 받아와서 출력만 하면 되는 너무나 간단한 문제였는데...
나는 그런줄도 모르고 굳이굳이 BufferedReader 사용해서 풀었다.
뭐 공부했으니까 ^__^</p>
<p>그럼 먼저 프로그래머스에서 정답 처리한 풀이!</p>
<pre><code>class Solution {
    public int[] solution(int[] num_list, int n) {
        int[] answer = new int[n];
        for(int i=0;i&lt;n;i++){
            answer[i] = num_list[i];
            System.out.print(answer[i]);
        }
        return answer;
    }
}</code></pre><p>하지만 다른 사람들 풀이를 보다 보니 이것보다 간단한 방법이 있었다.</p>
<pre><code>import java.util.Arrays;

class Solution {
    public int[] solution(int[] num_list, int n) {
        int[] answer = new int[n];
        answer = Arrays.copyOfRange(num_list,0,n);
        return answer;
    }
}</code></pre><p>✅ <strong>Arrays.copyOfRange(배열, 시작인덱스, 끝인덱스)</strong></p>
<p>0: 시작 인덱스 (포함됨)
n: 끝 인덱스 (❗ 포함되지 않음 = n-1까지 복사됨)</p>
<p>그리고 내가 푼 풀이
무려 2가지 방법으로 풀었다고^ㅠ^</p>
<ol>
<li>split 사용<pre><code>import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
</code></pre></li>
</ol>
<p>public class algorithm_1 {
    public static void main(String[] args) throws IOException {</p>
<pre><code>    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

    String line = br.readLine();
    String[] lines = line.split(&quot; &quot;);

    int[] num_list = new int[lines.length];
    for(int i=0; i&lt;lines.length; i++) {
        num_list[i] = Integer.parseInt(lines[i]);
    }

    int n = Integer.parseInt(br.readLine());
    int[] result = new int[n];

    for(int j=0;j&lt;n;j++) {
        result[j] = num_list[j];
        System.out.print(result[j]);
    }

}</code></pre><p>}</p>
<pre><code>

2. StringTokenizer 사용</code></pre><p>import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;</p>
<p>public class algorithm_1 {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());</p>
<pre><code>    int[] num_list = new int[st.countTokens()];

    int i=0;
    while(st.hasMoreTokens()) {
        num_list[i++] = Integer.parseInt(st.nextToken());
    }

    int n = Integer.parseInt(br.readLine());
    for(int j=0;j&lt;n;j++) {
        System.out.print(num_list[j]);
    }

}</code></pre><p>}</p>
<p>```
✅ StringTokenizer란?
문자열을 구분자(delimiter) 기준으로 토큰(token) 으로 나눠주는 클래스
공백, 쉼표, 콜론 등 어떤 문자로든 나눌 수 있다.
한 번에 모두 나누지 않고, 필요할 때 하나씩 꺼내쓸 수 있는 방식.</p>
<p>✅ hasMoreTokens()란?
아직 읽지 않은 토큰이 남아있는지 확인하는 메서드
→ 즉, true면 꺼낼 값이 아직 있고, false면 다 읽었다는 뜻</p>
<p>공부했으니까 된거겠지 ~</p>