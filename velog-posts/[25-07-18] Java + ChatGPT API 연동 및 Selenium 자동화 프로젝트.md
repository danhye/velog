<h2 id="🧠-java--chatgpt-api-연동-및-selenium-자동화-프로젝트">🧠 Java + ChatGPT API 연동 및 Selenium 자동화 프로젝트</h2>
<h3 id="☕-개발-환경">☕ 개발 환경</h3>
<ul>
<li>IDE: Spring Tool Suite 4 (4.22.1.RELEASE)</li>
<li>JDK: Java 17</li>
<li>빌드 툴: Maven</li>
<li>UI: Swing</li>
<li>API 연동: OpenAI GPT API</li>
<li>자동화: Selenium</li>
</ul>
<h3 id="📌-java의-특징">📌 Java의 특징</h3>
<p>Java는 모든 단위가 클래스로 구성되어야 한다.</p>
<p>main() 메서드는 프로그램의 시작 지점이며, JVM이 가장 먼저 찾는 진입점이다.</p>
<p>주요 I/O</p>
<p>System.out: 표준 출력 (콘솔 출력)
System.in: 표준 입력</p>
<h4 id="✅-기본-문법-예시">✅ 기본 문법 예시</h4>
<ul>
<li>int : Integer 의 약자, 정수</li>
<li>String : 문자열</li>
<li>= : operator 대입 연산자</li>
</ul>
<p>한번에 하기 : 선언 + 초기화 ( 선언과 동시에 값 할당, 훨씬 더 간편!)</p>
<pre><code class="language-java">int age = 25;

String name = “김자바”;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/e9a87110-6dc4-4c8f-8e87-c248bfe875a0/image.png" />
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/4a71efa5-65c9-4092-9d5e-86bd184c0c5c/image.png" />
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/312f4651-d710-4de2-b601-3dddbdbeeeb3/image.png" />
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/2366659d-9433-4d85-b7fd-b79f27cce13d/image.png" /></p>
<h3 id="🌐-openai-gpt-api-연동-콘솔-출력용">🌐 OpenAI GPT API 연동 (콘솔 출력용)</h3>
<h4 id="🌐-api-application-programming-interface란">🌐 API (Application Programming Interface)란?</h4>
<p>서로 다른 소프트웨어 시스템이 데이터나 기능을 주고받기 위해 사용하는 통신 규칙 또는 인터페이스입니다.</p>
<h4 id="🔁-api의-동작-구조">🔁 API의 동작 구조</h4>
<pre><code>클라이언트   ──요청(Request)──▶  서버(API 서버)
클라이언트   ◀─응답(Response)──</code></pre><p>클라이언트 : 데이터를 요청하는 주체 (예: 브라우저, 앱, 프로그램)</p>
<p>서버: 요청을 처리하고 결과(데이터)를 응답하는 주체</p>
<p>요청(Request): 어떤 데이터를 보내거나, 특정 작업을 요청하는 행위</p>
<h4 id="📦-maven-의존성-추가">📦 Maven 의존성 추가</h4>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/51350841-721c-44b8-8520-90e574a6e0a0/image.png" /></p>
<pre><code class="language-java">&lt;!-- org.json --&gt;
&lt;dependency&gt;
  &lt;groupId&gt;org.json&lt;/groupId&gt;
  &lt;artifactId&gt;json&lt;/artifactId&gt;
  &lt;version&gt;20250517&lt;/version&gt;
&lt;/dependency&gt;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/e2fed210-fc8b-43f3-9689-5501fdbb3a09/image.png" />
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/dbfeeaf4-d511-45f7-9969-bb64e6059f57/image.png" />
위와 같이 추가 됨</p>
<h5 id="💬-chatgpt-호출-예제">💬 ChatGPT 호출 예제</h5>
<pre><code class="language-java">import java.io.*;
import java.net.HttpURLConnection;
import java.net.URL;
import org.json.*;

public class ChatGPT {
    private static final String API_KEY = &quot;개인키&quot;;
    private static final String GPT_URL = &quot;https://api.openai.com/v1/chat/completions&quot;;

