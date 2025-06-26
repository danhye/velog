<h3 id="연산-순서">연산 순서</h3>
<p>(먼저) <strong>단항 &gt; 산술 &gt; 비교 &gt; 논리(&amp;&amp; &lt; ||) &gt; 삼항 &gt; 대입</strong> (나중에)</p>
<h3 id="산술변환">산술변환</h3>
<p>연산 수행 직전 발생하는 피연산자의 자동 형변환</p>
<p>1) 두 피연산자의 타입을 같게 일치시킨다. (큰 타입으로)
2) 피연산자의 타입이 int보다 작은 타입이면 int로 변환한다.</p>
<h3 id="부호-연산자-">부호 연산자(+,-)</h3>
<p>부호를 반대로 변경</p>
<pre><code>i = -10;
i = -i;
system.out.println(i); //10 </code></pre><h3 id="비교연산자">비교연산자</h3>
<p>문자열 비교할 때 -&gt; <strong>equals()</strong>
대소문자 구별하지 않고, 문자열 비교 -&gt; <strong>equalsIgnoreCase()</strong></p>
<pre><code>String str1 = &quot;abc&quot;;
String str2 = new String(&quot;abc&quot;);

System.out.printf
(&quot;str1.equals(\&quot;abc&quot;\) ? %b%n&quot;, str2.equals(&quot;abc&quot;));
// 실행결과: str1/equals(&quot;abc&quot;) ? true

System.out.printf
(&quot;str2.equalsIgnoreCase(\&quot;ABC&quot;\ ? %b%n&quot;, str2.equalsIgnoreCase(&quot;ABC&quot;));
//실행결과: str2.equalsIgnoreCase(&quot;ABC&quot;) ? true</code></pre><p>[3-27 실습]
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/869a5378-b071-471d-802c-2f6964df92e8/image.png" /></p>
<h3 id="비트연산자----">비트연산자 &amp; | ^ ~ &lt;&lt; &gt;&gt;</h3>
<p>: 2진수로 표현했을 때 비트의 값을 비교</p>
<p><strong>|(OR연산자)</strong> : 피연산자 중 한 쪽의 값이 1이면, 1. 그외에는 0.
<strong>&amp;(AND연산자)</strong> : 피연산자 중 양 쪽의 값이 모두 1이여만 1. 그외에는 0.
<strong>^(XOR연산자)</strong> : 피연산자의 값이 서로 다를 때 1. 같을 때 0.</p>
<p>[3-28 실습]
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/b765bf1f-d73f-438a-83d9-4e543dc3b92d/image.png" />
<strong>~(비트 전환 연산자)</strong> : 0은 1로, 1은 0으로  </p>
<p>[3-29 실습]
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/fc114385-91e2-4f4d-9d78-612b2f51a899/image.png" /></p>
<p><strong>쉬프트 연산자 &lt;&lt; &gt;&gt;</strong>
<strong>&lt;&lt;</strong>: 왼쪽으로 이동시키고 오른쪽 0으로 채우기
ex) 8(00001000) &lt;&lt; 2 = 00100000
<strong>&gt;&gt;</strong>: 오른쪽은 버리고, 왼쪽은 음수일 경우 1로 채우기 or 양수일 경우 0으로 채우기
ex) -8 &gt;&gt; 2 = 11111110
8 &gt;&gt; 2 = 00000010</p>
<p><strong>x &lt;&lt; n = x * 2ⁿ
x &gt;&gt; n = x / 2ⁿ</strong></p>
<h3 id="대입연산자">대입연산자</h3>
<p>대입연산자의 결합방향은 오른쪽 -&gt; 왼쪽</p>