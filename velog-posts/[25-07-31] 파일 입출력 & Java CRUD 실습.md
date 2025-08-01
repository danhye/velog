<p>스트림 = 통로 !</p>
<h1 id="file-클래스">File 클래스</h1>
<h2 id="파일-클래스">파일 클래스</h2>
<ul>
<li>파일 및 디렉토리를 <strong>객체</strong>로서 관리</li>
<li>특정 파일 또는 디렉토리에 대응되는 <strong>객체를 생성</strong></li>
<li>파일 및 디렉토리에 대한 정보를 관리하고 조작할 수 있는 다양한 메서드를 제공</li>
<li>파일 객체의 생성 : 경로를 문자열로 전달, 해당 파일 또는 디렉토리 경로에 대응되는 객체 생성</li>
<li>문자열 경로는 상대 경로 또는 절대 경로를 사용</li>
</ul>
<pre><code class="language-java">// 문자열로 &quot;경로&quot;를 전달 -&gt; 파일 객체 생성!
File file = new File(&quot;파일 또는 디렉토리의 경로&quot;);</code></pre>
<blockquote>
<p>상대 경로
⇒ 현재 작업 중인 디렉토리(working directory)를 기준으로 경로</p>
<ul>
<li>.. : 현재 나를 기준으로 상위 폴더 의미</li>
<li>. : 현재 폴더 의미<ul>
<li>Q. 부모 디렉토리에 있는 cat.jpg 파일에 접근 방법 ? ../cat.jpg</li>
</ul>
</li>
</ul>
</blockquote>
<blockquote>
<p>절대 경로
⇒ 시스템 상의 파일 폴더가 위치해 있는 전체의 경로</p>
</blockquote>
<blockquote>
<p>File.separator 사용 권장 !</p>
</blockquote>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/97536e5c-1550-4fdc-b593-776a985e347c/image.png" /></p>
<pre><code class="language-java">package test01_File;

import java.io.File;

public class FileTest {
    public static void main(String[] args) {
        //File 클래스 : 파일 &amp; 폴더 (디렉토리) 관리하기 위한 클래스
        File file = new File(&quot;C:\\Users\\SSAFY\\Desktop\\새 폴더\\Java_11_파일입출력\\img.png&quot;);

        System.out.println(file.exists());
        System.out.println(file.isFile());
        System.out.println(file.isDirectory());

        System.out.println(file.isAbsolute());
        System.out.println(file.getAbsolutePath()); //절대경로를 반환

         File src = new File(&quot;src&quot;);

         File folder = new File(&quot;folder&quot;); // 이름이 폴더인 디렉토리를 생성한건가?
         System.out.println(folder.exists());
         folder.mkdir();
         System.out.println(folder.exists());

         //파일을 생성할  수도 있고 폴더를 생성하고 관리 할 수도 있다.
    }
}
</code></pre>
<pre><code class="language-java">package day08;

import java.io.File;

