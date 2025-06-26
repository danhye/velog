<p>[5-9 실습] - 임의의 값으로 배열 채우기
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/65988f58-b5a8-466c-ac6a-ed258427af9e/image.png" /></p>
<h4 id="버블-정렬bubble-sort">버블 정렬(bubble sort)</h4>
<p>[5-10 실습] - 정렬하기(sort)</p>
<pre><code>package Array;

public class ArrayEx10 {
    public static void main(String[] args) {
        int[] numArr = new int[10];

        for(int i=0;i&lt;numArr.length;i++) {
            /*
             numArr[i] = (int)(Math.random() * 10);
             System.out.print(nummArr[i]);
             를 하나로 합친 것
             */
            System.out.print(numArr[i] = (int)(Math.random() * 10));
        }//end of for
        System.out.println();

        for(int i=0;i&lt;numArr.length-1;i++) {
            boolean changed = false; //자리바꿈이 발생했는지 체크한다.
            //매 반복마다 changed를 false로 초기화한다.

            for(int j=0;j&lt;numArr.length-1-i;j++) {
                if(numArr[j] &gt; numArr[j+1]) { //옆의 값이 작으면 서로 바꾼다.
                    // numArr[j]와 바로 옆의 요소 numArr[j+1]을 비교한다.
                    int temp = numArr[j];
                    numArr[j] = numArr[j+1];
                    numArr[j+1] = temp;
                    changed = true; //자리바꿈이 발생했으니 changed를 true로!
                }//end of if
            }//end of for j

            if(!changed) break; //자리바꿈이 없으면 반복문을 벗어난다.

            for(int k=0;k&lt;numArr.length;k++) {
                System.out.print(numArr[k]); //정렬된 최종 결과를 출력
            }//end of for k
            System.out.println();

        }//end of for i
    }
}</code></pre><p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/303e05a7-bb5a-4097-ab8f-d76d55b3c656/image.png" />
배열의 첫 번째부터 n-1까지의 요소에 대해, 근접한 값과 크기를 비교하여 자리 바꿈을 반복하는 예제.</p>
<p>왼쪽 요소의 값이 크면, 두 값의 위치를 바꾸고 그렇지않으면 바꾸지 않는다.
그래서 제일 큰 값이 맨 마지막에 위치하도록.</p>
<p>첫번째 배열 : 비교횟수는 모두 4번 = numArr.length - 1
두번째 배열 : 비교횟수 3번 
...
마지막 배열 : 최대값. 비교 안해도 됨.
이처럼 비교작업(for문) 반복할 수록 비교해야하는 범위는 하나씩 줄어든다. 
-&gt; <em>numArr.length - 1 <strong>- i</strong></em></p>
<p>[5-11 실습] - 빈도수 구하기
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/fdb86140-e4b7-4bc7-aad0-f087ccc636f8/image.png" /></p>
<h3 id="string배열">String배열</h3>
<h4 id="string배열의-선언과-생성">String배열의 선언과 생성</h4>
<p>String[] name = new String[3]; //3개의 문자열을 담을 수 있는 배열 생성</p>
<pre><code>이렇게 생성 가능! 

String[] name = new String[]{&quot;Kim&quot;,&quot;Lee&quot;,&quot;Park&quot;}; 
String[] name = {&quot;Kim&quot;,&quot;Lee&quot;,&quot;Park&quot;};

String[] name = new String[3];
name[0] = new String(&quot;Kim&quot;);
name[1] = new String(&quot;Lee&quot;);
name[2] = new String(&quot;Park&quot;);

String[] name = new String[3];
name[0] = &quot;Kim&quot;;
name[1] = &quot;Lee&quot;;
name[2] = &quot;Park&quot;;</code></pre><h4 id="기본값">기본값</h4>
<pre><code>자료형                   기본값
boolean                   false
char                '\u0000'
byte,short,int            0
long                    0L
float                   0.0f
double                0.0d or 0.0
참조형 변수               null</code></pre><p>[5-13 실습]
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/e6fbcf2f-a938-419d-8cc9-c9294569408d/image.png" /></p>