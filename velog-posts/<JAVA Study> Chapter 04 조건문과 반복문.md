<h3 id="if문">if문</h3>
<p>[4-4 실습]
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/f6d4a186-f702-4e12-853c-41e2faa6ce85/image.png" />
[4-5 실습]
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/b1116a01-dc8d-47e3-9a40-0250d6c710f4/image.png" /></p>
<h3 id="switch문">switch문</h3>
<p><strong>switch문의 제약조건</strong></p>
<ol>
<li><p>switch문의 조건식 결과는 정수 또는 문자열이어야 한다.</p>
</li>
<li><p>case문의 값은 정수 상수만 가능하며, 중복되지 않아야한다.</p>
<pre><code>public static void main(String[] args){
 int num, result;
 final int one = 1;

 switch(result){
     case '1': //가능. 문자 리터럴(정수 상수 49와 동일)
     case one:  //가능. final이 붙은 정수 상수
     case &quot;YES&quot;: //가능. 문자 리터럴
     case num: //불가능. 변수는 안돼
     case 1.0: //불가능. 실수 안돼
 }
}</code></pre><p>[4-6 실습]<img alt="" src="https://velog.velcdn.com/images/danhye821/post/2a4a4a38-dccb-4b11-a2ef-9b72b2fa834c/image.png" />
[4-7 실습]<img alt="" src="https://velog.velcdn.com/images/danhye821/post/3a3cf01f-21e7-462f-b391-540965e3dee3/image.png" /></p>
</li>
</ol>
<p>*<em>(int)(Math.random() * 3) + 1 *</em>: 1,2,3 중에 랜덤으로 선택!</p>
<p>[4-8 실습]<img alt="" src="https://velog.velcdn.com/images/danhye821/post/c2fb0d60-943a-49ce-8710-89c6d3c4ef26/image.png" />
<strong>charAt(x)</strong> : x+1번째 문자를 추출(0번째 부터 숫자를 세기 때문에 ?+1)</p>
<p>[4-9 실습]<img alt="" src="https://velog.velcdn.com/images/danhye821/post/700f16b5-99b8-404f-ab47-053d97556873/image.png" />[4-4 실습 switch ver.] 
switch(여기안에 연산 가능) 
int / int = int -&gt; 89 / 10 = 8</p>
<h3 id="반복문---for-while-do-while">반복문 - for, while, do-while</h3>
<p>i += 2 : 2씩 증가
I *= 3 : 3배씩 증가</p>
<p>for(int i=1,j=10;i&lt;=10;i++,j--){...} : i,j 모두 조건 줄 수 있다
for(;;) : 초기화, 조건식, 증감식 모두 생략, 조건식은 참이 된다.</p>
<p>[4-17 실습] - 별 출력
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/bca124f9-a321-4258-8dc4-5a73dd855c2b/image.png" />
[4-18 실습] -  구구단
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/4e847ada-a25c-4b75-b9cc-5e9be9c2b805/image.png" />
[4-21 실습]
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/38f99ec7-abc8-46b2-8e3c-7190c1e3e9a8/image.png" /></p>
<h3 id="향상된-for문">향상된 for문</h3>
<p><strong>for( <em>타입 변수명</em> : <em>배열 또는 컬렉션</em> ) { 반복할 문장 }</strong>
: 위의 문장에서 타입은 배열 또는 컬렉션의 요소의 타입이어야 한다.
배열 또는 컬렉션에 저장된 값이 매 반복마다 하나씩 순서대로 읽혀서 변수에 저장된다.
But, 일반적인 for문과 달리 배열이나 컬렉션에 저장된 요소들을 읽어오는 용도로만 사용 가능하다.</p>
<pre><code>int[] arr = {10,20,30,40,50};

for(int i=0;i &lt; arr.length; i++){
    System.out.print(arr[i]); // 10 20 30 40 50
 }</code></pre><p>이것과</p>
