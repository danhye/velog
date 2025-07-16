<h1 id="오늘의-학습-정리-html-css-ai-프롬프팅">오늘의 학습 정리: HTML, CSS, AI 프롬프팅</h1>
<h2 id="📌-렌더링-방식">📌 렌더링 방식</h2>
<h3 id="1-서버-사이드-렌더링-ssr">1. 서버 사이드 렌더링 (SSR)</h3>
<ul>
<li>각 사용자 요청마다 서버가 HTML을 렌더링해 응답</li>
<li>사용자 맞춤형 동적 처리 가능하지만 <strong>서버 부하 발생 가능</strong></li>
</ul>
<pre><code>브라우저(클라이언트) → Spring(서버)
           정적 리소스          ↕          동적 데이터
       (HTML, CSS, JS 등)              (백엔드 영역)</code></pre><h3 id="2-클라이언트-사이드-렌더링-csr">2. 클라이언트 사이드 렌더링 (CSR)</h3>
<ul>
<li>사용자 브라우저에서 렌더링 수행</li>
<li>초기 로딩이 느릴 수 있으나 서버 부하가 적음</li>
</ul>
<h2 id="💻-visual-studio-code-사용법">💻 Visual Studio Code 사용법</h2>
<ul>
<li>Microsoft 제공 무료 텍스트 에디터</li>
<li>기본 Emmet 기능 내장: <code>!</code> + Enter → HTML 뼈대 자동완성</li>
<li><code>example.html</code> 파일 더블클릭 → 브라우저 오픈</li>
<li><code>Live Server</code> 확장 설치 → 파일 저장 시 자동 새로고침</li>
<li>오른쪽 클릭 → &quot;Open with Live Server&quot;</li>
</ul>
<h2 id="🌐-html-hypertext-markup-language">🌐 HTML (HyperText Markup Language)</h2>
<ul>
<li>웹 페이지의 구조를 표현하는 마크업 언어</li>
<li>프로그래밍 언어 ❌ → 로직이나 연산 없음</li>
</ul>
<h3 id="✅-html-기본-구조">✅ HTML 기본 구조</h3>
<pre><code class="language-html">&lt;h1&gt;Title&lt;/h1&gt;
&lt;hr /&gt;</code></pre>
<ul>
<li>여는 태그 + 닫는 태그 + 내용</li>
<li>빈 태그: <code>&lt;br&gt;</code>, <code>&lt;img&gt;</code>, <code>&lt;input&gt;</code> 등은 닫는 태그 없음</li>
<li><strong>중첩 가능</strong>, 오류 발생 안 하고 레이아웃만 깨질 수 있음</li>
</ul>
<h3 id="✅-속성-attribute">✅ 속성 (Attribute)</h3>
<pre><code class="language-html">&lt;a href=&quot;https://google.com&quot;&gt;링크&lt;/a&gt;
&lt;img src=&quot;image.jpg&quot; alt=&quot;설명&quot; /&gt;</code></pre>
<ul>
<li>태그에 추가 정보 제공</li>
<li><code>속성명=&quot;속성값&quot;</code> 형식</li>
<li>속성 간 공백 X, 쌍따옴표 필수</li>
</ul>
<h3 id="✅-인라인-vs-블록-요소">✅ 인라인 vs 블록 요소</h3>
<table>
<thead>
<tr>
<th>인라인 요소</th>
<th>블록 요소</th>
</tr>
</thead>
<tbody><tr>
<td><code>&lt;a&gt;</code>, <code>&lt;b&gt;</code>, <code>&lt;i&gt;</code>, <code>&lt;span&gt;</code>, <code>&lt;img&gt;</code></td>
<td><code>&lt;p&gt;</code>, <code>&lt;h1&gt;~&lt;h6&gt;</code>, <code>&lt;div&gt;</code>, <code>&lt;ul&gt;</code>, <code>&lt;ol&gt;</code>, <code>&lt;li&gt;</code>, <code>&lt;form&gt;</code></td>
</tr>
</tbody></table>
<ul>
<li>블록 요소: 한 줄 전체 차지</li>
<li>인라인 요소: 글자처럼 이어짐</li>
</ul>
<h3 id="✅-주석">✅ 주석</h3>
<pre><code class="language-html">&lt;!-- 이것은 주석입니다 --&gt;</code></pre>
<ul>
<li>코드 설명 및 협업 시 필수</li>
</ul>
<h3 id="✅-이미지-링크-예시">✅ 이미지 링크 예시</h3>
<pre><code class="language-html">&lt;a href=&quot;https://www.ssafy.com&quot;&gt;
  &lt;img width=&quot;200&quot; height=&quot;100&quot; src=&quot;images.png&quot; alt=&quot;ssafy&quot; /&gt;