    public static void main(String[] args) throws IOException {
        URL url = new URL(GPT_URL);
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();

        conn.setRequestMethod(&quot;POST&quot;);
        conn.setRequestProperty(&quot;Authorization&quot;, &quot;Bearer &quot; + API_KEY);
        conn.setRequestProperty(&quot;Content-Type&quot;, &quot;application/json&quot;);
        conn.setDoOutput(true);

        // 요청 메시지 생성
        JSONObject message = new JSONObject();
        message.put(&quot;role&quot;, &quot;user&quot;);
        message.put(&quot;content&quot;, &quot;hello! how are you!&quot;);

        JSONArray messages = new JSONArray();
        messages.put(message);

        JSONObject request = new JSONObject();
        request.put(&quot;model&quot;, &quot;gpt-4o-mini&quot;);
        request.put(&quot;temperature&quot;, 0.7);
        request.put(&quot;messages&quot;, messages);

        // 전송
        try (BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(conn.getOutputStream()))) {
            bw.write(request.toString());
        }

        // 응답 수신
        StringBuilder sb = new StringBuilder();
        try (BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()))) {
            String line;
            while ((line = br.readLine()) != null) {
                sb.append(line);
            }
        }

        // JSON 파싱
        JSONObject root = new JSONObject(sb.toString());
        String content = root.getJSONArray(&quot;choices&quot;)
                            .getJSONObject(0)
                            .getJSONObject(&quot;message&quot;)
                            .getString(&quot;content&quot;);

        System.out.println(content);
    }
}</code></pre>
<br />
console 창의 응답 값 편한 형태로 보기  

<p>chatGPT 답변 확인할 수 있음</p>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/9f9e78ee-8d8f-43a4-b6f8-8717c00a1eb7/image.png" /></p>
<p>클라이언트와 서버는 API 통신을 통해 JSON 문자열을 주고 받음</p>
<p>&lt; JSON 라이브러리 &gt; 
<strong>import</strong> org.json.JSONArray;
<strong>import</strong> org.json.JSONObject;</p>
<p>통해서 원하는 데이터를 가져올 수 있음</p>
<p>배열로 감쌈 (JSONArray)</p>
<pre><code class="language-java"> JSONArray messages = new JSONArray();
 messages.put(message);</code></pre>
<h4 id="🖥️-swing-기반-데스크탑-챗봇-ui">🖥️ Swing 기반 데스크탑 챗봇 UI</h4>
<h4 id="🧪-실습해보기">🧪 실습해보기!</h4>
<p>MyGPT.java 클래스 생성</p>
<pre><code class="language-java">import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.io.*;
import java.net.HttpURLConnection;
import java.net.URL;
import org.json.JSONArray;
import org.json.JSONObject;

public class MyGPT extends JFrame {
    private JTextArea chatArea;
    private JTextField inputField;
    private JButton sendButton;
    private JSONArray messages = new JSONArray();

    // OpenAI API 정보
    private static final String API_KEY = &quot;개인APIKey&quot;;
    private static final String GPT_URL = &quot;https://api.openai.com/v1/chat/completions&quot;;

    // 화면 구성하기 ( 생성자 함수 ) : 초기 화면을 만드는 부분
    public MyGPT() {
        setTitle(&quot;💬 MyGPT 와 대화하기&quot;);
        setSize(500, 600); // 창 크기
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); // 창 닫으면 프로그램 종료
        setLayout(new BorderLayout()); // 창을 위아래로 나누기 (구역 분할)

        // 채팅창 영역 ( 사용자가 쓴 말과 챗봇이 응답한 내용을 chatArea에 출력
        chatArea = new JTextArea(); // 채팅 내역 보여 줄 텍스트 박스
        chatArea.setEditable(false); // 사용자가 수정 하지 못하도록 설정
        chatArea.setLineWrap(true); // 줄바꿈 허용
        JScrollPane scrollPane = new JScrollPane(chatArea); // 스크롤 붙이기
        add(scrollPane, BorderLayout.CENTER); // 창 중앙에 배치

        // 입력창과 버튼
        JPanel inputPanel = new JPanel(new BorderLayout());
        inputField = new JTextField(); // 입력창
        sendButton = new JButton(&quot;전송&quot;);

        // 입력창과 버튼을 BorderLayout.SOUTH (아래쪽) 에 넣기
        inputPanel.add(inputField, BorderLayout.CENTER);
        inputPanel.add(sendButton, BorderLayout.EAST);
        add(inputPanel, BorderLayout.SOUTH);