<pre><code>for(int tmp : arr){
    System.out.print(tmp); // 10 20 30 40 50![](https://velog.velcdn.com/images/danhye821/post/c8905ee8-af87-4eab-a410-90ae3b71ffde/image.png)

}</code></pre><p>이건 같은 결과값! </p>
<h3 id="while문">while문</h3>
<p>[4-27 실습]
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/5922feed-4142-4cbf-b876-5fb363bb5ff3/image.png" /></p>
<h3 id="do-while문">do-while문</h3>
<p><strong>do{
        **//조건식의 결과가 true일 때, 수행할 문장</strong>
    }while(조건식);** //조건식의 결과가 false일때 빈 문장; 끝나고 반복문  out</p>
<p>[4-28 실습]
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/7d3bc26a-381f-432e-af5c-77899ced43bd/image.png" />
[4-29 실습] - 3,6,9 짝
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/285e2f5a-9ebf-4707-b605-bac430d4c50c/image.png" /></p>
<h3 id="break문">break문</h3>
<p>자신이 포함된 가장 가까운 반복문 벗어남.</p>
<h3 id="continue문">continue문</h3>
<p>반복문 내에서만 사용될 수 있으며, 반복이 진행되는 도중에 continue문을 만나면 반복문의 끝으로 이동하여 다음 반복으로 넘어간다.</p>
<p>반복문 전체를 벗어나는 것 X
다음 반복을 계속 수행 O</p>
<p>[4-32 실습]<img alt="" src="https://velog.velcdn.com/images/danhye821/post/f88314ef-bef6-4106-b220-d42e411af353/image.png" /></p>
<h3 id="이름-붙은-반복문">이름 붙은 반복문</h3>
<p>break문 - 근접한 단 하나의 반복문만 벗어날 수 있음.</p>
<p><strong>여러개의 반복문이 중첩된 경우</strong>, 
중첩 반복문 앞에 이름을 붙이고 break문, continue문에 이름을 지정해주면 
-&gt; 하나 이상의 반복문 벗어나거나 반복을 건너뛸 수 있다.</p>
<p>[4-33 실습]
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/0612de05-a309-49c6-bb36-ed4c588e1a58/image.png" />
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/eb7cd21c-a346-42c7-9e31-d194cbca767d/image.png" />
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/f20e17bf-40ff-48d8-b305-7efae49f41fd/image.png" />
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/919ce496-904b-416c-98fc-bd0fad5f16a3/image.png" /></p>
<p>[4-34 실습]</p>
<pre><code>package Flow;

import java.util.Scanner;

public class FlowEx34 {
    public static void main(String[] args) {
        int menu = 0, num =0;

        Scanner scanner = new Scanner(System.in);

        outer:
        while(true) {
            System.out.println(&quot;(1) square&quot;);
            System.out.println(&quot;(2) square root&quot;);
            System.out.println(&quot;(3) log&quot;);
            System.out.print(&quot;원하는 메뉴(1~3)를 선택하세요.(종료:0) &gt; &quot;);

            String tmp = scanner.nextLine();
            menu = Integer.parseInt(tmp);

            if(menu==0) {
                System.out.println(&quot;프로그램을 종료합니다.&quot;);
                break;
            }else if(!(1&lt;=menu &amp;&amp; menu &lt;=3)) {
                System.out.println(&quot;메뉴를 잘 못 선택하셨습니다.(종료는 0)&quot;);
                continue;
            }

            for(;;) { //조건식 참
                System.out.print(&quot;계산할 값을 입력하세요.(계산 종료:0, 전체 종료:99) &gt; &quot;);
                tmp = scanner.nextLine();
                num = Integer.parseInt(tmp);

                if(num==0)
                    break; //계산종료. for문을 벗어난다. 가장 근접한 반복문. if문은 반복문이 X
                if(num==99)
                    break outer; //전체종료.
                switch(menu) {
                    case 1:
                        System.out.println(&quot;result=&quot; + num*num);
                        break;
                    case 2:
                        System.out.println(&quot;result=&quot; + Math.sqrt(num));
                        break;
                    case 3:
                        System.out.println(&quot;result=&quot; + Math.log(num));
                        break;
                }
            }//end of for(;;)
        }//end of while, outer
    }//end of main
}
</code></pre><p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/bc6b8d88-fa9d-4c8a-b385-aa36941580a5/image.png" /></p>