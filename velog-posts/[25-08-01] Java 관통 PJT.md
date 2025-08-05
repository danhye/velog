<h1 id="json--gson">JSON &amp; GSON</h1>
<h2 id="json">JSON</h2>
<ul>
<li>데이터를 <strong>키-값</strong> 쌍의 구조로 표현</li>
<li>텍스트 기반으로 저장하고 전송하기 위한 표준 포맷</li>
<li>경량 데이터 교환 형식</li>
<li>언어 독립적인 데이터 형식</li>
<li>설정파일, 데이터 교환(API 응답), 데이터 저장 등에 사용</li>
<li>XML보다 간결하며, 데이터를 읽고 쓰는데 더 효율적임</li>
</ul>
<blockquote>
<p><strong>XML ?</strong> 데이터를 사람이 읽기 쉬운 텍스트 형태로 기술/교환하기 위해 만든 태그 기반 마크업 언어</p>
</blockquote>
<h2 id="json의-데이터-타입">JSON의 데이터 타입</h2>
<ol>
<li>문자열 String </li>
<li>숫자 Number</li>
<li>불리언 Boolean</li>
<li>객체 Object</li>
<li>배열 Array</li>
<li>null 데이터가 없음</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/3feafadd-c2a3-4142-ab7b-67573335a77f/image.png" /></p>
<ul>
<li>Key는 문자열 !</li>
<li>배열은 [] (대괄호) !</li>
<li>전체를 {} 로 감싸서 전송 → 문자열임 !</li>
</ul>
<h2 id="json의-문법">JSON의 문법</h2>
<ul>
<li>{”key” : value, …}</li>
<li>키는 항상 문자열</li>
<li>값은 문자열, 숫자, 불리언, 객체, 배열, null 일 수 있음</li>
<li>, Comma를 이용하여 구분 (마지막 값 뒤에는 작성 X)</li>
<li>중첩 가능</li>
<li>주석 미지원</li>
</ul>
<hr />
<h1 id="gson">GSON</h1>
<ul>
<li>구글에서 제공하는 Java기반 JSON 라이브러리</li>
<li>JSON 데이터 → Java 객체 / Java 객체 → JSON 데이터</li>
</ul>
<blockquote>
<p><strong>라이브러리(Library)?</strong>
특정 기능을 미리 구현해 둔 재사용 가능한 코드 집합</p>
</blockquote>
<ul>
<li>생상성 향상</li>
<li>코드 중복 감소</li>
<li>유지 보수 용이<blockquote>
</blockquote>
</li>
</ul>
<h2 id="gson-특징">GSON 특징</h2>
<ul>
<li>간단한 사용법</li>
<li>다양한 데이터 구조 지원</li>
<li>객체(클래스, instance) 와 JSON 간 매핑 → 필드 이름을 기준으로 Java 객체와 JSON 데이터를 자동 매핑</li>
<li>복잡한 데이터 구조 처리 → 중첩된 객체, 배열, 제네릭 등을 지원</li>
<li>Java 표준 타입 지원 → 기본 데이터 타입과 컬렉션(List, Map 등) 을 쉽게 처리</li>
</ul>
<pre><code class="language-java">package test01_gson;

import com.google.gson.Gson;

public class GsonTest01 {
    //GSON 등록해야 사용이 가능하다.
    //1. 직접 추가 (jar를 이용하여 등록)
    //2.maven pjt 등록하는 방법
    public static void main(String[] args) {
        //GSON 얘를 통해서 바꿀거야 !
        Gson gson = new Gson();


        //Java 객체를 JSON 데이터로 ..
        Student st = new Student(&quot;양띵균&quot;, &quot;컴퓨터공학&quot;, &quot;자바비전공&quot;);
        System.out.println(st);

        //GSON을 통해 만들어진 아이들은 모두 String
        String stJson = gson.toJson(st);
        System.out.println(stJson);
    }
}
</code></pre>
<p>[출력]
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/5345ee16-02b2-4cbf-a217-ef089f71c56c/image.png" /></p>
<blockquote>
<p>위 : toString 형태 / 아래 : Json 형태</p>
</blockquote>
<pre><code class="language-java">//다시 JSON 구했어 ! -&gt; Student 로 바꾸자
Student st2 = gson.fromJson(stJson, Student.class);
System.out.println(st2);</code></pre>
<pre><code class="language-java">package test01_gson;

import java.lang.reflect.Type;
import java.util.ArrayList;
import java.util.List;

import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;