        // 이벤트 등록 (버튼을 누르거나 엔터를 치면 sendMessage() 함수 실행)
        sendButton.addActionListener(e -&gt; sendMessage());
        inputField.addActionListener(e -&gt; sendMessage());

        setVisible(true); 
    }

    private void sendMessage() { // 사용자가 입력하면 챗봇에게 요청을 보내는 함수

        /*
         * 역할 요약 :
         * 1. 입력창에서 사용자 말 꺼냄
         * 2. chatArea에 사용자 메시지 출력
         * 3. API에 요청 -&gt; 응답 받아서 출력
         */

        String userText = inputField.getText().trim(); // 사용자 입력, 불필요한 공백을 제거하기 위해 trim 수행
        if (userText.isEmpty()) return; // 입력 없으면 종료

        chatArea.append(&quot;👤 나: &quot; + userText + &quot;\n&quot;); // 내가 한 말 출력
        inputField.setText(&quot;&quot;); // 입력창 비우기

        // messages 배열에 사용자 메시지 추가
        JSONObject userMsg = new JSONObject();
        userMsg.put(&quot;role&quot;, &quot;user&quot;);
        userMsg.put(&quot;content&quot;, userText);
        messages.put(userMsg);

        // GPT API 호출 - 새로운 스레드에서 실행
        new Thread(() -&gt; {
            try {
                String response = sendToGPT(messages); // 응답 받아오기

                // 응답도 저장
                JSONObject assistantMsg = new JSONObject();
                assistantMsg.put(&quot;role&quot;, &quot;assistant&quot;);
                assistantMsg.put(&quot;content&quot;, response);
                messages.put(assistantMsg);

                SwingUtilities.invokeLater(() -&gt; { // 지금 말고 화면 업데이트가 가능할 때 실행
                    chatArea.append(&quot;🤖 챗봇: &quot; + response + &quot;\n\n&quot;); // 응답 출력
                });
            } catch (IOException e) {
                SwingUtilities.invokeLater(() -&gt; {
                    chatArea.append(&quot;❌ 오류: &quot; + e.getMessage() + &quot;\n&quot;);
                });
            }
        }).start();
    }

    private String sendToGPT(JSONArray messages) throws IOException {
        // 실제로 GPT에게 메시지 보내고 응답 받는 함수

        // URL, 연결 설정
        URL url = new URL(GPT_URL);
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();

        conn.setRequestMethod(&quot;POST&quot;);
        conn.setRequestProperty(&quot;Authorization&quot;, &quot;Bearer &quot; + API_KEY);
        conn.setRequestProperty(&quot;Content-Type&quot;, &quot;application/json&quot;);
        conn.setDoOutput(true);

        // 요청 JSON 만들기
        JSONObject request = new JSONObject();
        request.put(&quot;model&quot;, &quot;gpt-4o-mini&quot;);
        request.put(&quot;temperature&quot;, 0.7);
        request.put(&quot;messages&quot;, messages);

        // 서버에 요청 보내기
        try (BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(conn.getOutputStream()))) {
            bw.write(request.toString());
        }

     // 서버에서 응답 받기
        StringBuilder sb = new StringBuilder();
        try (BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()))) {
            String line;
            while ((line = br.readLine()) != null) {
                sb.append(line);
            }
        }

        // 응답 JSON 파싱해서 content(내용)만 꺼냄
        JSONObject root = new JSONObject(sb.toString());
        return root.getJSONArray(&quot;choices&quot;)
                   .getJSONObject(0)
                   .getJSONObject(&quot;message&quot;)
                   .getString(&quot;content&quot;)
                   .trim();
    }

    // GUI 시작
    public static void main(String[] args) {
        SwingUtilities.invokeLater(MyGPT::new);
    }
}
</code></pre>
<p>API 는 제한이 있어서 사이트 정보를 가져오려면 해당 사이트의 HTML 정보를 직접 가져와야 한다 ~
그래서 Selenium을 사용할 수 있는데 ~~</p>
<h4 id="🤖-api-요약">🤖 API 요약</h4>
<p>API_KEY    : 개인 발급 OpenAI 키
messages : 역할(role), 내용(content)을 담는 JSON 배열
응답 파싱 : root → choices[0] → message → content</p>
<h3 id="🕹-selenium-웹-자동화">🕹 Selenium 웹 자동화</h3>
<p>*<em>💡 특징 *</em></p>
<p>브라우저를 직접 조작하여 자동화 수행
HTML DOM 요소에 직접 접근 가능
반복적인 웹 작업 처리에 탁월</p>
<h4 id="☑-maven-설정-selenium">☑ Maven 설정 (Selenium)</h4>
<pre><code>&lt;dependency&gt;
  &lt;groupId&gt;org.seleniumhq.selenium&lt;/groupId&gt;
  &lt;artifactId&gt;selenium-java&lt;/artifactId&gt;
  &lt;version&gt;4.34.0&lt;/version&gt;
