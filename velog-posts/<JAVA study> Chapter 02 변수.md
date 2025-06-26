<p><em>잊어먹기 싫어서 나혼자 복습하는 JAVA STUDY 오늘부터 렛츠꼬!</em></p>
<h3 id="상수와-리터럴constant--literal">상수와 리터럴(constant &amp; literal)</h3>
<p><strong>상수</strong>는 반드시 선언과 동시에 초기화. 주로 대문자 사용</p>
<pre><code>final int MAX_VALUE = 100;</code></pre><h3 id="리터럴의-타입과-접미사">리터럴의 타입과 접미사</h3>
<pre><code>int bitNum = 0b10; //2진수 10, 2진수 표시 - 0b
int octNum = 010;  //8진수 10, 8진수 표시 - 0
int hexNum = 0x10; //16진수 10, 16진수 표시 - 0x, 0X</code></pre><p>리터럴에 소수점이나 10의 제곱 - e,E </p>
<pre><code>/*
자료형        실수형 리터럴        다른 형태의 동등한 표현
double         10.                10.0
double        .10                0.10
float        10f                10.0f
float        3.14e3f            3140.0f
float        1e1                10.0
float        1e-3            0.001
*/</code></pre><h3 id="문자-리터럴과-문자-리터럴">문자 리터럴과 문자 리터럴</h3>
<p>기본형 타입의 값을 문자열로 변환할 때는 아무런 내용도 없는 빈 문자열(&quot;&quot;)을 더해주면 된다.</p>
<pre><code>System.out.println(&quot;&quot; + 7 + 7); //실행결과: 77</code></pre><p>줄바꿈 문자 : %n , \n</p>
<h3 id="printf-지시자">printf() 지시자</h3>
<pre><code>/*
지시자            설명
%b            boolean
%d            10진 정수(decimal)
%o            8진 정수(octal)
%x,%X        16진 정수(hexa-decimal)
%f            부동소수점(floating-point), 
            소수점 아래 6자리까지만 출력
%e,%E        지수(exponent)
%c            character
%s            string
*/
</code></pre><p>[2-4실습]
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/71ca3cf2-2fe3-4229-9615-b0f9d3f55d0b/image.png" />
[2-5실습]
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/fd1f500f-ada6-4a62-9337-d03cdc8ae831/image.png" /></p>
<h3 id="scanner">Scanner</h3>
<pre><code>Scanner scanner = new Scanner(System.in); // Scanner 객체 생성 해야해!!

//#1 - 입력받고 형변환 (추천)
String input = scanner.nextLine();
int num = Integer.parseInt(input);

//#2 - 바로 형변환
int num = scanner.nextInt();</code></pre><p>[2-6실습]
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/c77120c6-bc98-4d2d-afd2-426054e17abe/image.png" /></p>
<h3 id="특수문자-다루기">특수문자 다루기</h3>
<pre><code>특수문자            문자리터럴
tab                    \t
backspace            \b
form feed            \f
new line            \n
carriage return        \r    - 맨앞으로 이동
역슬래쉬(\)             \\
작은따옴표             \'
큰따옴표                 \&quot;
유니코드(16진수)        \u유니코드</code></pre><p>[2-8실습]
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/54d13e58-7a03-468a-85a6-67d111e41376/image.png" /></p>