public class FileTest {
    public static void main(String[] args) {
        // File 클래스 : 파일 또는 디렉토리를 관리하기 위한 클래스
        File f = new File(&quot;glassmandu.jpg&quot;); // 상대 경로
        System.out.println(f.exists()); // 파일이 존재하면 true (아니면 false)

        System.out.println(f.isFile()); // 파일입니까? true
        System.out.println(f.isDirectory()); //디렉토리입니까? false
        System.out.println(f.isAbsolute()); //절대경로입니까? false
        System.out.println(f.getAbsolutePath()); //절대경로 문자열로 반환
        System.out.println(&quot;-------------------------&quot;);

        File bin = new File(&quot;bin&quot;);
        System.out.println(bin.isDirectory()); //true
        System.out.println(bin.exists()); //true

        // 없는 폴더를 지정
        File folder = new File(&quot;static/2025/07/31&quot;);
        System.out.println(folder.exists()); //false

        //폴더 생성 mkdir vs mkdirs
//        System.out.println(folder.mkdir()); //폴더 생성 성공? false
        // 부모 디렉토리가 존재하는 경우에만 생성할 수 있다. 

        System.out.println(folder.mkdirs()); //true
    }
}
</code></pre>
<h1 id="입출력과-스트림">입출력과 스트림</h1>
<h2 id="입출력-io-input---output">입출력 IO (Input  / Output)</h2>
<h2 id="스트림-stream">스트림 Stream</h2>
<ul>
<li>데이터를 운반하는데 사용되는 통로</li>
<li><strong>단방향</strong> 통신 가능</li>
<li>하나의 스트림을 이용하여 입력과 출력 처리 불가능</li>
<li>데이터의 방향에 따라 입력 스트림, 출력 스트림으로 나뉨</li>
</ul>
<p>→ 입출력 동시에 하고싶다면 ? </p>
<p>⇒ 입력스트림 + 출력스트림 별도로 만들어서 처리 !</p>
<p>이러한 데이터 스트림을 JAVA에서는 다루게 되는 데이터타입에 따라 분류</p>
<h3 id="데이터-타입에-따른-스트림의-분류">데이터 타입에 따른 스트림의 분류</h3>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/f3c9cc9e-760a-4755-ad15-ee18b8e572bf/image.png" /></p>
<ul>
<li>입출력 단위 byte(1byte = 8bit) : 바이트 스트림 <code>InputStream OutputStream</code></li>
<li>입출력 단위 char(2bytes) : 문자 스트림 <code>Reader</code> <code>writer</code></li>
</ul>
<h3 id="스트림의-종류">스트림의 종류</h3>
<ul>
<li>데이터의 타입 : 바이트, 문자</li>
<li>데이터의 방향 : 입력, 출력</li>
<li>노드의 종류 : 표준 입출력, 파일</li>
<li>스트림의 성격 : 노드 스트림, 보조 스트림</li>
</ul>
<h1 id="바이트-스트림-byte-stream">바이트 스트림 Byte Stream</h1>
<ul>
<li>바이트 단위로 데이터를 입력 받거나 출력하기 위한 스트림 클래스<ul>
<li>기본 입출력 (System.in, System.out)</li>
</ul>
</li>
<li>주로 이진 파일을 읽고 쓰는데 사용</li>
<li>최상위 <strong>추상 클래스</strong> : InputStream, OutputStream<ul>
<li>추상 클래스 ? 추상 메서드를 가지고 있는 클래스, 객체(instance)를 생성할 수 없는 클래스</li>
<li>대신, 다형성 때문에 참조는 가능하다 !</li>
</ul>
</li>
<li>노드의 종류에 따라 다양한 구체적인 서브 클래스 사용</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/531d7f62-35a3-4ed4-b443-80abfd3440e1/image.png" /></p>
<h2 id="inputstream-클래스">InputStream 클래스</h2>
<ul>
<li>모든 바이트 입력 스트림의 최상위 추상클래스</li>
<li>바이트 단위로 데이터를 읽기 위한 여러 메서드를 제공</li>
<li>입력 소스 → 프로그램 방향으로 흐름</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/25d34216-2300-4b22-ac59-1000d1135705/image.png" /></p>
<h2 id="outputstream-클래스">OutputStream 클래스</h2>
<ul>
<li>모든 바이트 출력 스트림의 최상위 추상 클래스</li>
<li>바이트 단위로 데이터를 출력하기 위한 여러 메서드 제공</li>
<li>프로그램 → 출력 대상 방향으로 흐름</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/817029fa-8787-4180-a290-8535f95c7218/image.png" /></p>
<pre><code class="language-java">package test02_ByteStream;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class ByteStreamTest02 {
    public static void main(String[] args) {
        //생성자에 File 객체를 넣던지.. 문자열로 경로를 넣던지..
        //try with resources : 통로가 try문 끝나면 알아서 close() 호출되도록
        try(FileInputStream fis = new FileInputStream(&quot;img.png&quot;)) {
            FileOutputStream fos =  new FileOutputStream(&quot;img-copy2.png&quot;);

            int b; //byte를 담을 손
            //마지막 배열이면 -1을 반환
            while((b = fis.read()) != -1) {
                fos.write(b);
            }
        }catch(FileNotFoundException e) {
            System.out.println(&quot;파일 없음 이슈&quot;);
        } catch (IOException e) {
            System.out.println(&quot;통로 이슈&quot;);
        } 

        //통로를 닫아야한다. -&gt; 우리 쪼꼬미는 지금 어차피 기능이 별로 없어서... 멈춘다.

    }
}
</code></pre>
<p>아래가 훨씬 빠름!</p>
<pre><code class="language-java">package test02_ByteStream;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class ByteStreamTest03 {
    public static void main(String[] args) {
        //생성자에 File 객체를 넣던지.. 문자열로 경로를 넣던지..
        //try with resources : 통로가 try문 끝나면 알아서 close() 호출되도록
        try(FileInputStream fis = new FileInputStream(&quot;img.png&quot;)) {
            FileOutputStream fos =  new FileOutputStream(&quot;img-copy3.png&quot;);

            int b; //byte를 담을 손
            byte[] buffer = new byte[100]; // 바구니 ! 
            //마지막 배열이면 -1을 반환
            while((b = fis.read(buffer)) != -1) {
//                System.out.println(b);
                fos.write(buffer, 0, b);
            }
        }catch(FileNotFoundException e) {
            System.out.println(&quot;파일 없음 이슈&quot;);
        } catch (IOException e) {
            System.out.println(&quot;통로 이슈&quot;);
        } 

        //통로를 닫아야한다. -&gt; 우리 쪼꼬미는 지금 어차피 기능이 별로 없어서... 멈춘다.
    }
}
</code></pre>
<h1 id="문자-스트림character-stream">문자 스트림(Character Stream)</h1>
<ul>
<li>바이트 스트림과 달리 문자 단위 (2byte, 유니코) 단위로 데이터를 처리</li>
<li>주로 키보드에서 입력을 받거나 텍스트 파일을 읽고 쓰는데 사용</li>
<li>문자 스트림의 최상위 추상 클래스 :  Reader, Writer</li>
<li>FileReader와 FileWriter가 파일에 텍스트를 읽거나 쓸 때는 현재 JVM이 돌아가고 있는 시스템의 기본 문자 인코딩 방식을 사용</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/77a440bf-3760-48f1-a1ec-4d19d17ce56e/image.png" /></p>
<h2 id="reader-클래스">Reader 클래스</h2>
<ul>
<li>자바의 모든 문자 입력 스트림의 최상위 추상 클래스</li>
<li>문자 단위로 데이터를 입력 받기 위한 다양한 메서드 정의</li>
</ul>
<h2 id="writer-클래스">Writer 클래스</h2>
<ul>
<li>자바의 모든 문자 출력 스트림의 최상위 추상 클래스</li>
<li>문자 단위로 데이터를 출력하기 위한 다양한 메서드 제공</li>
</ul>
<blockquote>
<p>CTRL + Shift + o : 전체 import</p>
</blockquote>
<pre><code class="language-java">package test03_ByteStream;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class CharacterStream01 {
    public static void main(String[] args) {
        //생성자에 File 객체를 넣던지.. 문자열로 경로를 넣던지..
        //try with resources : 통로가 try문 끝나면 알아서 close() 호출되도록
        try(FileReader reader = new FileReader(&quot;big_input.txt&quot;)) {
            FileWriter writer = new FileWriter(&quot;big_input_copy1.txt&quot;);

            int c; //문자 전달 손
            while((c = reader.read()) != -1) {
                System.out.println(c);
                writer.write(c);
            }
        }catch(FileNotFoundException e) {
            System.out.println(&quot;파일 없음 이슈&quot;);
        } catch (IOException e) {
            System.out.println(&quot;통로 이슈&quot;);
        } 

        //통로를 닫아야한다. -&gt; 우리 쪼꼬미는 지금 어차피 기능이 별로 없어서... 멈춘다.
    }
}
</code></pre>
<pre><code class="language-java">package test03_ByteStream;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class CharacterStream01 {
    public static void main(String[] args) {
        //생성자에 File 객체를 넣던지.. 문자열로 경로를 넣던지..
        //try with resources : 통로가 try문 끝나면 알아서 close() 호출되도록
        try(FileReader reader = new FileReader(&quot;big_input.txt&quot;)) {
            FileWriter writer = new FileWriter(&quot;big_input_copy2.txt&quot;);

            int c; //문자 전달 손
            char[] buffer = new char[100]; //바구니 차례
            while((c = reader.read()) != -1) {
                System.out.println(c);
                writer.write(buffer, 0, c);
            }
        }catch(FileNotFoundException e) {
            System.out.println(&quot;파일 없음 이슈&quot;);
        } catch (IOException e) {
            System.out.println(&quot;통로 이슈&quot;);
        } 

        //통로를 닫아야한다. -&gt; 우리 쪼꼬미는 지금 어차피 기능이 별로 없어서... 멈춘다.
    }
}
</code></pre>
<h1 id="보조-스트림">보조 스트림</h1>
<h2 id="노드-스트림node-stream">노드 스트림(Node Stream)</h2>
<ul>
<li>실제 노드에 연결</li>
<li>데이터의 실제 입출력을 담당</li>
</ul>
<h2 id="보조-스트림filter-stream">보조 스트림(Filter Stream)</h2>
<ul>
<li>실제 노드에 연결되지 않음</li>
<li>다른 스트림을 감싸서 추가적인 기능을 제공</li>
<li>여러 보조 스트림을 체인 형태로 연결하여 다양한 기능을 조합할 수 있음</li>
<li>노드 스트림 없이 <strong>단독으로 사용할 수 없음</strong></li>
<li>보조 스트림의 close()를 호출하면 노드 스트림의 close()까지 호출 됨</li>
<li>InputStream + Reader → <code>InputStreamReader</code> 
OutputStream + Writer → <code>OutputStreamWriter</code></li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/05354f29-e344-4541-87c3-daef75835ca6/image.png" /></p>
<pre><code class="language-java">//                                Reader     &lt;----- 중계역할 &lt;----     InputStream
//                                         InputStreamReader(보조스트림)
//                           char(2bytes)    &lt;-----------------      byte(1bytes)           
/* BufferedReader br = */ new BufferedReader(new InputStreamReader(System.in));