&lt;/dependency&gt;
xml
복사
편집
&lt;plugin&gt;
  &lt;artifactId&gt;maven-compiler-plugin&lt;/artifactId&gt;
  &lt;version&gt;3.8.1&lt;/version&gt;
  &lt;configuration&gt;
    &lt;release&gt;17&lt;/release&gt;
  &lt;/configuration&gt;
&lt;/plugin&gt;</code></pre><h5 id="📁-java-클래스-생성-위치">📁 Java 클래스 생성 위치</h5>
<p>src/
└── main/
    └── java/
        └── SeleniumTest.java</p>
<p>Selenium 사용 시 페이지 로딩 지연에 주의 
→ Thread.sleep() 또는 WebDriverWait 사용 권장</p>
<h4 id="🧪-실습해보기-1">🧪 실습해보기!</h4>
<p>Selenium으로 자동 로그인 매크로 만들기 💻</p>
<p>처음 시도한 타겟은 네이버 로그인 페이지였으나,<br />reCAPTCHA 로 인해 막혔다...</p>
<pre><code class="language-java">import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class LoginTest {
    public static void main(String[] args) throws InterruptedException {
        WebDriver driver = new ChromeDriver();

    // 1. 네이버 접속
    driver.get(&quot;https://www.naver.com&quot;);

    // 2. 로그인 버튼 클릭
    WebElement loginBtn = driver.findElement(By.cssSelector(&quot;#account &gt; div &gt; a&quot;));
    loginBtn.click();

    // 페이지 전환까지 대기 (JS 로딩 고려)
    Thread.sleep(1000);

    // 3. ID / PW 입력
    WebElement idInput = driver.findElement(By.id(&quot;id&quot;));
    WebElement pwInput = driver.findElement(By.id(&quot;pw&quot;));

    idInput.sendKeys(&quot;내ID&quot;);
    pwInput.sendKeys(&quot;내비밀번호&quot;);

    // 4. 로그인 버튼 클릭
    WebElement loginSubmit = driver.findElement(By.cssSelector(&quot;.btn_login&quot;));
    loginSubmit.click();
}
}</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/36324ec6-4569-43fa-b754-bf9d67c81758/image.png" /></p>
<p>후 ... ㅂㄷㅂㄷ</p>
<p>그래서 싸피 사이트의 자동 로그인을 구현해보았다.</p>
<pre><code class="language-java">import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class LoginTest {
    public static void main(String[] args) throws InterruptedException {
        WebDriver driver = new ChromeDriver();

        // 1. edu.ssafy 사이트에 접속
        driver.get(&quot;https://edu.ssafy.com&quot;);
        Thread.sleep(1000);

        //#userId
        //#userPwd
        // 2. ID / PW 입력
        WebElement idInput = driver.findElement(By.id(&quot;userId&quot;));
        WebElement pwInput = driver.findElement(By.id(&quot;userPwd&quot;));

        idInput.sendKeys(&quot;내ID&quot;);
        pwInput.sendKeys(&quot;내 비밀번호&quot;);

        // 3. 로그인 버튼 클릭
        WebElement loginSubmit = driver.findElement(By.cssSelector(&quot;#wrap &gt; div &gt; div &gt; div.section &gt; form &gt; div &gt; div.field-set.log-in &gt; div.form-btn &gt; a&quot;));
        loginSubmit.click();
    }
}
</code></pre>
<br />

<p>재밌다 ! 나중에 다른 매크로도 만들어 봐야지 <img alt="" src="https://velog.velcdn.com/images/danhye821/post/3bd72a5d-1ef6-46db-b344-cbf926058f31/image.gif" /></p>