&lt;/a&gt;</code></pre>
<h2 id="🎨-css-cascading-style-sheets">🎨 CSS (Cascading Style Sheets)</h2>
<ul>
<li>HTML 문서의 스타일을 지정하는 언어</li>
<li>기본 문법:</li>
</ul>
<pre><code class="language-css">h1 {
  color: blue;
  font-size: 15px;
}</code></pre>
<ul>
<li><strong>선택자 { 속성: 값; }</strong> 구조</li>
<li>주석: <code>/* 이것은 주석입니다 */</code></li>
</ul>
<h3 id="✅-스타일-적용-방법">✅ 스타일 적용 방법</h3>
<ol>
<li><strong>인라인 스타일</strong>: <code>&lt;tag style=&quot;color:blue&quot;&gt;</code></li>
<li><strong>내부 스타일</strong>: <code>&lt;style&gt;</code> 태그 안에 작성</li>
<li><strong>외부 스타일시트</strong>: <code>.css</code> 파일을 <code>&lt;link&gt;</code>로 불러오기</li>
</ol>
<h3 id="✅-선택자-종류">✅ 선택자 종류</h3>
<table>
<thead>
<tr>
<th>유형</th>
<th>예시</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>전체 선택자</td>
<td><code>*</code></td>
<td>모든 요소 선택</td>
</tr>
<tr>
<td>태그 선택자</td>
<td><code>div</code></td>
<td>div 요소 선택</td>
</tr>
<tr>
<td>ID 선택자</td>
<td><code>#notice</code></td>
<td>id=&quot;notice&quot; 선택</td>
</tr>
<tr>
<td>클래스 선택자</td>
<td><code>.head</code></td>
<td>class=&quot;head&quot; 선택</td>
</tr>
<tr>
<td>자손 선택자</td>
<td><code>ul span</code></td>
<td>ul 안의 모든 span 선택</td>
</tr>
<tr>
<td>자식 선택자</td>
<td><code>ul &gt; li</code></td>
<td>ul의 직계 li 선택</td>
</tr>
</tbody></table>
<ul>
<li>여러 클래스: <code>&lt;h1 class=&quot;sub1 sub2&quot;&gt;</code></li>
<li>예시:</li>
</ul>
<pre><code class="language-css">.menu li &gt; ul &gt; li {
  color: red;
}</code></pre>
<h2 id="🤖-ai-프롬프팅-기초">🤖 AI 프롬프팅 기초</h2>
<ul>
<li>AI에게 명확한 요구 전달 = 좋은 결과 생성의 핵심</li>
</ul>
<h3 id="프롬프트-기법">프롬프트 기법</h3>
<table>
<thead>
<tr>
<th>기법</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>페르소나 프롬프팅</td>
<td>전문가 역할 부여 (예: &quot;당신은 법률 전문가입니다&quot;)</td>
</tr>
<tr>
<td>제로샷 프롬프팅</td>
<td>예시 없이 질문 → 간단한 요청에 적합</td>
</tr>
<tr>
<td>생각의 사슬</td>
<td>단계별로 사고과정 요청 → 복잡한 문제 해결에 유용</td>
</tr>
<tr>
<td>퓨샷 프롬프팅</td>
<td>예시 몇 개 주고 비슷한 출력 유도</td>
</tr>
</tbody></table>
<h3 id="할루시네이션-주의">할루시네이션 주의</h3>
<ul>
<li><strong>가장 그럴듯한 답</strong>을 만드는 경향 있음</li>
<li>존재하지 않는 문헌/사실을 말할 수 있음</li>
<li><strong>항상 검증 필요</strong></li>
<li><strong>같은 질문 → 다른 응답 가능성 있음</strong></li>
<li>최신 정보나 정확한 사실은 제한될 수 있음 (언어, 학습 시점 한계)</li>
</ul>