//                                               OutputStreamWriter
/* BufferedWriter bw = */ new BufferedWriter(new OutputStreamWriter(System.out));</code></pre>
<h3 id="버퍼buffer">버퍼(buffer)</h3>
<p>데이터를 한 곳에서 다른 곳으로 전달할 때 </p>
<p>일시적으로 데이터를 저장하는 임시 메모리 영역</p>
<p>→ 여러개의 문자열을 한꺼번에 받아서 처리를 수행할 수 있게 해준다 !</p>
<ul>
<li>버퍼 + 문자열 스트림 : Buffer + Reader  = <code>BufferedReader</code></li>
<li>글자들을 한번에 받아서 개행 단위(\n)로 처리할 수 있다<ul>
<li>메서드 종류<ul>
<li>readLine() : 개행(\n 문자) 단위로 입력값을 문자열로 반환 메서드</li>
</ul>
</li>
</ul>
</li>
</ul>
<blockquote>
<p>객체 Object도 파일화가 가능하다.</p>
</blockquote>
<h1 id="객체-직렬화">객체 직렬화</h1>
<h2 id="직렬화">직렬화</h2>
<ul>
<li><strong>객체 상태를 바이트 스트림으로 변환</strong>하여 <strong>저장하거나 전송</strong>할 수 있도록 하는 과정</li>
<li><strong>역직렬화</strong><ul>
<li>직렬화된 바이트 스트림을 <strong>다시 객체로 복원</strong>하는 절차</li>
</ul>
</li>
<li><code>ObjectOutputStream</code> : ‘객체 → 바이트 스트림’으로 변환하는 스트림</li>
<li><code>ObjectInputStream</code> : ‘바이트 스트림→ 객체’로 복원하는 스트림</li>
</ul>
<h3 id="직렬화-가능-클래스-만들기">직렬화 가능 클래스 만들기</h3>
<p>변환할 클래스를 직렬화 가능 상태로 만들기 </p>
<ul>
<li>객체가 ObjextOutputStream에 의해 직렬화하기 위해서는 해당 객체의 클래스가 반드시 <strong>Serializable 인터페이스</strong>를 구현해야 함</li>
<li>Serializable 인터페이스 : 메서드를 포함하지 않으며, 직렬화 가능 여부를 표시하는 마커 인터페이스</li>
<li>해당 인터페이스를 구현한 클래스를 상속 받았다면 구현하지 않아도 됨</li>
<li>자손에만 구현했다면 조상 클래스는 직렬화 되지 않음</li>
<li>직렬화 과정에서 제외하고 싶은 필드는  transient 키워드를 통해 직렬화 대상에서 제외시킬 수 있음</li>
<li><strong>serialVersionUID</strong> : 클래스의 버전 관리를 위해 serialVersionUID를 명시적으로 선언하는 것을 권장</li>
<li>클래스의 변경으로 인한 역직렬화 오류를 방지하는 데 사용</li>
</ul>
<h2 id="serialversionuid">serialVersionUID</h2>
<ul>
<li><p>직렬화 된 객체를 역직렬화 할 때는 직렬화 했을 때와 같은 클래스를 사용해야 함</p>
</li>
<li><p>따라서 해당 UID를 활용하여 클래스의 변경 여부를 파악</p>
</li>
<li><p>작성하지 않으면 <strong>컴파일러가 자동으로 생성</strong></p>
<p>  (멤버 변경 시 자동 수정 → 위험)</p>
</li>
<li><p>따라서, 작성하는 것을 권장</p>
</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/9773e69f-6d7c-4108-aeac-243cbe8cd12d/image.png" /></p>
<pre><code class="language-java">package test05_객체직렬화;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;

