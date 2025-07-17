<h3 id="📝-markdown">📝 Markdown</h3>
<p>마크다운이란?
텍스트 기반의 마크업 언어로, 문서의 구조를 빠르게 작성할 수 있습니다.
간단한 문법으로도 HTML과 비슷한 효과를 낼 수 있어 개발 업계의 표준 문서 포맷으로 널리 사용됩니다.</p>
<h3 id="✅-markdown을-왜-배워야-할까">✅ Markdown을 왜 배워야 할까?</h3>
<ul>
<li>개발 업계 표준 문서 작성 포맷이기 때문.</li>
<li>HTML, XML 보다 문서의 구조와 내용을 빠르고 쉽게 적을 수 있음</li>
<li>에디터가 없어도, 텍스트 에디터만 있다면 쉽게 수정 가능</li>
</ul>
<h4 id="특징">특징</h4>
<p>작성된 Markdown 문서는 다른 프로그램에 의해 변환되어 출력 됨
문법 참조 링크 : <a href="https://www.markdownguide.org/basic-syntax/">https://www.markdownguide.org/basic-syntax/</a></p>
<h2 id="📌-문법-예시">📌 문법 예시</h2>
<h3 id="heading제목">Heading(제목)</h3>
<p>문서의 단계별 제목으로 사용
#의 개수에 따라 제목의 수준을 구별</p>
<pre><code class="language-markdown"># 제목1
## 부제목
### 부부제목</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/3cc5bde2-96b8-4aca-b44e-a0a96c8a6233/image.png" /></p>
<h3 id="리스트">리스트</h3>
<p>목록을 표시하기 위해 사용
순서가 있는 리스트와 순서가 없는 리스트 제공</p>
<pre><code class="language-markdown"># 순서가 있는 리스트
1. 바나나
    1. 바나나는 맛있습니다. (띄어쓰기 4칸)
    2. 맛있으면 바나나
2. 딸기
3. 사과

# 순서가 없는 리스트
- 바나나
- 딸기
- 사과
    - 사과 </code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/c8feec5d-a6f1-46f2-8666-d79a863316cf/image.png" /></p>
