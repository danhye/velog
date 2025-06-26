<p>회원탈퇴 작업</p>
<p>MemberMapper.java</p>
<pre><code>    //회원탈퇴
    @Update(&quot;UPDATE spmember SET auth=0 WHERE mem_num=#{mem_num}&quot;) //#{여기에!}
    public void deleteMember(Integer mem_num); //이 변수가
    @Delete(&quot;DELETE FROM spmember_detail WHERE mem_num=#{mem_num}&quot;)
    public void deleteMember_detail(Integer mem_num);</code></pre><p>MemberServiceImpl.java</p>
<pre><code>    @Override
    public void deleteMember(Integer mem_num) {
        memberMapper.deleteMember(mem_num);
        memberMapper.deleteMember_detail(mem_num);

    }</code></pre><p>MemberController.java</p>
<pre><code>    /* =======================
     *    회원탈퇴(회원정보삭제)
     * =======================*/
    //회원탈퇴 폼 호출
    @GetMapping(&quot;/member/delete.do&quot;)
    public String formDelete() {
        return &quot;memberDelete&quot;; //타일스 설정
    }</code></pre><p>return &quot;common/resultView&quot;;</p>
<p>JSP단독호출할때는 이런식으로 입력한다 common(폴더) / resultView(파일명) 임</p>
<p>​</p>
<p>views&gt;member&gt;memberDelete.jsp 파일생성!</p>
<p>memberDelete.jsp</p>
<pre><code>&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=UTF-8&quot;
    pageEncoding=&quot;UTF-8&quot;%&gt;
&lt;%@ taglib prefix=&quot;form&quot; uri=&quot;http://www.springframework.org/tags/form&quot; %&gt;
&lt;!-- 회원탈퇴 폼 시작 --&gt;
&lt;div class=&quot;page-main&quot;&gt;
    &lt;h2&gt;회원탈퇴&lt;/h2&gt;
    &lt;form:form modelAttribute=&quot;memberVO&quot; action=&quot;delete.do&quot; id=&quot;member_delete&quot;&gt;
        &lt;form:errors element=&quot;div&quot; cssClass=&quot;error-color&quot;/&gt;
        &lt;ul&gt;
            &lt;li&gt;
                &lt;form:label path=&quot;id&quot;&gt;아이디&lt;/form:label&gt;
                &lt;form:input path=&quot;id&quot;/&gt; 
                &lt;form:errors path=&quot;id&quot; cssClass=&quot;error-color&quot;/&gt;
            &lt;/li&gt;
            &lt;li&gt;
                &lt;form:label path=&quot;passwd&quot;&gt;비밀번호&lt;/form:label&gt;
                &lt;form:password path=&quot;passwd&quot;/&gt;
                &lt;form:errors path=&quot;passwd&quot; cssClass=&quot;error-color&quot;/&gt;
            &lt;/li&gt;
        &lt;/ul&gt;
        &lt;div class=&quot;align-center&quot;&gt;
            &lt;form:button&gt;전송&lt;/form:button&gt;
            &lt;input type=&quot;button&quot; value=&quot;MY페이지&quot; onclick=&quot;location.href='myPage.do'&quot;&gt;
        &lt;/div&gt;
    &lt;/form:form&gt;
&lt;/div&gt;
&lt;!-- 회원탈퇴 폼 끝 --&gt;</code></pre><p>member.xml</p>
<pre><code>    &lt;definition name=&quot;memberDelete&quot; extends=&quot;myPage&quot;&gt;
        &lt;put-attribute name=&quot;title&quot; value=&quot;회원탈퇴&quot; /&gt;
        &lt;put-attribute name=&quot;body&quot;
            value=&quot;/WEB-INF/views/member/memberDelete.jsp&quot; /&gt;
    &lt;/definition&gt;</code></pre><p>AppConfig.java</p>
<pre><code>    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(loginCheck).addPathPatterns(&quot;/member/myPage.do&quot;)
                                           .addPathPatterns(&quot;/member/update.do&quot;)
                                           .addPathPatterns(&quot;/member/changePassword.do&quot;)
                                           .addPathPatterns(&quot;/member/delete.do&quot;);
        //모든 요청에 interceptor가 동작되지 않고 요청한 주소에서만 동작됨. myPage.do를 요청했을때만!!                            
    }</code></pre><p>MemberController.java</p>
<p>logger부분 어노테이션 처리 -&gt; @Slf4j하고  logger를 전부 log로 바꾼다</p>
<pre><code>@Slf4j //롬복으로 로그 지정하기! 그러면 밑의 로그대상지정 단계가 필요없다. log -&gt; log로 명칭 변경된다!!
@Controller
public class MemberController {
    /*
     * //로그 대상 지정 private static final log log =
     * logFactory.getlog(MemberController.class);
     * 
     * 롬복처리 인해 필요없는 과정
     */</code></pre><p>그리고 회원탈퇴 전송 부분 처리</p>
<pre><code>/* =======================
     *    회원탈퇴(회원정보삭제)
     * =======================*/
    //회원탈퇴 폼 호출
    @GetMapping(&quot;/member/delete.do&quot;)
    public String formDelete() {
        return &quot;memberDelete&quot;; //타일스 설정
    }
    //전송된 데이터 처리
    @PostMapping(&quot;/member/delete.do&quot;)
    public String submitDelete(@Valid MemberVO memberVO,BindingResult result, HttpSession session, Model model) {
        log.debug(&quot;&lt;&lt;회원탈퇴&gt;&gt; : &quot; + memberVO);

        //id와 passwd 필드만 유효성 체크,유효성 체크 결과 오류가 있으면 폼 호출
        if(result.hasFieldErrors(&quot;id&quot;) || result.hasFieldErrors(&quot;passwd&quot;)) {
            return formDelete();
        }

        MemberVO user = (MemberVO)session.getAttribute(&quot;user&quot;);
        MemberVO db_member = memberService.selectMember(user.getMem_num());
        boolean check = false;
        //아이디, 비밀번호 일치 여부 체크
        try {
            if(db_member!=null &amp;&amp; db_member.getId().equals(memberVO.getId())) {
                //비밀번호 일치 여부 체크
                check = db_member.isCheckedPassword(memberVO.getPasswd());
            }
            if(check) {
                //인증 성공, 회원정보 삭제
                memberService.deleteMember(user.getMem_num());
                //로그아웃
                session.invalidate();

                model.addAttribute(&quot;accessMsg&quot;, &quot;회원탈퇴를 완료했습니다&quot;);
                return &quot;common/notice&quot;; //단독으로 jsp호출
            }
            //인증 실패
            throw new AuthCheckException();
        }catch(AuthCheckException e) {
            result.reject(&quot;invalidIdOrPassword&quot;);
            return formDelete();
        }
    }</code></pre><p>소소한 디자인 고치기</p>
<p>notice.jsp</p>
<pre><code>&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=UTF-8&quot;
    pageEncoding=&quot;UTF-8&quot;%&gt;
&lt;%@ taglib prefix=&quot;c&quot; uri=&quot;http://java.sun.com/jsp/jstl/core&quot; %&gt;    
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;meta charset=&quot;UTF-8&quot;&gt;
&lt;title&gt;안내&lt;/title&gt;
&lt;link rel=&quot;stylesheet&quot; href=&quot;${pageContext.request.contextPath}/css/layout.css&quot;&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;div class=&quot;page-one&quot;&gt;
    &lt;!-- 내용 시작 --&gt;
    &lt;h2&gt;안내&lt;/h2&gt;
    &lt;div class=&quot;result-display&quot;&gt;
        &lt;div class=&quot;align-center&quot;&gt;
            &lt;c:if test=&quot;${!empty accessMsg}&quot;&gt;
                ${accessMsg}
            &lt;/c:if&gt;
            &lt;c:if test=&quot;${empty accessMsg}&quot;&gt;
                잘못된 접속입니다.
            &lt;/c:if&gt;
            &lt;p&gt;
            &lt;c:if test=&quot;${!empty accessUrl}&quot;&gt;
            &lt;input type=&quot;button&quot; value=&quot;이동&quot;
              onclick=&quot;location.href='${accessUrl}'&quot;&gt;
            &lt;/c:if&gt;
            &lt;c:if test=&quot;${empty accessUrl}&quot;&gt;
            &lt;input type=&quot;button&quot; value=&quot;이동&quot;
             onclick=&quot;location.href='${pageContext.request.contextPath}/main/main.do'&quot;&gt;
            &lt;/c:if&gt;
        &lt;/div&gt;
    &lt;/div&gt;
    &lt;!-- 내용 끝 --&gt;
&lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre><p>kr.spring.member.controller &gt; MemberAdminController 만든당</p>
<p>어드민컨트롤러는 따로 해주기위해서 </p>
<p>​</p>
<p>MemberAdminController.java</p>
<pre><code>package kr.spring.member.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;

import kr.spring.member.service.MemberService;
import lombok.extern.slf4j.Slf4j;

@Slf4j
@Controller
public class MemberAdminController {
    @Autowired
    private MemberService memberService;


}</code></pre><p>MainController.java</p>
<pre><code>package kr.spring.main.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller //자동스캔대상이 되려면 꼭 넣어줘야해
public class MainController {

    @RequestMapping(&quot;/&quot;)
    public String main() {
        return &quot;redirect:/main/main.do&quot;;
    }

    @RequestMapping(&quot;/main/main.do&quot;)
    public String main(Model model) { //Model 객체는 컨트롤러에서 데이터를 생성해 이를 JSP 즉 View에 전달하는 역할

        return &quot;main&quot;;//타일스 설정의 식별자
    }

    @RequestMapping(&quot;/main/admin.do&quot;)
    public String adminMain(Model model) {

        return &quot;admin&quot;;//타일스 설정의 식별자
    }
}</code></pre><p>main.xml</p>
<pre><code>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; ?&gt;

&lt;!DOCTYPE tiles-definitions PUBLIC
       &quot;-//Apache Software Foundation//DTD Tiles Configuration 3.0//EN&quot;
       &quot;http://tiles.apache.org/dtds/tiles-config_3_0.dtd&quot;&gt;

&lt;tiles-definitions&gt;
    &lt;definition name=&quot;main&quot; template=&quot;/WEB-INF/views/template/layout_basic.jsp&quot;&gt; &lt;!--definition: UI조합그룹 --&gt;
        &lt;put-attribute name=&quot;title&quot; value=&quot;SpringPage&quot;/&gt;
        &lt;put-attribute name=&quot;header&quot; value=&quot;/WEB-INF/views/template/header.jsp&quot;/&gt;
        &lt;put-attribute name=&quot;body&quot; value=&quot;/WEB-INF/views/main/main.jsp&quot;/&gt; &lt;!-- body는 매번 바뀌어서 template안에 넣지않음 --&gt;
        &lt;put-attribute name=&quot;footer&quot; value=&quot;/WEB-INF/views/template/footer.jsp&quot;/&gt;
    &lt;/definition&gt;
    &lt;!-- 관리자 --&gt;
     &lt;definition name=&quot;admin&quot; template=&quot;/WEB-INF/views/template/layout_admin.jsp&quot;&gt; 
        &lt;put-attribute name=&quot;title&quot; value=&quot;관리자페이지&quot;/&gt;
        &lt;put-attribute name=&quot;header&quot; value=&quot;/WEB-INF/views/template/header.jsp&quot;/&gt;
        &lt;put-attribute name=&quot;nav&quot; value=&quot;/WEB-INF/views/template/admin_nav.jsp&quot;&gt;&lt;/put-attribute&gt;
        &lt;put-attribute name=&quot;body&quot; value=&quot;/WEB-INF/views/main/admin.jsp&quot;/&gt;
        &lt;put-attribute name=&quot;footer&quot; value=&quot;/WEB-INF/views/template/footer.jsp&quot;/&gt;
    &lt;/definition&gt;
&lt;/tiles-definitions&gt;</code></pre><p>관리자부분 main도 만들어줬슴미다</p>
<p>​</p>
<p>​</p>
<p>views&gt;main&gt;admin.jsp 뷰도 생성</p>
<p>​</p>
<p>admin.jsp</p>
<pre><code>&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=UTF-8&quot;
    pageEncoding=&quot;UTF-8&quot;%&gt;
&lt;%@ taglib prefix=&quot;c&quot; uri=&quot;http://java.sun.com/jsp/jstl/core&quot;%&gt;
&lt;%@ taglib prefix=&quot;fmt&quot; uri=&quot;http://java.sun.com/jsp/jstl/fmt&quot;%&gt;
&lt;!-- 관리자 메인 시작 --&gt;
&lt;div class=&quot;page-main&quot;&gt;
    &lt;h2&gt;회원목록&lt;/h2&gt;
&lt;/div&gt;
&lt;!-- 관리자 메인 끝 --&gt;</code></pre><p>views&gt;template&gt;admin_nav.jsp 생성</p>
<p>​</p>
<p>admin_nav.jsp</p>
<pre><code>&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=UTF-8&quot;
    pageEncoding=&quot;UTF-8&quot;%&gt;
&lt;!-- Admin 메뉴 시작 --&gt;
&lt;div class=&quot;side-bar&quot;&gt;
    &lt;ul&gt;
        &lt;li&gt;
            &lt;input type=&quot;button&quot; class=&quot;menu-btn&quot; value=&quot;회원관리&quot; 
            onclick=&quot;location.href='${pageContext.request.contextPath}/member/admin_list.do'&quot;&gt;
        &lt;/li&gt;
        &lt;li&gt;
            &lt;input type=&quot;button&quot; class=&quot;menu-btn&quot; value=&quot;상품관리&quot; 
            onclick=&quot;location.href='${pageContext.request.contextPath}/item/admin_list.do'&quot;&gt;
        &lt;/li&gt;
        &lt;li&gt;
            &lt;input type=&quot;button&quot; class=&quot;menu-btn&quot; value=&quot;주문관리&quot; 
            onclick=&quot;location.href='${pageContext.request.contextPath}/order/admin_list.do'&quot;&gt;
        &lt;/li&gt;
    &lt;/ul&gt;
&lt;/div&gt;
&lt;!-- Admin 메뉴 끝 --&gt;</code></pre><p>views&gt;template&gt;layout_admin.jsp 생성</p>
<p>​</p>
<p>layout_admin.jsp</p>
<pre><code>&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=UTF-8&quot;
    pageEncoding=&quot;UTF-8&quot;%&gt;
&lt;%@taglib prefix=&quot;tiles&quot; uri=&quot;http://tiles.apache.org/tags-tiles&quot; %&gt;
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;meta charset=&quot;UTF-8&quot;&gt;
&lt;title&gt;&lt;tiles:getAsString name=&quot;title&quot;/&gt;&lt;/title&gt;
&lt;link rel=&quot;stylesheet&quot; href=&quot;${pageContext.request.contextPath}/css/layout.css&quot;&gt;
&lt;script type=&quot;text/javascript&quot; src=&quot;${pageContext.request.contextPath}/js/jquery-3.6.0.min.js&quot;&gt;&lt;/script&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;div id=&quot;main&quot;&gt;
    &lt;div id=&quot;main_header&quot;&gt;
        &lt;tiles:insertAttribute name=&quot;header&quot;/&gt;
    &lt;/div&gt;
    &lt;div class=&quot;side-height&quot;&gt;
        &lt;div id=&quot;page_nav&quot;&gt;
            &lt;tiles:insertAttribute name=&quot;nav&quot;/&gt;
        &lt;/div&gt;
        &lt;div id=&quot;page_body&quot;&gt;
            &lt;tiles:insertAttribute name=&quot;body&quot;/&gt;
        &lt;/div&gt;
    &lt;/div&gt;
    &lt;div id=&quot;main_footer&quot; class=&quot;page_clear&quot;&gt; &lt;!-- float막으려고 page_clear 넣은거 --&gt;
        &lt;tiles:insertAttribute name=&quot;footer&quot;/&gt;
    &lt;/div&gt;
&lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre><p>kr.spring.util 에 pagingUtil파일 넣어줍니드..</p>
<p>​</p>
<p>MemberMapper.java</p>
<pre><code>    //회원관리 - 관리자
    public int selectRowCount(Map&lt;String,Object&gt;map);
    public List&lt;MemberVO&gt; selectList(Map&lt;String,Object&gt; map);
    public void updateByAdmin(MemberVO memberVO);</code></pre><p>똑같은 코드를 MemberService.java에도 붙여넣고</p>
<p>MemberServiceImpl.java</p>
<pre><code>    @Override
    public int selectRowCount(Map&lt;String, Object&gt; map) {
        // TODO Auto-generated method stub
        return 0;
    }

    @Override
    public List&lt;MemberVO&gt; selectList(Map&lt;String, Object&gt; map) {
        // TODO Auto-generated method stub
        return null;
    }

    @Override
    public void updateByAdmin(MemberVO memberVO) {
        // TODO Auto-generated method stub

    }</code></pre><p>MemberMapper.xml</p>
<pre><code>&lt;!-- 관리자 회원목록 --&gt;
    &lt;!-- sql태그와 includ태그를 이용해서 SQL문을 재사용 --&gt;
    &lt;sql id=&quot;memberSearch&quot;&gt;
        &lt;where&gt; &lt;!-- 동적SQL. 조건이 만족되지 않으면 where절이 만들어지지 않음. --&gt;
            &lt;if test=&quot;keyword != null and keyword != ''&quot;&gt;
                &lt;if test=&quot;keyfield == 1&quot;&gt;
                    m.id LIKE '%' || #{keyword} || '%'
                &lt;/if&gt;
                &lt;if test=&quot;keyfield == 2&quot;&gt;
                    d.name LIKE '%' || #{keyword} || '%'
                &lt;/if&gt;
                &lt;if test=&quot;keyfield == 3&quot;&gt;
                    d.email LIKE '%' || #{keyword} || '%'
                &lt;/if&gt;
                &lt;if test=&quot;keyfield == 4&quot;&gt;
                    m.id LIKE '%' || #{keyword} || '%' or
                    d.name LIKE '%' || #{keyword} || '%' or
                    d.email LIKE '%' || #{keyword} || '%'
                &lt;/if&gt;
            &lt;/if&gt;
        &lt;/where&gt;
    &lt;/sql&gt;

    &lt;!-- 전체/검색 레코드 수 --&gt;
    &lt;select id=&quot;selectRowCount&quot; parameterType=&quot;map&quot; resultType=&quot;integer&quot;&gt;
        SELECT
            COUNT (*)
        FROM spmember m LEFT OUTER JOIN spmember_detail d
        ON m.mem_num = d.mem_num
        &lt;include refid=&quot;memberSearch&quot;&gt;&lt;/include&gt; &lt;!-- include를 쓰면 코드 단순, 여러곳에서 이용가능 장점이 있다! --&gt;
    &lt;/select&gt;

    &lt;!-- 전체/검색 목록 --&gt;
    &lt;select id=&quot;selectList&quot; parameterType=&quot;map&quot; resultType=&quot;memberVO&quot;&gt; &lt;!-- resultType 하나의 레코드가 자바빈에 담긴다--&gt;
        SELECT
            *
        FROM ( SELECT
                  a.*,
                  rownum rnum
                FROM ( SELECT
                            *
                        FROM spmember m LEFT OUTER JOIN spmember_detail d
                        ON m.mem_num = d.mem_num
                        &lt;include refid=&quot;memberSearch&quot;&gt;&lt;/include&gt;
                        ORDER BY d.reg_date DESC NULLS LAST)a)
        &lt;![CDATA[
        WHERE rnum &gt;= #{start} AND rnum &lt;= #{end}
        ]]&gt; &lt;!-- 여기 안에 있는 건 문법체크를 안행 --&gt;
    &lt;/select&gt;</code></pre><p>MemberServiceImpl.java</p>
<pre><code>    @Override
    public int selectRowCount(Map&lt;String, Object&gt; map) {
        return memberMapper.selectRowCount(map);
    }

    @Override
    public List&lt;MemberVO&gt; selectList(Map&lt;String, Object&gt; map) {
        return memberMapper.selectList(map);
    }</code></pre><p>MemberAdminController.java</p>
<pre><code>package kr.spring.member.controller;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.ModelAndView;

import kr.spring.member.service.MemberService;
import kr.spring.member.vo.MemberVO;
import kr.spring.util.PagingUtil;
import lombok.extern.slf4j.Slf4j;

@Slf4j
@Controller
public class MemberAdminController {
    @Autowired
    private MemberService memberService;

    /*=================================
     * 회원목록 - 관리자
     * ================================*/
    @RequestMapping(&quot;/member/admin_list.do&quot;)
    public ModelAndView getList(@RequestParam(value=&quot;pageNum&quot;,defaultValue=&quot;1&quot;)int currentPage, 
                                                                        String keyfield,String keyword) {
        Map&lt;String,Object&gt; map = new HashMap&lt;String,Object&gt;();
        map.put(&quot;keyfield&quot;, keyfield);
        map.put(&quot;keyword&quot;, keyword);

        //전체/검색 레코드 수 
        int count = memberService.selectRowCount(map);

        log.debug(&quot;&lt;&lt;count&gt;&gt; : &quot; + count);

        //페이지 처리
        PagingUtil page = new PagingUtil(keyfield,keyword,currentPage,count,20,10,&quot;admin_list.do&quot;);

        List&lt;MemberVO&gt; list = null;
        if(count &gt; 0) {
            map.put(&quot;start&quot;, page.getStartRow());
            map.put(&quot;end&quot;, page.getEndRow());

            list = memberService.selectList(map);
        }

        ModelAndView mav = new ModelAndView();
        mav.setViewName(&quot;admin_memberList&quot;);
        mav.addObject(&quot;count&quot;,count);
        mav.addObject(&quot;list&quot;,list);
        mav.addObject(&quot;page&quot;,page.getPage());


        return mav;
    }

}</code></pre><p>member&gt;admin_memberList.jsp 만든다</p>
<p>​</p>
<p>admin_memberList.jsp</p>
<pre><code>&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=UTF-8&quot;
    pageEncoding=&quot;UTF-8&quot;%&gt;
&lt;%@ taglib prefix=&quot;c&quot; uri=&quot;http://java.sun.com/jsp/jstl/core&quot;%&gt;
&lt;!-- 회원목록 관리자 시작 --&gt;
&lt;script type=&quot;text/javascript&quot;&gt;
    $(function(){
        //검색 유효성 체크
        $('#search_form').submit(function(){
            if($('#keyword').val().trim()==''){
                alert('검색어를 입력하세요');
                $('#keyword').val('').focus();
                return false;
            }
        });
    });
&lt;/script&gt;
&lt;div class=&quot;page-main&quot;&gt;
    &lt;h2&gt;회원목록(관리자용)&lt;/h2&gt;
    &lt;form action=&quot;admin_list.do&quot; id=&quot;search_form&quot; method=&quot;get&quot;&gt;
        &lt;ul class=&quot;search&quot;&gt;
            &lt;li&gt;
                &lt;select name=&quot;keyfield&quot;&gt;
                    &lt;option value=&quot;1&quot; &lt;c:if test=&quot;${param.keyfield == 1}&quot;&gt;selected&lt;/c:if&gt;&gt;ID&lt;/option&gt;
                    &lt;option value=&quot;2&quot; &lt;c:if test=&quot;${param.keyfield == 2}&quot;&gt;selected&lt;/c:if&gt;&gt;이름&lt;/option&gt;
                    &lt;option value=&quot;3&quot; &lt;c:if test=&quot;${param.keyfield == 3}&quot;&gt;selected&lt;/c:if&gt;&gt;이메일&lt;/option&gt;
                    &lt;option value=&quot;4&quot; &lt;c:if test=&quot;${param.keyfield == 4}&quot;&gt;selected&lt;/c:if&gt;&gt;전체&lt;/option&gt;
                &lt;/select&gt;
            &lt;/li&gt;
            &lt;li&gt;
                &lt;input type=&quot;search&quot; name=&quot;keyword&quot; id=&quot;keyword&quot; value=&quot;${param.keyword}&quot;&gt;
            &lt;/li&gt;
            &lt;li&gt;
                &lt;input type=&quot;submit&quot; value=&quot;찾기&quot;&gt;
                &lt;input type=&quot;button&quot; value=&quot;목록&quot; onclick=&quot;location.href='admin_list.do'&quot;&gt;
            &lt;/li&gt;
        &lt;/ul&gt;
    &lt;/form&gt;
    &lt;c:if test=&quot;${count == 0}&quot;&gt;
    &lt;div class=&quot;result-display&quot;&gt;표시할 회원정보가 없습니다.&lt;/div&gt;
    &lt;/c:if&gt;
    &lt;c:if test=&quot;${count &gt; 0}&quot;&gt;
    &lt;table class=&quot;striped-table&quot;&gt;
        &lt;tr&gt;
            &lt;th&gt;아이디&lt;/th&gt;
            &lt;th&gt;이름&lt;/th&gt;
            &lt;th&gt;전화번호&lt;/th&gt;
            &lt;th&gt;이메일&lt;/th&gt;
            &lt;th&gt;가입일&lt;/th&gt;
            &lt;th&gt;권한&lt;/th&gt;
        &lt;/tr&gt;
        &lt;c:forEach var=&quot;member&quot; items=&quot;${list}&quot;&gt;
        &lt;tr&gt;
            &lt;td&gt;
                &lt;c:if test=&quot;${member.auth==0}&quot;&gt;${member.id}&lt;/c:if&gt;
                &lt;c:if test=&quot;${member.auth &gt; 0}&quot;&gt;&lt;a href=&quot;admin_update.do?mem_num=${member.mem_num}&quot;&gt;${member.id}&lt;/a&gt;&lt;/c:if&gt;
            &lt;/td&gt;
            &lt;td&gt;${member.name}&lt;/td&gt;
            &lt;td&gt;${member.phone}&lt;/td&gt;
            &lt;td&gt;${member.email}&lt;/td&gt;
            &lt;td&gt;${member.reg_date}&lt;/td&gt;
            &lt;td&gt;
                &lt;c:if test=&quot;${member.auth == 0}&quot;&gt;탈퇴&lt;/c:if&gt;
                &lt;c:if test=&quot;${member.auth == 1}&quot;&gt;정지&lt;/c:if&gt;
                &lt;c:if test=&quot;${member.auth == 2}&quot;&gt;일반&lt;/c:if&gt;
                &lt;c:if test=&quot;${member.auth == 9}&quot;&gt;관리&lt;/c:if&gt;
            &lt;/td&gt;
        &lt;/tr&gt;
        &lt;/c:forEach&gt;
    &lt;/table&gt;
    &lt;div class=&quot;align-center&quot;&gt;${page}&lt;/div&gt;
    &lt;/c:if&gt;

&lt;/div&gt;
&lt;!-- 회원목록 관리자 끝 --&gt;</code></pre><p>layout.css</p>
<pre><code> /* 공통 목록
----------------------------------*/
form#search_form{
    width:98%;
    padding-bottom:0;
    border:none;
}               
ul.search{
    width:380px;
    list-style:none;
    padding:0;
    margin:0 auto;
}    
ul.search li{
    margin:0 0 9px 0;
    padding:0;
    display:inline;
}               
.list-space{
    width:700px;
    margin:10px auto;
}                
table.basic-table{
    width:100%;
    margin:7px 0;
    border:1px solid #000;
    border-collapse:collapse;
}
table.basic-table td, table.basic-table th{
    border:1px solid #000;
    padding:5px;
}
table.striped-table{
    border:1px #FFF solid;
    font-size:15px;
    width:100%;
    border-collapse:collapse;
    margin:7px 0;
}
table.striped-table td, table.striped-table th{
    padding:.7em .5em;
    vertical-align:middle;
}
table.striped-table th{
    font-weight:bold;
    background:#E1E1E1;
}
table.striped-table td{
    border-bottom:1px solid rgba(0,0,0,.1);
}
table.striped-table tr:nth-child(odd){
    background-color:rgb(250,250,247);
}</code></pre><p>member.xml</p>
<pre><code>&lt;definition name=&quot;memberDelete&quot; extends=&quot;myPage&quot;&gt;
        &lt;put-attribute name=&quot;title&quot; value=&quot;회원탈퇴&quot; /&gt;
        &lt;put-attribute name=&quot;body&quot;
            value=&quot;/WEB-INF/views/member/memberDelete.jsp&quot; /&gt;
&lt;/definition&gt;</code></pre><p>MemberMapperjava</p>
<pre><code>@Update(&quot;UPDATE spmember SET auth=#{auth} WHERE mem_num=#{mem_num}&quot;) //auth,mem_num 자바빈에 있는 프로퍼티
    public void updateByAdmin(MemberVO memberVO);</code></pre><p>MemberServiceImple.java</p>
<pre><code>    @Override
    public void updateByAdmin(MemberVO memberVO) {
        memberMapper.updateByAdmin(memberVO);
    }</code></pre><p>MemberAdminController.java</p>
<pre><code>    /*=================================
     * 회원권한변경 - 관리자
     * ================================*/
    //수정 폼
    @GetMapping(&quot;/member/admin_update.do&quot;)
    public String form(@RequestParam int mem_num,Model model) {
        MemberVO memberVO = memberService.selectMember(mem_num);

        model.addAttribute(&quot;memberVO&quot;,memberVO);

        return &quot;admin_memberModify&quot;;
    }</code></pre><p>views&gt;member&gt;admin_memberModify.jsp</p>
<p>​</p>
<p>admin_memberModify.jsp</p>
<pre><code>&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=UTF-8&quot;
    pageEncoding=&quot;UTF-8&quot;%&gt;
&lt;!-- layout에 쏙 들어가는 구조라서 밑의 html코드는 필요없성 --&gt;
&lt;%@ taglib prefix=&quot;form&quot; uri=&quot;http://www.springframework.org/tags/form&quot;%&gt;
&lt;%@ taglib prefix=&quot;c&quot; uri=&quot;http://java.sun.com/jsp/jstl/core&quot; %&gt;
&lt;!-- 회원권한 수정 - 관리자 시작 --&gt;
&lt;div class=&quot;page-main&quot;&gt;
    &lt;h2&gt;회원권한수정(관리자용)&lt;/h2&gt;
    &lt;form:form modelAttribute=&quot;memberVO&quot; action=&quot;admin_update.do&quot; id=&quot;modify_form&quot;&gt;
    &lt;form:hidden path=&quot;mem_num&quot;/&gt;
        &lt;form:errors element=&quot;div&quot; cssClass=&quot;error-color&quot;/&gt;
        &lt;ul&gt;
            &lt;li&gt;
                &lt;label&gt;회원권한&lt;/label&gt;
                &lt;c:if test=&quot;${memberVO.auth &lt; 9}&quot;&gt;
                &lt;form:radiobutton path=&quot;auth&quot; value=&quot;1&quot;/&gt;정지
                &lt;form:radiobutton path=&quot;auth&quot; value=&quot;2&quot;/&gt;일반
                &lt;/c:if&gt;
                &lt;c:if test=&quot;${memberVO.auth == 9}&quot;&gt;관리&lt;/c:if&gt;
            &lt;/li&gt;
        &lt;/ul&gt;
        &lt;ul&gt;    
            &lt;li&gt;
                &lt;label&gt;이름&lt;/label&gt;
                ${memberVO.name}
            &lt;/li&gt;
            &lt;li&gt;
                &lt;label&gt;전화번호&lt;/label&gt;
                ${memberVO.phone}
            &lt;/li&gt;
            &lt;li&gt;
                &lt;label&gt;이메일&lt;/label&gt;
                ${memberVO.email}
            &lt;/li&gt;
            &lt;li&gt;
                &lt;label&gt;우편번호&lt;/label&gt;
                ${memberVO.zipcode}
            &lt;/li&gt;
            &lt;li&gt;
                &lt;label&gt;주소&lt;/label&gt;
                ${memberVO.address1} ${memberVO.address2}
            &lt;/li&gt;
        &lt;/ul&gt;
        &lt;div class=&quot;align-center&quot;&gt;
            &lt;form:button class=&quot;default-btn&quot;&gt;수정&lt;/form:button&gt;
            &lt;input type=&quot;button&quot; value=&quot;홈으로&quot; class=&quot;default-btn&quot; 
            onclick=&quot;location.href='${pageContext.request.contextPath}/main/main.do'&quot;&gt;
        &lt;/div&gt;
    &lt;/form:form&gt;
&lt;/div&gt;
&lt;!-- 회원권한 수정 - 관리자 끝 --&gt;</code></pre><p>member.xml</p>
<pre><code>&lt;definition name=&quot;admin_memberModify&quot; extends=&quot;admin&quot;&gt;
        &lt;put-attribute name=&quot;title&quot; value=&quot;회원권한수정(관리자용)&quot; /&gt;
        &lt;put-attribute name=&quot;body&quot;
            value=&quot;/WEB-INF/views/member/admin_memberModify.jsp&quot; /&gt;
    &lt;/definition&gt;</code></pre><p>관리자 부분에 츄가</p>
<p>​</p>
<p>MemberAdminController.java</p>
<pre><code>    //전송된 데이터 처리
    @PostMapping(&quot;/member/admin_update.do&quot;)
    public String submit(MemberVO memberVO, Model model, HttpServletRequest request) {
        log.debug(&quot;&lt;&lt;관리자 회원권한 수정&gt;&gt; : &quot; + memberVO);

        //회원권한 수정
        memberService.updateByAdmin(memberVO);

        //View에 표시할 메세지
        model.addAttribute(&quot;message&quot;,&quot;회원권한 수정 완료&quot;);
        model.addAttribute(&quot;url&quot;,request.getContextPath() + &quot;/member/admin_update.do?mem_num=&quot; + memberVO.getMem_num());


        return &quot;common/resultView&quot;;
    }</code></pre><p>MainController.java</p>
<pre><code>package kr.spring.main.controller;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import kr.spring.member.service.MemberService;
import kr.spring.member.vo.MemberVO;
import lombok.extern.slf4j.Slf4j;

@Slf4j
@Controller //자동스캔대상이 되려면 꼭 넣어줘야해
public class MainController {
    @Autowired
    private MemberService memberService;

    @RequestMapping(&quot;/&quot;)
    public String main() {
        return &quot;redirect:/main/main.do&quot;;
    }

    @RequestMapping(&quot;/main/main.do&quot;)
    public String main(Model model) { //Model 객체는 컨트롤러에서 데이터를 생성해 이를 JSP 즉 View에 전달하는 역할

        return &quot;main&quot;;//타일스 설정의 식별자
    }

    @RequestMapping(&quot;/main/admin.do&quot;)
    public String adminMain(Model model) {

        Map&lt;String,Object&gt; map = new HashMap&lt;String,Object&gt;();
        map.put(&quot;start&quot;, 1);
        map.put(&quot;end&quot;, 5); //최신 5개의 데이터

        List&lt;MemberVO&gt; memberList = memberService.selectList(map);

        //최신 5명 회원정보 
        model.addAttribute(&quot;memberList&quot;,memberList);

        return &quot;admin&quot;;//타일스 설정의 식별자
    }
}</code></pre><p>admin.jsp</p>
<pre><code>&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=UTF-8&quot;
    pageEncoding=&quot;UTF-8&quot;%&gt;
&lt;%@ taglib prefix=&quot;c&quot; uri=&quot;http://java.sun.com/jsp/jstl/core&quot;%&gt;
&lt;%@ taglib prefix=&quot;fmt&quot; uri=&quot;http://java.sun.com/jsp/jstl/fmt&quot;%&gt;
&lt;!-- 관리자 메인 시작 --&gt;
&lt;div class=&quot;page-main&quot;&gt;
    &lt;h2&gt;회원목록&lt;/h2&gt;
    &lt;table class=&quot;striped-table&quot;&gt;
        &lt;tr&gt;
            &lt;th&gt;아이디&lt;/th&gt;
            &lt;th&gt;이름&lt;/th&gt;
            &lt;th&gt;전화번호&lt;/th&gt;
            &lt;th&gt;이메일&lt;/th&gt;
            &lt;th&gt;가입일&lt;/th&gt;
            &lt;th&gt;권한&lt;/th&gt;
        &lt;/tr&gt;
        &lt;c:forEach var=&quot;member&quot; items=&quot;${memberList}&quot;&gt;
        &lt;tr&gt;
            &lt;td&gt;
                &lt;c:if test=&quot;${member.auth==0}&quot;&gt;${member.id}&lt;/c:if&gt;
                &lt;c:if test=&quot;${member.auth &gt; 0}&quot;&gt;&lt;a href=&quot;admin_update.do?mem_num=${member.mem_num}&quot;&gt;${member.id}&lt;/a&gt;&lt;/c:if&gt;
            &lt;/td&gt;
            &lt;td&gt;${member.name}&lt;/td&gt;
            &lt;td&gt;${member.phone}&lt;/td&gt;
            &lt;td&gt;${member.email}&lt;/td&gt;
            &lt;td&gt;${member.reg_date}&lt;/td&gt;
            &lt;td&gt;
                &lt;c:if test=&quot;${member.auth == 0}&quot;&gt;탈퇴&lt;/c:if&gt;
                &lt;c:if test=&quot;${member.auth == 1}&quot;&gt;정지&lt;/c:if&gt;
                &lt;c:if test=&quot;${member.auth == 2}&quot;&gt;일반&lt;/c:if&gt;
                &lt;c:if test=&quot;${member.auth == 9}&quot;&gt;관리&lt;/c:if&gt;
            &lt;/td&gt;
        &lt;/tr&gt;
        &lt;/c:forEach&gt;
    &lt;/table&gt;
&lt;/div&gt;
&lt;!-- 관리자 메인 끝 --&gt;</code></pre>