public class ObjectInputOutTest {
    public static void main(String[] args) {
        Person p = new Person(&quot;Kim&quot;, 40);

        // List, Array 도 가능하다 !

        //실제로 파일로 저장을 해보자 !
//        try(ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(&quot;person.txt&quot;));){
//            oos.writeObject(p);
//        }catch(IOException e) {
//            e.printStackTrace();
//        }
        //실제로 파일을 읽어서 객체로 바꾸어보자!
        try(ObjectInputStream ois = new ObjectInputStream(new FileInputStream(&quot;person.txt&quot;));){
            Object obj = ois.readObject();
            System.out.println(obj); //동적바인딩에 의해 Person의 toString이 동작을 한 것이다 !

        }catch(IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
</code></pre>
<pre><code class="language-java">package day08;

import java.io.Serializable;

// 1. 직렬 -&gt; 파일... persons.dat
// 2. Person 안에 있는 필드를 수정...
//   X -&gt; 다른 버전이니까~ 이 때에는 UID 도 변경! 
// 3. persons.dat -&gt; 역직렬화 -&gt; 객체? X (막아줘야한다!)
public class Person implements Serializable {

    private static final long serialVersionUID = -2150443977425410910L;

    private String name;
    public int age;
    public String className;
    public int score;
//    Object inventory;

    public Person() {
        this.name = &quot;홍길동&quot;;
        this.age = 20;
        this.className = &quot;서울 17반&quot;;
        this.score = 60;
//        inventory = new Object();
    }

    // ..getter ..setter
}
</code></pre>
<hr />
<h1 id="실습해보기-">실습해보기 !!</h1>
<p>[Student.java]</p>
<pre><code class="language-java">package com.ssafy.pjt;

import java.io.Serializable;

// 설계도 ! -&gt; 조금만 더 ! 
public class Student implements Serializable, Comparable&lt;Student&gt; {

    private static final long serialVersionUID = 1L;

    private static int autoIncreaseId = 1;
    private int studentId;
    private String name;
    private String major;
    private String track;


    public Student() {
    }

    public Student(String name, String major, String track) {
        super(); // 이거 누구야 ! -&gt; Object 기본 생성자 호출

        this.studentId = autoIncreaseId++; //고유한 학번
        this.name = name;
        this.major = major;
        this.track = track;
    }

    public Student(int studentId, String name, String major, String track) {
        super(); 
        this.studentId = studentId; //고유한 학번
        this.name = name;
        this.major = major;
        this.track = track;
    }

    public int getStudentId() {
        return studentId;
    }

    public void setStudentId(int studentId) {
        this.studentId = studentId;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getMajor() {
        return major;
    }

    public void setMajor(String major) {
        this.major = major;
    }

    public String getTrack() {
        return track;
    }

    public void setTrack(String track) {
        this.track = track;
    }

    @Override
    public String toString() {
        return &quot;Student [studentId=&quot; + studentId + &quot;, name=&quot; + name + &quot;, major=&quot; + major + &quot;, track=&quot; + track + &quot;]&quot;;
    }

    @Override
    public int compareTo(Student o) {
        // - 해두었는데 ...
        // -1, 0 , 1 잘 기억 해두기 !
        return this.studentId - o.studentId;
    }


}
</code></pre>
<p>[StudentManager.java]</p>
<pre><code class="language-java">package com.ssafy.pjt;

import java.util.List;

//인터페이스 -&gt; 완벽히 추상화된 설계도
//인터페이스가 가진 모든 메서드가 추상 메서드다? &lt;- 요즘 버전은 아님 !!!
//인터페이스가 가진 모든 필드는 상수이다? &lt;- 맞다 ! public static final 생략 되어 있음 ~
public interface StudentManager {

    //메서드 (CRUD)
    //등록
    public abstract boolean add(Student student);
    //조회(전체, 상세, 검색)
    public abstract List&lt;Student&gt; getAll();
    public abstract Student getStudent(int studentId);
    //검색 시 이름이 중복되나? 고려하면 배열/리스트든 골라서 쓸 수 있음
    public abstract List&lt;Student&gt; searchName(String name); // 반환타입,,, 동명이인 존재 시 객체 반환할 수도 있고.. 설계하기 나름
    //수정
    public abstract void update(Student student) throws StudentNotFoundException;
    //삭제
    public abstract boolean delete(int studentId);
    //저장
    public abstract void saveData();
    //불러오기
    public abstract void loadData();
}
</code></pre>
<p>[StudentManagerImpl.java]</p>
<pre><code class="language-java">package com.ssafy.pjt;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.List;

//싱글턴패턴 수정해보자 !
public class StudentManagerImpl implements StudentManager {

    //학생들 목록을 관리하자 !
    //1. 배열 ( 고정된 길이 )
    //2. 리스트 (동적 길이 )
    //    2-1. ArrayList : 조회 O(1) - 바로 넣을 수 있다 / 추가 O(n) -  반복문 n번 돌아야 한다
    //    2-2. LinkedList
    private List&lt;Student&gt; studentList = new ArrayList&lt;&gt;(); //추정이 가능하면 &lt;&gt; 제네릭을 비워도 된다.

    //파일로 데이터를 저장하기 위해서 ~ 파일 객체 생성
    private File file = new File(&quot;student.txt&quot;);
    //구분자
    private static final String DELIM = &quot;\t&quot;;


    //나라도 생성하자
    private static StudentManager manager = new StudentManagerImpl();

    // 생성자
    // 외부에서 생성할 수 없게 되었다 !
    private StudentManagerImpl() {    
        loadData();
    }

    //가져다 쓰세요!
    public static StudentManager getManager() {

//         생성 시점을 다르게 할 수도 있다 !        
//        if(manager == null) {
//            manager = new StudentManagerImpl();
//        }

        return manager;
    }


// 메서드 구현
// ======================= 1. 등록 ===============================
    @Override
    public boolean add(Student student) {
        //배열이였다면 
        //size 자리에 넣고 size++; -&gt; 길이를 벗어나지 않았는지 ... 쳌
        //100명 정도만 관리할래 ! 한다면,
        if(studentList.size() &gt; 100) {
            System.out.println(&quot;더 이상 기억할 수 없어요 !&quot;);
            return false;
        }
        //--------- 추후 고려 포인트 ---------------
        //유효성 검사 
        //모든 필드가 완벽하게 작성이 되어 있는 지, 유효한지 등을 췤

        studentList.add(student); // ArrayList는 동적으로 크기가 변경 (grow 메서드) 되기 때문에 길이 확인 하지 않아도 된다.
        return true;
    }

// ======================= 2. 전체 조회 ===============================    
    @Override
    public List&lt;Student&gt; getAll() {
        //배열로 했다면..
        //실제 있는 크기 만큼 -&gt; 새로운 배열을 생성하고 복사해서 반환한다 !

        return studentList;
    }

// ======================= 3. 상세 조회 ===============================    
    @Override
    public Student getStudent(int studentId) {
//        for (int i = 0; i &lt; studentList.size(); i++) {
//            Student tmp = studentList.get(i); // 이렇게 변수 처리 하면 조회 1회
//            if(tmp.getStudentId() == studentId) {
//                return tmp;
//            }
//        }
//        
        for(Student st : studentList) {
            if(st.getStudentId() == studentId) return st;
        }

        return null;
    }

// ======================= 4. 검색 ===============================    
    // 해당 코드는 일치해야만 가져온다.
    // 일부 포함 검색이라면 어떻게 해야할까? .contains()
    @Override
    public List&lt;Student&gt; searchName(String name) {
        //여러명이 있을 수 있으니 List 생성
        List&lt;Student&gt; tmp = new ArrayList&lt;&gt;();
        for(Student st : studentList) {
            if(st.getName().equals(name)) tmp.add(st);
        }

        return tmp;
        //현재로서 반환값이 null일 수는 없음
    }

// ======================= 5. 수정 ===============================    
    @Override
    public void update(Student student) throws StudentNotFoundException{
        //student 인스턴스가 가지고 있는 인간을 가지고 와야 한다. 
        for (int i = 0; i &lt; studentList .size(); i++) {
            if(studentList.get(i).getStudentId() == student.getStudentId()) {
                studentList.set(i, student);
                return;
            }
        }
        throw new StudentNotFoundException(student.getStudentId());
    }

// ======================= 6. 삭제 ===============================
    @Override
    public boolean delete(int studentId) {
//        for (int i = 0; i &lt; studentList.size(); i++) 
            //일치하면 지워 !
            //하나만 지울 때는 상관없지만
            //여러개 지우면 리스트의 크기도 달라지고, 인덱스가 -1씩 되기 때문에 뒤 -&gt; 앞으로 돌아라 !!
            //여러개 지울때는 거꾸로 돌기
        for (int i = studentList.size()-1; i &gt;= 0 ; i--) {
            if(studentList.get(i).getStudentId() == studentId) {
                studentList.remove(i);
                return true;
            }
        }

        return false;
    }

// interface에 없는 메서드 구현해도 오류 나지 않아, 근데 접근을 못해 (? 약간 이해 못함 ㅡㅡ)
// ======================= 7.저장  ===============================
    @Override
    public void saveData() {
        //저장을 한다 ! -&gt; 프로그램에서 파일로 내보내는 것 FileOutputStream
        //죄다 문자야 ! -&gt; Reader 써도 될지도? FileReader OK? O
        try(BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(new FileOutputStream(file)))){
            for(Student st : studentList) {
                bw.write(st.getStudentId() + DELIM + st.getName() + DELIM + st.getTrack() + DELIM + st.getMajor());
                bw.newLine();
            }

            System.out.println(&quot;저장완료&quot;);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    // ======================= 8.불러오기  ===============================
//    @Override
    public void loadData() {
        if (file.exists()) {
            try (BufferedReader br = new BufferedReader(new InputStreamReader(new FileInputStream(file)));) {
                String line;
                while ((line = br.readLine()) != null) {
                    String[] tmp = line.split(DELIM);
                    if (tmp.length != 4)
                        continue; // 이상한 데이터니까 버리자
                    int studentId = Integer.parseInt(tmp[0]);
                    String name = tmp[1];
                    String track = tmp[2];
                    String major = tmp[3];
                    studentList.add(new Student(studentId, name, major, track));
                }
            } catch (FileNotFoundException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
}
</code></pre>
<p>[Test.java]</p>
<pre><code class="language-java">package com.ssafy.pjt;

public class Test {
    public static void main(String[] args) {
        StudentManager mgr = StudentManagerImpl.getManager();
//        StudentManagerImpl mgr = StudentManagerImpl.getManager(); &lt;- 왜 에러 나는 지 고민해봐 !

        Student st1 = new Student(&quot;양띵균&quot;, &quot;컴퓨터공학&quot;, &quot;Java 비전공&quot;);
        Student st2 = new Student(&quot;양땅균&quot;, &quot;컴퓨터공학&quot;, &quot;Java 비전공&quot;);
        Student st3 = new Student(&quot;양땡균&quot;, &quot;컴퓨터공학&quot;, &quot;Java 비전공&quot;);
        Student st4 = new Student ( &quot;양떵균&quot;, &quot;컴퓨터공학&quot;, &quot;Java 비전공&quot;);
        Student st5 = new Student(&quot;양똥균&quot;, &quot;컴퓨터공학&quot;, &quot;Java 비전공&quot;);

        mgr.add(st1);
        mgr.add(st2);
        mgr.add(st3);
        mgr.add(st4);
        mgr.add(st5);
        System.out.println(&quot;-----전체 목록-----&quot;);
        System.out.println(mgr.getAll());
        for(Student st : mgr.getAll()) System.out.println(st);

        System.out.println(mgr.getStudent(100));

        System.out.println(&quot;-----이름 검색-----&quot;);
        for(Student st : mgr.searchName(&quot;양똥균&quot;)) System.out.println(st);

        System.out.println(&quot;-----수정-----&quot;);
        Student st6 = new Student(&quot;양명균&quot;, &quot;경제학&quot;, &quot;Java비전공&quot;);
        st6.setStudentId(5);

        try {
            mgr.update(st6);
        } catch (StudentNotFoundException e) {
            System.out.println(e.getMessage());
        }

        for(Student st : mgr.getAll()) System.out.println(st);

        System.out.println(&quot;-----지우기-----&quot;);
        mgr.delete(5);

        for(Student st : mgr.getAll()) System.out.println(st);

        System.out.println(&quot;-----데이터 저장-----&quot;);
        mgr.saveData();

    }
}
</code></pre>
<p>[StudentNotFoundException.java]</p>
<pre><code class="language-java">package com.ssafy.pjt;

public class StudentNotFoundException extends Exception {

    public StudentNotFoundException(int studentID) {
        super(studentID + &quot;라는 학번은 없습니다. &quot;);
    }    
}</code></pre>
<p>[Test2.java]</p>
<pre><code class="language-java">package com.ssafy.pjt;

public class Test2 {
    public static void main(String[] args) {
        StudentManager mgr = StudentManagerImpl.getManager();

        for(Student st : mgr.getAll()) System.out.println(st);
    }
}</code></pre>