<h3 id="code-block--inline-code-block">code block &amp; inline code block</h3>
<p>일반 텍스트와 달리 해당 프로그래밍 언어에 맞춰서 텍스트 스타일을 변화
개발에서 마크다운을 사용하는 가장 큰 이유 </p>
<p>기호 : ```(한칸 띄우기) </p>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/9818c3f8-c245-48ea-8c29-d0225d4dbe07/image.png" /></p>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/e1389349-2c03-4e26-b2cd-858d3a8ce582/image.png" /></p>
<p>참고) ``` 는 중첩 가능</p>
<h3 id="링크link--이미지image">링크(link) &amp; 이미지(image)</h3>
<p>특정 주소를 사용해 다른 페이지로 이동하는 링크 혹은 이미지 출력
이미지의 너비와 높이는 마크다운으로 조절할 수 없음(HTML 필요, 흑마법)</p>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/ee243ad5-9367-4ddc-885f-17f41f66e019/image.png" /></p>
<p><strong>강제 개행</strong>
줄바꿈 후 첫 줄 띄어쓰기 두번</p>
<pre><code class="language-markdown">### 링크와 이미지
- 링크  
[네이버](https://www.naver.com)  
원하는   
글자를  
개행하면서  
작성해볼 수 있어요

- 이미지  
![이미지](ssafy.png)  
![이미지](assets/ssafy.png)
&lt;img src=&quot;ssafy.png&quot;  
width=&quot;300&quot; height=&quot;200&quot; /&gt;</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/62c80dd4-3389-4b6a-90fb-a78e5f8ef359/image.png" /></p>
<h3 id="텍스트-관련-문법">텍스트 관련 문법</h3>
<pre><code class="language-markdown"># 텍스트 작성
**굵게**  
*기울임*  
~~취소선~~

- [ ] 새로운 항목1
- [ ] 원래는 체크박스가 생성됨
- [x] 체크된 상태는 x</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/2fdc27a2-7094-4fe5-9c15-b479a0e3c247/image.png" /></p>
<h3 id="수평선">수평선</h3>
<p>단락을 구분할 때 사용하는 수평선
-(hypen)을 3개 이상 적으면 작동</p>
<pre><code class="language-markdown"># 구분선

문장1

---

문장2</code></pre>
<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/5fcff735-61da-4db9-bea9-fce9672a0e10/image.png" /></p>
<p>마크다운 에디터</p>
<ul>
<li>typora(유료)</li>
<li>MarkText(무료)</li>
</ul>
<br />
<br />
<br />

<h2 id="💻-cli-command-line-interface">💻 CLI (Command Line Interface)</h2>
<p><strong>CLI란?</strong>
텍스트 기반의 명령어를 통해 컴퓨터와 상호작용하는 방식.
GUI보다 빠르고 자원 소모가 적어 개발/서버 환경에서 필수입니다.</p>
<h3 id="📂-cli-기본-명령어">📂 CLI 기본 명령어</h3>
<table>
<thead>
<tr>
<th><strong>명령어</strong></th>
<th><strong>설명</strong></th>
</tr>
</thead>
<tbody><tr>
<td><code>pwd</code></td>
<td>현재 경로 출력</td>
</tr>
<tr>
<td><code>ls</code></td>
<td>폴더/파일 리스트 확인</td>
</tr>
<tr>
<td><code>cd</code></td>
<td>경로 이동</td>
</tr>
<tr>
<td><code>mkdir</code></td>
<td>폴더 생성</td>
</tr>
<tr>
<td><code>touch</code></td>
<td>파일 생성</td>
</tr>
<tr>
<td><code>rm</code></td>
<td>파일/폴더 삭제 (<code>-r</code>, <code>-f</code> 옵션 조합 가능)</td>
</tr>
<tr>
<td><code>echo</code></td>
<td>문자열 출력 (파일에 쓰기 가능)</td>
</tr>
<tr>
<td><code>vi</code></td>
<td>기본 텍스트 에디터 (<code>ESC → :wq</code> 저장/종료)</td>
</tr>
</tbody></table>
<br />

<h3 id="💡-경로-표현">💡 경로 표현</h3>
<p>. : 현재 디렉토리</p>
<p>.. : 상위 디렉토리</p>
<p>~ : 사용자 홈 디렉토리</p>
<h4 id="리다이렉션--파이프">리다이렉션 &amp; 파이프</h4>
<p>echo &quot;Hello&quot; &gt; hello.txt    # 새로쓰기
echo &quot;World&quot; &gt;&gt; hello.txt   # 이어쓰기</p>
<br />

<p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/9670c18f-b300-4679-b76e-25c0d7faa596/image.png" />
&lt;정답&gt;</p>
<ol>
<li>pwd</li>
<li>mkdir sample</li>
<li>cd sample</li>
<li>pwd</li>
<li>touch red orange white</li>
<li>vi red -&gt; i -&gt; 빨강 -&gt; esc -&gt; wq!</li>
<li>echo &quot;주황&quot; &gt; orange</li>
<li>pwd</li>
<li>rm white</li>
<li>pwd</li>
</ol>
<br />
<br />

<h2 id="🌱-git--github-정리">🌱 GIT &amp; GITHUB 정리</h2>
<h3 id="🔧-git이란">🔧 Git이란?</h3>
<blockquote>
<p><strong>분산 버전 관리 시스템(DVCS)</strong><br />변화를 기록하고 추적하여, 특정 시점으로 되돌아가거나 여러 개발자가 동시에 작업할 수 있게 해주는 시스템</p>
</blockquote>
<h4 id="✅-버전-관리란">✅ 버전 관리란?</h4>
<ul>
<li><strong>버전</strong>: 컴퓨터의 특정 상태  </li>
<li><strong>관리</strong>: 해당 상태를 유지하고 기록하는 것  </li>
<li><strong>버전 관리 프로그램</strong>: 코드의 히스토리(버전)을 관리하는 도구<br />→ 개발 이력을 추적하고, 협업 과정 파악 가능</li>
</ul>
<hr />
<h3 id="🔁-버전-관리-방식-비교">🔁 버전 관리 방식 비교</h3>
<table>
<thead>
<tr>
<th>구분</th>
<th>중앙 집중식 (CVCS)</th>
<th>분산 버전 관리 (DVCS)</th>
</tr>
</thead>
<tbody><tr>
<td>예시</td>
<td>SVN</td>
<td>Git, Mercurial</td>
</tr>
<tr>
<td>저장 방식</td>
<td>서버에만 저장</td>
<td>로컬 + 서버 모두 저장</td>
</tr>
<tr>
<td>특징</td>
<td>서버 장애 시 복구 불가</td>
<td>로컬에서 복구 가능</td>
</tr>
<tr>
<td>협업 방식</td>
<td>항상 서버에 접근 필요</td>
<td>개별 작업 후 통합 가능</td>
</tr>
<tr>
<td>서버 예시</td>
<td>X</td>
<td>GitHub, GitLab 등</td>
</tr>
</tbody></table>
<blockquote>
<p>☕️ Git ≠ GitHub<br />Git: 버전 관리 도구<br />GitHub: Git 기반 저장소 서비스<br /><code>Git : 커피</code> ☕️ → <code>GitHub : 카페</code> 🏡</p>
</blockquote>
<hr />
<h3 id="🧠-git의-특징-요약">🧠 Git의 특징 요약</h3>
<ol>
<li><p><strong>분산 버전 관리</strong><br />→ 브랜치 기능으로 다양한 버전 동시 개발 가능</p>
</li>
<li><p><strong>성능</strong><br />→ 빠른 속도와 대용량 프로젝트 지원</p>
</li>
<li><p><strong>오픈 소스</strong><br />→ 누구나 자유롭게 기여 가능한 커뮤니티 기반</p>
</li>
<li><p><strong>공개 저장소 활용</strong><br />→ GitHub를 통해 협업 및 프로젝트 공유에 용이</p>
</li>
</ol>
<hr />
<h3 id="🗂️-github을-쓰는-이유">🗂️ GitHub을 쓰는 이유</h3>
<ul>
<li>Git을 이용한 버전 관리</li>
<li>포트폴리오 및 협업 도구로 활용 가능</li>
</ul>
<hr />
<h3 id="📦-git-구조">📦 Git 구조</h3>
<blockquote>
<p>Git은 <strong>3가지 영역</strong>을 기반으로 커밋이 이루어짐</p>
</blockquote>
<table>
<thead>
<tr>
<th>영역</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td><strong>Working Directory</strong></td>
<td>실제 작업 중인 디렉토리</td>
</tr>
<tr>
<td><strong>Staging Area</strong></td>
<td>커밋할 파일을 담아두는 임시 저장소</td>
</tr>
<tr>
<td><strong>Repository</strong></td>
<td>커밋된 파일들이 저장되는 영역</td>
</tr>
</tbody></table>
<br />

<p>작업 → git add → git commit</p>
<ul>
<li><code>untracked</code> → <code>staged</code> → <code>committed</code></li>
</ul>
<hr />
<h3 id="❓-staging-area가-중요한-이유">❓ Staging Area가 중요한 이유</h3>
<h4 id="✅-일부분만-커밋하고-싶을-때">✅ 일부분만 커밋하고 싶을 때</h4>
<ul>
<li>원하는 파일만 골라서 커밋 가능</li>
</ul>
<h4 id="✅-충돌-해결-시">✅ 충돌 해결 시</h4>
<ul>
<li>충돌 중간 상태를 안전하게 저장하는 완충지대 역할</li>
</ul>
<h4 id="✅-커밋-수정-시">✅ 커밋 수정 시</h4>
<ul>
<li><code>git commit --amend</code>로 메시지 또는 파일 내용 수정 가능</li>
</ul>
<hr />
<h3 id="🛠️-주요-git-명령어">🛠️ 주요 Git 명령어</h3>
<table>
<thead>
<tr>
<th>명령어</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td><code>git init</code></td>
<td>로컬 저장소 초기화</td>
</tr>
<tr>
<td><code>git status</code></td>
<td>현재 파일 상태 확인</td>
</tr>
<tr>
<td><code>git add .</code></td>
<td>모든 변경 파일을 Staging Area에 올림</td>
</tr>
<tr>
<td><code>git commit -m &quot;메시지&quot;</code></td>
<td>커밋 생성</td>
</tr>
<tr>
<td><code>git restore 파일명</code></td>
<td>수정사항 취소</td>
</tr>
<tr>
<td><code>git commit --amend</code></td>
<td>마지막 커밋 수정</td>
</tr>
</tbody></table>
<hr />
<h3 id="🌐-github-연동-흐름">🌐 GitHub 연동 흐름</h3>
<pre><code class="language-text">[Working Directory] 
      ↓ git add
[Staging Area] 
      ↓ git commit
[Local Repository] 
      ↓ git push
[Remote Repository]

🔁 로컬 ↔ 원격 저장소 명령어
명령어    설명
git clone [URL]    원격 저장소 복제
git push origin main    로컬 → 원격 푸시
git pull    원격 → 로컬 가져오기
</code></pre>