public class GsonTest02 {
    //GSON 등록해야 사용이 가능하다.
    //1. 직접 추가 (jar를 이용하여 등록)
    //2.maven pjt 등록하는 방법
    public static void main(String[] args) {
        //GSON 얘를 통해서 바꿀거야 !
        Gson gson = new Gson();

        //Java 객체를 JSON 데이터로 ..
        Student st1 = new Student(&quot;양띵균&quot;, &quot;컴퓨터공학&quot;, &quot;자바비전공&quot;);
        Student st2 = new Student(&quot;이단혜&quot;, &quot;아동가족복지학과&quot;, &quot;자바비전공&quot;);
        Student st3 = new Student(&quot;이재용&quot;, &quot;CEO학과&quot;, &quot;자바비전공&quot;);
        Student st4 = new Student(&quot;뿡뿡이&quot;, &quot;방구학과&quot;, &quot;자바비전공&quot;);

        //여러개의 객체를 저장하는 방법은 2가지 정도
        //1. 객체 배열
        Student[] sArr = new Student[4];
        sArr[0] = st1;
        sArr[1] = st2;
        sArr[2] = st3;
        sArr[3] = st4;

        System.out.println(&quot;=========객체배열 -&gt; JSON=========&quot;);
        String arrStr = gson.toJson(sArr);
        System.out.println(arrStr);
        System.out.println();

        System.out.println(&quot;=========JSON -&gt; 객체배열=========&quot;);
        Student[] sArr2 = gson.fromJson(arrStr, Student[].class);
        for(Student s : sArr2) {
            System.out.println(s);
        }
        System.out.println();

        //======================================================================
        //2. 객체 리스트
        List&lt;Student&gt; sList = new ArrayList&lt;&gt;();
        sList.add(st1);
        sList.add(st2);
        sList.add(st3);
        sList.add(st4);

        System.out.println(&quot;=========객체리스트 -&gt; JSON=========&quot;);
        String listStr = gson.toJson(sList);
        System.out.println(listStr);
        System.out.println();

        System.out.println(&quot;=========JSON -&gt; 객체리스트=========&quot;);
        Type studentListType = new TypeToken&lt;ArrayList&lt;Student&gt;&gt;() {}.getType();
        ArrayList&lt;Student&gt; sList2 = gson.fromJson(listStr, studentListType);
        for(Student st : sList2) {
            System.out.println(st);
        }
        System.out.println();
    }
}</code></pre>
<pre><code class="language-java">=========객체배열 -&gt; JSON=========
[{&quot;studentId&quot;:1,&quot;name&quot;:&quot;양띵균&quot;,&quot;major&quot;:&quot;컴퓨터공학&quot;,&quot;track&quot;:&quot;자바비전공&quot;},{&quot;studentId&quot;:2,&quot;name&quot;:&quot;이단혜&quot;,&quot;major&quot;:&quot;아동가족복지학과&quot;,&quot;track&quot;:&quot;자바비전공&quot;},{&quot;studentId&quot;:3,&quot;name&quot;:&quot;이재용&quot;,&quot;major&quot;:&quot;CEO학과&quot;,&quot;track&quot;:&quot;자바비전공&quot;},{&quot;studentId&quot;:4,&quot;name&quot;:&quot;뿡뿡이&quot;,&quot;major&quot;:&quot;방구학과&quot;,&quot;track&quot;:&quot;자바비전공&quot;}]

=========JSON -&gt; 객체배열=========
Student [studentId=1, name=양띵균, major=컴퓨터공학, track=자바비전공]
Student [studentId=2, name=이단혜, major=아동가족복지학과, track=자바비전공]
Student [studentId=3, name=이재용, major=CEO학과, track=자바비전공]
Student [studentId=4, name=뿡뿡이, major=방구학과, track=자바비전공]

=========객체리스트 -&gt; JSON=========
[{&quot;studentId&quot;:1,&quot;name&quot;:&quot;양띵균&quot;,&quot;major&quot;:&quot;컴퓨터공학&quot;,&quot;track&quot;:&quot;자바비전공&quot;},{&quot;studentId&quot;:2,&quot;name&quot;:&quot;이단혜&quot;,&quot;major&quot;:&quot;아동가족복지학과&quot;,&quot;track&quot;:&quot;자바비전공&quot;},{&quot;studentId&quot;:3,&quot;name&quot;:&quot;이재용&quot;,&quot;major&quot;:&quot;CEO학과&quot;,&quot;track&quot;:&quot;자바비전공&quot;},{&quot;studentId&quot;:4,&quot;name&quot;:&quot;뿡뿡이&quot;,&quot;major&quot;:&quot;방구학과&quot;,&quot;track&quot;:&quot;자바비전공&quot;}]

=========JSON -&gt; 객체리스트=========
Student [studentId=1, name=양띵균, major=컴퓨터공학, track=자바비전공]
Student [studentId=2, name=이단혜, major=아동가족복지학과, track=자바비전공]
Student [studentId=3, name=이재용, major=CEO학과, track=자바비전공]
Student [studentId=4, name=뿡뿡이, major=방구학과, track=자바비전공]
</code></pre>
<p>[참고] 클래스 다이어그램 내 접근 제한
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/e1d9115b-dedb-4f23-b9b5-6cfeab5993fd/image.png" /></p>
<ul>
<li>실습실 클래스 다이어그램 : 아마테라스 tool 로 생성</li>
</ul>