<p>MemberController.java (회원가입 부분 작성)</p>
<pre><code>package kr.spring.member.controller;


import java.util.HashMap;
import java.util.Map;
import java.util.regex.Pattern;

import javax.validation.Valid;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

import kr.spring.member.service.MemberService;
import kr.spring.member.vo.MemberVO;

@Controller
public class MemberController {
    //로그 대상 지정
    private static final Logger logger = LoggerFactory.getLogger(MemberController.class);

    @Autowired
    private MemberService memberService;

    /* =======================
     * 자바빈 초기화
     * ======================= */
    @ModelAttribute
    public MemberVO initCommand() {
        return new MemberVO();
    }
     /* =======================
     * 회원가입
     * =======================*/
    //아이디 중복 체크
    @RequestMapping(&quot;/member/confirmId.do&quot;)
    @ResponseBody
    public Map&lt;String,String&gt; confirmId(@RequestParam String id){
        logger.debug(&quot;&lt;&lt;아이디 중복체크&gt;&gt; : &quot; + id);
        Map&lt;String,String&gt; mapAjax = new HashMap&lt;String,String&gt;();

        MemberVO member = memberService.selectCheckMember(id);
        if(member!=null) { //아이디 중복 
            mapAjax.put(&quot;result&quot;,&quot;idDuplicated&quot;); 
        }else {
            if(!Pattern.matches(&quot;^[A-Za-z0-9]{4,12}$&quot;, id)) { //패턴 불일치
                mapAjax.put(&quot;result&quot;,&quot;notMatchPattern&quot;); 
            }else {//패턴 일치하면서 아이디 미중복
                mapAjax.put(&quot;result&quot;,&quot;idNotFound&quot;); 
            } } 
        return mapAjax; 

    }

    //회원가입 폼 호출
    @GetMapping(&quot;/member/registerUser.do&quot;)
    public String form() {
        return &quot;memberRegister&quot;;
    }

    //회원가입 처리
    @PostMapping(&quot;/member/registerUser.do&quot;)
    public String submit(@Valid MemberVO memberVO, BindingResult result, Model model) {
        logger.debug(&quot;&lt;&lt;회원가입&gt;&gt; : &quot; + memberVO);

        //유효성 체크 결과 오류가 있으면 폼 호출
        if(result.hasErrors()) {
            return form();
        }

        //회원가입
        memberService.insertMember(memberVO);

        model.addAttribute(&quot;accessMsg&quot;, &quot;회원가입이 완료되었습니다.&quot;);

        return &quot;common/notice&quot;;
    }

}</code></pre><p>MemberMapper.xml (Id를 이용한 회원정보 체크 부분 작성)</p>
<pre><code>l (Id를 이용한 회원정보 체크 부분 작성)

&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;!DOCTYPE mapper PUBLIC &quot;-//mybatis.org//DTD Mapper 3.0//EN&quot; &quot;http://mybatis.org/dtd/mybatis-3-mapper.dtd&quot;&gt;
&lt;mapper namespace=&quot;kr.spring.member.dao.MemberMapper&quot;&gt;
    &lt;!-- 회원가입 - spmember_detail --&gt;
    &lt;insert id=&quot;insertMember_detail&quot; parameterType=&quot;memberVO&quot;&gt; &lt;!-- id는 메소드명 parameterType은 전달되어지는 객체 --&gt;
        INSERT INTO spmember_detail(
            mem_num,
            name,
            passwd,
            phone,
            email,
            zipcode,
            address1,
            address2)
        VALUES(
            #{mem_num},
            #{name},
            #{passwd},
            #{phone},
            #{email},
            #{zipcode},
            #{address1},
            #{address2}) &lt;!-- 자바빈의 프로퍼티 --&gt;

    &lt;/insert&gt;

    &lt;!-- ID를 이용한 회원정보 체크 --&gt;
    &lt;select id=&quot;selectCheckMember&quot; parameterType=&quot;string&quot; resultType=&quot;memberVO&quot;&gt;
        SELECT 
            m.mem_num,
            m.id,
            m.auth,
            d.au_id,
            d.passwd,
            m.nick_name,
            d.email
        FROM spmember m LEFT OUTER JOIN spmember_detail d
        ON m.mem_num=d.mem_num
        WHERE m.id=#{id}
    &lt;/select&gt;

&lt;/mapper&gt;</code></pre><p>MemberServiceImpl.java (selectCheckMember 부분 작성)</p>
<pre><code>package kr.spring.member.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import kr.spring.member.dao.MemberMapper;
import kr.spring.member.vo.MemberVO;

@Service
@Transactional
public class MemberServiceImpl implements MemberService{

    @Autowired
    private MemberMapper memberMapper;

    @Override
    public void insertMember(MemberVO member) {
        member.setMem_num(memberMapper.selectMem_num()); //밑의 세개가 다 sql문장
        memberMapper.insertMember(member);                 //세개를 트랜잭션으로 묶은거 ..? 그게 뭔 말일까 ㅋㅋ ㅜ
        memberMapper.insertMember_detail(member); 

    }

    @Override
    public MemberVO selectCheckMember(String id) {
        return memberMapper.selectCheckMember(id);
    }

    @Override
    public MemberVO selectMember(Integer mem_num) {
        // TODO Auto-generated method stub
        return null;
    }

    @Override
    public void updateMember(MemberVO member) {
        // TODO Auto-generated method stub

    }

    @Override
    public void updatePassword(MemberVO member) {
        // TODO Auto-generated method stub

    }

    @Override
    public void deleteMember(Integer mem_num) {
        // TODO Auto-generated method stub

    }

    @Override
    public void updateAu_id(String au_id, int mem_num) {
        // TODO Auto-generated method stub

    }

    @Override
    public void selectAu_id(String au_id) {
        // TODO Auto-generated method stub

    }

    @Override
    public void deleteAu_id(int mem_num) {
        // TODO Auto-generated method stub

    }

}</code></pre><p>confirmId.js</p>
<pre><code>$(function(){
    let checkId = 0;

    //아이디 중복 체크
    $(&quot;#confirmId&quot;).click(function(){
        if($('#id').val().trim()==''){   //val():선택한 양식의 값을 가져온다.
            $('#message_id').css('color','red').text('아이디를 입력하세요');
            $('#id').val('').focus();
        }

        $('#message_id').text('');//메세지 초기화
        $.ajax({
            url:'confirmId.do',
            type:'post',
            data:{id:$('#id').val()},
            dataType:'json',
            success:function(param){
                if(param.result == 'idNotFound'){
                    $('#message_id').css('color','#000').text('등록가능ID');
                    checkId = 1;
                }else if(param.result == 'idDuplicated'){
                    $('#message_id').css('color','red').text('중복된 ID');
                    $('#id').val('').focus();
                    checkId = 0;
                }else if(param.result == 'notMatchPattern'){
                    $('#message_id').css('color','red').text('영문,숫자 4자 이상 12자이하 입력');
                    $('#id').val('').focus();
                    checkId = 0;
                }else{
                    checkId = 0;
                    alert('ID중복체크 오류');
                }
            },
            error:function(){
                checkId = 0;
                alert('네트워크 오류 발생');
            }
        });

    });//end of click

    //아이디 중복 안내 메시지 초기화 및 아이디 중복 값 초기화
    $('#member_register #id').keydown(function(){
        checkId = 0;
        $('#message_id').text(''); //비어있게!
    }); //end of keydown

    //submit 이벤트 발생시 아이디 중복체크 여부 확인
    $('#member_register').submit(function(){
        if(checkId==0){
            $('#message_id').css('color','red').text('아이디 중복 체크 필수');
            if($('#id').val().trim()==''){
                $('#id').val('').focus();
            }
            return false; //checkId가 0일 경우 return false인 것.
        }
    });//end if submit


});</code></pre><p>js 작성했으니까 memberRegister.jsp에 script 추가해주고 테스트!</p>
<p>그럼 아이디 중복체크가 된다.</p>
<p>header.jsp(마이페이지 로그아웃 쪽 작성)</p>
<pre><code>&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=UTF-8&quot;
    pageEncoding=&quot;UTF-8&quot;%&gt;
&lt;%@ taglib prefix=&quot;c&quot; uri=&quot;http://java.sun.com/jsp/jstl/core&quot;%&gt;
&lt;!-- 상단 시작 --&gt;
&lt;h2 class=&quot;align-center&quot;&gt;SpringPage&lt;/h2&gt;
&lt;div class=&quot;align-right&quot;&gt;
    &lt;c:if test=&quot;${!empty user &amp;&amp; user.auth ==2 }&quot;&gt;
        &lt;a href=&quot;${pageContext.request.contextPath}/member/myPage.do&quot;&gt;My페이지&lt;/a&gt;
    &lt;/c:if&gt;
    &lt;c:if test=&quot;${!empty user &amp;&amp; !empty user.nick_name}&quot;&gt;
        [&lt;span class=&quot;user_name&quot;&gt;${user.nick_name}&lt;/span&gt;]
    &lt;/c:if&gt;
    &lt;c:if test=&quot;${!empty user &amp;&amp; empty user.nick_name}&quot;&gt;
        [&lt;span class=&quot;user_name&quot;&gt;${user.id}&lt;/span&gt;]
    &lt;/c:if&gt;

    &lt;c:if test=&quot;${!empty user}&quot;&gt;
        &lt;a href=&quot;${pageContext.request.contextPath}/member/logout.do&quot;&gt;로그아웃&lt;/a&gt;
    &lt;/c:if&gt;

    &lt;c:if test=&quot;${empty user}&quot;&gt;
    &lt;a href=&quot;${pageContext.request.contextPath}/member/registerUser.do&quot;&gt;회원가입&lt;/a&gt;
    &lt;a href=&quot;${pageContext.request.contextPath}/member/login.do&quot;&gt;로그인&lt;/a&gt;
    &lt;/c:if&gt;
    &lt;c:if test=&quot;${empty user || user.auth &lt; 9}&quot;&gt;
    &lt;a href=&quot;${pageContext.request.contextPath}/main/main.do&quot;&gt;홈으로&lt;/a&gt;
    &lt;/c:if&gt;
&lt;/div&gt;

&lt;!-- 상단 끝 --&gt;</code></pre><p>MemberController.java</p>
<pre><code>    /* =======================
     * 회원로그인
     * =======================*/
    //로그인 폼
    @GetMapping(&quot;/member/login.do&quot;)
    public String formLogin() {
        return &quot;memberLogin&quot;;
    }
</code></pre><p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/e1ed9d70-1fc5-419b-9c49-68d5574ede6e/image.png" /></p>
<p>memberRegister에 커서가 가 있지만 </p>
<p>memberLogin.jsp를 만들어준당  로그인폼 만들기 !</p>
<p>memberLogin.jsp</p>
<pre><code>&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=UTF-8&quot;
    pageEncoding=&quot;UTF-8&quot;%&gt;
&lt;%@ taglib prefix=&quot;form&quot; uri=&quot;http://www.springframework.org/tags/form&quot; %&gt;
&lt;!-- 로그인 폼 시작 --&gt;
&lt;div class=&quot;page-main&quot;&gt;
    &lt;h2&gt;회원로그인&lt;/h2&gt;
    &lt;form:form modelAttribute=&quot;memberVO&quot; action=&quot;login.do&quot; id=&quot;member_login&quot;&gt;
        &lt;form:errors element=&quot;div&quot; cssClass=&quot;error-color&quot;/&gt;
        &lt;ul&gt;
            &lt;li class=&quot;floating-label&quot;&gt;
                &lt;form:input path=&quot;id&quot; placeholder=&quot;아이디&quot; cssClass=&quot;form-input&quot; autocomplete=&quot;off&quot;/&gt; &lt;!-- autocomplete자동완성 안되게 설정하는거임 --&gt;
                &lt;form:label path=&quot;id&quot;&gt;아이디&lt;/form:label&gt;
                &lt;form:errors path=&quot;id&quot; element=&quot;div&quot; cssClass=&quot;error-color&quot;/&gt;
            &lt;/li&gt;
            &lt;li class=&quot;floating-label&quot;&gt;
                &lt;form:password path=&quot;passwd&quot; placeholder=&quot;비밀번호&quot; cssClass=&quot;form-input&quot;/&gt;
                &lt;form:label path=&quot;passwd&quot;&gt;비밀번호&lt;/form:label&gt;
                &lt;form:errors path=&quot;passwd&quot; element=&quot;div&quot; cssClass=&quot;error-color&quot;/&gt;
            &lt;/li&gt;
            &lt;li&gt; &lt;!-- 자동로그인 기능 --&gt;
                &lt;input type=&quot;checkbox&quot; name=&quot;auto&quot; id=&quot;auto&quot;&gt;로그인상태유지
            &lt;/li&gt;
        &lt;/ul&gt;
        &lt;div class=&quot;align-center&quot;&gt;
            &lt;form:button class=&quot;login-btn&quot;&gt;로그인&lt;/form:button&gt;
        &lt;/div&gt;
    &lt;/form:form&gt;
&lt;/div&gt;
&lt;!-- 로그인 폼 끝 --&gt;</code></pre><p>자동로그인 기능은 아직 구현되지 않았다. 나중에</p>
<p>layout.css에 로그인 부분 css설정 추가했다.</p>
<p>추가한 전체적인 css설정은 </p>
<pre><code>@charset &quot;UTF-8&quot;;
/* 전체 레이아웃
----------------------------------*/
#main{
    width:1024px;
    margin:0 auto;
    font-size:10pt;
    font-family:&quot;맑은 고딕&quot;;
}
#main_header{
    margin-bottom:10px;
    padding:0 10px 0 0;
}
#main_body{
    padding:5px 5px 30px 5px;
    min-height:500px;
}
#main_footer{
    background-color:yellow;
}
a:link{
    text-decoration:none;
    color:#000000;
}
a:visited{
    text-decoration:none;
    color:#000000;
}
a:hover{
    text-decoration:underline;
    color:#999999;
}
/* 좌측 메뉴 레이아웃(Admin,MyPage)
----------------------------------*/

/* 페이지 기본 레이아웃
----------------------------------*/
.page-main{
    width:98%;
    margin:0 auto;
}
.page-one{
    width:600px;
    margin:0 auto;
}
/* 공통 메시지 및 버튼
----------------------------------*/
.result-display{
    width:400px;
    height:200px;
    margin:50px auto;
    border:1px solid #000;
    display:flex;
    align-items:center;
    justify-content:center;
}
.error-color{
    color:#ff0000;
}
.default-btn{
    padding:5px 20px;
    border:1px solid #09aa5c;
    border-radius:2px;
    color:#fff;
    background-color:#09aa5c;
    font-weight:bold;
    cursor:pointer;
}
.default-btn:hover{
    background-color:#fff;
    color:#09aa5c;
    transition:0.2s ease-out; /*ease-out 부드럽게 전환*/
}

/* 공통 정렬
----------------------------------*/
.align-center{
    text-align:center;
}
.align-right{
    text-align:right;
}

/* 상단 메뉴
----------------------------------*/
div#main_menu ul{
    list-style:none;
}               
div#main_menu ul li{
    margin-bottom:5px;
}

 /* 메인
----------------------------------*/

/* 공통 등록, 수정 폼
----------------------------------*/
form{
    width:600px;
    margin:0 auto;
    border:1px solid #000;
    padding:10px 10px 30px 10px;
}
form ul li{
    clear:both;
}
form ul li label{
    width:100px;
    float:left;
}
ul{
    list-style:none;
}
input{
    margin-top:4px;
    padding:5px;
}
input[type=&quot;text&quot;],input[type=&quot;password&quot;],input[type=&quot;email&quot;]{
    width:350px;
}

/* 회원가입 등록, 수정, 삭제 폼
----------------------------------*/
#member_register{
    width:800px;
}
#member_register ul, #member_modify ul{
    padding-left:120px;
}
#member_register ul li label{
    padding-top:6px;
}
#member_register input[id=&quot;id&quot;]{
    width:236px;
}
#member_register input[id=&quot;zipcode&quot;], #member_modify input[id=&quot;zipcode&quot;]{
    width:220px;
}
#member_modify{
    width:96%;
}
#member_delete input[id=&quot;id&quot;], #member_delete input[id=&quot;passwd&quot;]{
    width:240px;
}

/* 로그인
----------------------------------*/
#member_login{
    width:300px;
    padding:0;
    border:none;
}   
#member_login li{
    padding:0;
}           
.floating-label{
    position:relative;
}
.floating-label &gt; .form-input{
    width:300px;
    height:32px;
    padding:0.55rem 0.55rem; /* rem수치가 큰겨 */
}
.floating-label &gt; label{
    position:absolute;
    top:0;
    left:0;
    padding:1.25rem 0.65rem;
    transition:all 0.25s;
}
.floating-label &gt; .form-intput:placeholder{
    color:transparent; /* 투명하게 */
}
.floating-label &gt; .form-input:focus, .floating-label &gt; .form-intput:not(:placeholder-shown){
    padding-top:1.125rem;
    padding-bottom:0.125rem;
}
.floating-label &gt; .form-input:focus + label, .floating-label &gt;.form-input:not(:placeholder-shown) + label{
    opacity:0.65;
    transform:scale(0.85) translateY(-0.5rem) translateX(-0.5rem); /* 이동시키기 */
}
#member_login .login-btn{
    width:210px;
}
@charset &quot;UTF-8&quot;;
/* 전체 레이아웃
----------------------------------*/
#main{
    width:1024px;
    margin:0 auto;
    font-size:10pt;
    font-family:&quot;맑은 고딕&quot;;
}
#main_header{
    margin-bottom:10px;
    padding:0 10px 0 0;
}
#main_body{
    padding:5px 5px 30px 5px;
    min-height:500px;
}
#main_footer{
    background-color:yellow;
}
a:link{
    text-decoration:none;
    color:#000000;
}
a:visited{
    text-decoration:none;
    color:#000000;
}
a:hover{
    text-decoration:underline;
    color:#999999;
}
/* 좌측 메뉴 레이아웃(Admin,MyPage)
----------------------------------*/

/* 페이지 기본 레이아웃
----------------------------------*/
.page-main{
    width:98%;
    margin:0 auto;
}
.page-one{
    width:600px;
    margin:0 auto;
}
/* 공통 메시지 및 버튼
----------------------------------*/
.result-display{
    width:400px;
    height:200px;
    margin:50px auto;
    border:1px solid #000;
    display:flex;
    align-items:center;
    justify-content:center;
}
.error-color{
    color:#ff0000;
}
.default-btn{
    padding:5px 20px;
    border:1px solid #09aa5c;
    border-radius:2px;
    color:#fff;
    background-color:#09aa5c;
    font-weight:bold;
    cursor:pointer;
}
.default-btn:hover{
    background-color:#fff;
    color:#09aa5c;
    transition:0.2s ease-out; /*ease-out 부드럽게 전환*/
}

/* 공통 정렬
----------------------------------*/
.align-center{
    text-align:center;
}
.align-right{
    text-align:right;
}

/* 상단 메뉴
----------------------------------*/
div#main_menu ul{
    list-style:none;
}               
div#main_menu ul li{
    margin-bottom:5px;
}
/* 메인
----------------------------------*/

/* 공통 등록, 수정 폼
----------------------------------*/
form{
    width:600px;
    margin:0 auto;
    border:1px solid #000;
    padding:10px 10px 30px 10px;
}
form ul li{
    clear:both;
}
form ul li label{
    width:100px;
    float:left;
}
ul{
    list-style:none;
}
input{
    margin-top:4px;
    padding:5px;
}
input[type=&quot;text&quot;],input[type=&quot;password&quot;],input[type=&quot;email&quot;]{
    width:350px;
}

/* 회원가입 등록, 수정, 삭제 폼
----------------------------------*/
#member_register{
    width:800px;
}
#member_register ul, #member_modify ul{
    padding-left:120px;
}
#member_register ul li label{
    padding-top:6px;
}
#member_register input[id=&quot;id&quot;]{
    width:236px;
}
#member_register input[id=&quot;zipcode&quot;], #member_modify input[id=&quot;zipcode&quot;]{
    width:220px;
}
#member_modify{
    width:96%;
}
#member_delete input[id=&quot;id&quot;], #member_delete input[id=&quot;passwd&quot;]{
    width:240px;
}

/* 로그인
----------------------------------*/
#member_login{
    width:400px;
    padding:0;
    border:none;
}   
#member_login li{
    padding:0;
}           
.floating-label{
    position:relative;
}
.floating-label &gt; .form-input{
    width:300px;
    height:32px;
    padding:0.55rem 0.55rem; /* rem수치가 큰겨 */
}
.floating-label &gt; label{
    position:absolute;
    top:0;
    left:0;
    padding:1.25rem 0.65rem;
    transition:all 0.25s;
}
.floating-label &gt; .form-intput::placeholder{
    color:transparent; /* 투명하게 */
}
.floating-label &gt; .form-input:focus, .floating-label &gt; .form-intput:not(:placeholder-shown){
    padding-top:1.125rem;
    padding-bottom:0.125rem;
}
.floating-label &gt; .form-input:focus + label, .floating-label &gt;.form-input:not(:placeholder-shown) + label{
    opacity:0.65;
    transform:scale(0.85) translateY(-0.5rem) translateX(-0.5rem); /* 이동시키기 */
}
#member_login .login-btn{
    width:320px;
}</code></pre><p>WEB-INF &gt; tiles-def &gt; member.xml (회원로그인 부분 추가)</p>
<pre><code>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; ?&gt;

&lt;!DOCTYPE tiles-definitions PUBLIC
       &quot;-//Apache Software Foundation//DTD Tiles Configuration 3.0//EN&quot;
       &quot;http://tiles.apache.org/dtds/tiles-config_3_0.dtd&quot;&gt;

&lt;tiles-definitions&gt;
    &lt;definition name=&quot;memberRegister&quot; extends=&quot;main&quot;&gt; 
        &lt;put-attribute name=&quot;title&quot; value=&quot;회원가입&quot;/&gt;
        &lt;put-attribute name=&quot;body&quot; value=&quot;/WEB-INF/views/member/memberRegister.jsp&quot;/&gt; 
    &lt;/definition&gt;
     &lt;definition name=&quot;memberLogin&quot; extends=&quot;main&quot;&gt; 
        &lt;put-attribute name=&quot;title&quot; value=&quot;회원로그인&quot;/&gt;
        &lt;put-attribute name=&quot;body&quot; value=&quot;/WEB-INF/views/member/memberLogin.jsp&quot;/&gt; 
    &lt;/definition&gt;
&lt;/tiles-definitions&gt;</code></pre><p>로그인 폼을 보여지게 만든다.</p>
<p>MemberController.java</p>
<pre><code>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; ?&gt;

&lt;!DOCTYPE tiles-definitions PUBLIC
       &quot;-//Apache Software Foundation//DTD Tiles Configuration 3.0//EN&quot;
       &quot;http://tiles.apache.org/dtds/tiles-config_3_0.dtd&quot;&gt;

&lt;tiles-definitions&gt;
    &lt;definition name=&quot;memberRegister&quot; extends=&quot;main&quot;&gt; 
        &lt;put-attribute name=&quot;title&quot; value=&quot;회원가입&quot;/&gt;
        &lt;put-attribute name=&quot;body&quot; value=&quot;/WEB-INF/views/member/memberRegister.jsp&quot;/&gt; 
    &lt;/definition&gt;
     &lt;definition name=&quot;memberLogin&quot; extends=&quot;main&quot;&gt; 
        &lt;put-attribute name=&quot;title&quot; value=&quot;회원로그인&quot;/&gt;
        &lt;put-attribute name=&quot;body&quot; value=&quot;/WEB-INF/views/member/memberLogin.jsp&quot;/&gt; 
    &lt;/definition&gt;
&lt;/tiles-definitions&gt;
MemberVo에서 비밀번호 일치여부 체크 메서드를 추가해준다.

package kr.spring.member.vo;

import javax.validation.constraints.Email;
import javax.validation.constraints.NotEmpty;
import javax.validation.constraints.Pattern;
import javax.validation.constraints.Size;

import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

@Getter
@Setter
@ToString
public class MemberVO {
    private int mem_num;
    @Pattern(regexp=&quot;^[A-Za-z0-9]{4,12}$&quot;)
    private String id;
    private String nick_name;
    private int auth;
    private String auto;
    private String au_id;
    @NotEmpty
    private String name;
    @Pattern(regexp=&quot;^[A-Za-z0-9]{4,12}$&quot;)
    private String passwd;
    @NotEmpty
    private String phone;
    @Email
    @NotEmpty
    private String email;
    @Size(min=5,max=5)
    private String zipcode;
    @NotEmpty
    private String address1;
    @NotEmpty
    private String address2;

    //===비밀번호 일치여부 체크===//
    public boolean isCheckedPassword(String userPasswd) {
        if(auth &gt; 1 &amp;&amp; passwd.equals(userPasswd)) {
            return true;
        }
        return false;
    }
}</code></pre><p>나머지는 이전에 작성했던 것 . getter setter는 lombock이 생성해주기때문에 따로 작성하지 않았다.</p>
<p>디시 MemberController.java로 돌아와서 방금 만든 메서드로 로그인 체크, 비밀번호 일치여부 체크를 해준다. </p>
<p>그전에
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/8f8638d0-94fd-4f2b-9bb7-d08cd760a6fa/image.png" />
AuthCheckException.java 클래스 생성</p>
<pre><code>package kr.spring.util;

public class AuthCheckException extends Exception{ //타입이 중요

}</code></pre><p>MemberController.java </p>
<p>로그인 체크 부분 추가하고, session값 받아오기 위해서 메서드에 인자로 session도 추가했당</p>
<p>로그아웃 부분도 추가</p>
<pre><code> /* =======================
     * 회원로그인
     * =======================*/
    //로그인 폼
    @GetMapping(&quot;/member/login.do&quot;)
    public String formLogin() {
        return &quot;memberLogin&quot;;
    }
    //로그인 데이터 처리
    @PostMapping(&quot;/member/login.do&quot;)
    public String submitLogin(@Valid MemberVO memberVO, BindingResult result,HttpSession session) {
        logger.debug(&quot;&lt;&lt;회원로그인&gt;&gt; : &quot; + memberVO);

        //id와 passwd 필드만 유효성 체크. 결과 오류가 있으면 폼 호출
        if(result.hasFieldErrors(&quot;id&quot;) || result.hasFieldErrors(&quot;passwd&quot;)) {
            return formLogin();
        }

        //로그인 체크(id,비밀번호 일치 여부 체크)
        MemberVO member = null;
        try{
            member = memberService.selectCheckMember(memberVO.getId());
            boolean check = false;

            if(member!=null) {
                //비밀번호 일치 여부 체크
                check = member.isCheckedPassword(memberVO.getPasswd());
            }
            if(check) {//인증 성공
                //===자동 로그인 체크 시작===
                //===자동 로그인 체크 끝===

                //인증 성공, 로그인 처리
                session.setAttribute(&quot;user&quot;, member);

                logger.debug(&quot;&lt;&lt;인증 성공&gt;&gt;&quot;);
                logger.debug(&quot;&lt;&lt;id&gt;&gt; : &quot; + member.getId());
                logger.debug(&quot;&lt;&lt;auth&gt;&gt; : &quot; + member.getAuth());
                logger.debug(&quot;&lt;&lt;au_id&gt;&gt; : &quot; + member.getAu_id());


                if(member.getAuth() == 9) {
                    return &quot;redirect:/main/admin.do&quot;;
                }else {
                    return &quot;redirect:/main/main.do&quot;;
                }

            }
            //인증 실패
            throw new AuthCheckException();

        }catch(AuthCheckException e) {
            //인증 실패로 로그인폼 호출
            if(member!=null &amp;&amp; member.getAuth()==1) {
                //정지회원 메세지 표시
                result.reject(&quot;noAuthority&quot;);
            }else {
                result.reject(&quot;invalidIdOrPassword&quot;);
            }

            logger.debug(&quot;&lt;&lt;인증 실패&gt;&gt;&quot;);

            return formLogin();
        }
    }

     /* =======================
     * 로그아웃
     * =======================*/
    @RequestMapping(&quot;/member/logout.do&quot;)
    public String logout(HttpSession session) {
        //로그아웃
        session.invalidate();

        //===자동로그인 해제 시작===

        //===자동로그인 해제 끝 ===

        return &quot;redirect:/main/main.do&quot;;
    }</code></pre><p>정상적으로 로그인이 되고, 아이디&amp;비밀번호 불일치 확인, 로그아웃, 마이페이지 닉네임(닉네임이 없으면 아이디로 보여지기) 역시 잘된다</p>
<p>이제 MyPage 작업!!!</p>
<p>MyPage는 기존 main에서 사용하던 layout과 다른 layout을 사용하려한다
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/aa88c755-f4c3-4d81-9fa4-ac3f21ea46d7/image.png" />
만들어주고요</p>
<p>layout_mypage.jsp</p>
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
&lt;/html&gt;</code></pre><p>mypage_nav.jsp  mypage 메뉴부분
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/4e666a35-ee7a-4057-be75-5c3b23012366/image.png" /></p>
<pre><code>&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=UTF-8&quot;
    pageEncoding=&quot;UTF-8&quot;%&gt;
&lt;%@ taglib prefix=&quot;c&quot; uri=&quot;http://java.sun.com/jsp/jstl/core&quot;%&gt;
&lt;!-- My페이지 메뉴 시작 --&gt;
&lt;div class=&quot;side-bar&quot;&gt;
    &lt;ul&gt;
        &lt;li&gt;
            &lt;img src=&quot;${pageContext.request.contextPath}/member/photoView.do&quot; width=&quot;200&quot; height=&quot;200&quot; class=&quot;my-photo&quot;&gt;
            &lt;div class=&quot;camera&quot; id=&quot;photo_btn&quot;&gt;
                &lt;img src=&quot;${pageContext.request.contextPath}/images/camera.png&quot; width=&quot;35&quot;&gt;
            &lt;/div&gt;
        &lt;/li&gt;
        &lt;li&gt;
            &lt;div id=&quot;photo_choice&quot; style=&quot;display:none;&quot;&gt;
                &lt;input type=&quot;file&quot; id=&quot;upload&quot; accept=&quot;image/gif,image/png,image/jpeg&quot;&gt;&lt;br&gt;
                &lt;input type=&quot;button&quot; value=&quot;전송&quot; id=&quot;photo_submit&quot;&gt;
                &lt;input type=&quot;button&quot; value=&quot;취소&quot; id=&quot;photo_reset&quot;&gt;
            &lt;/div&gt;
        &lt;/li&gt;
    &lt;/ul&gt;
&lt;/div&gt;

&lt;!-- My페이지 메뉴 끝 --&gt;</code></pre><p>img를 blob로 테이블에 저장하기때문에 파일명이 아니라 주소(photoView.do)를 명시해야한다.</p>
<p>아오 근데 pageContext.request.contextPath에서 contextPath를 대문자 c로 써서 오류 났었다.</p>
<p>대소문자 구분 잘하자!
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/cb35bbc7-a74a-49ad-965c-0e7f1e653d84/image.png" />
memberView.jsp도 만들어주고요 이건 Mypage 뷰가 될 것입니다.</p>
<p>내용은 나중에 만들어줄것입니덩</p>
<p>MemberController.java에 return 값만 넘겨주고</p>
<pre><code> /* =======================
     * My페이지
     * =======================*/
    @RequestMapping(&quot;/member/myPage.do&quot;)
    public String myPage() {
        return &quot;myPage&quot;; 
    }</code></pre><p>member.xml</p>
<pre><code>&lt;!-- myPage --&gt;
    &lt;definition name=&quot;myPage&quot; template=&quot;/WEB-INF/views/template/layout_mypage.jsp&quot;&gt;
        &lt;put-attribute name=&quot;title&quot; value=&quot;MyPage&quot;/&gt;
        &lt;put-attribute name=&quot;header&quot; value=&quot;/WEB-INF/views/template/header.jsp&quot;/&gt;
        &lt;put-attribute name=&quot;nav&quot; value=&quot;/WEB-INF/views/template/mypage_nav.jsp&quot;/&gt;
        &lt;put-attribute name=&quot;body&quot; value=&quot;/WEB-INF/views/member/memberView.jsp&quot;/&gt;
        &lt;put-attribute name=&quot;footer&quot; value=&quot;/WEB-INF/views/template/footer.jsp&quot;/&gt;
    &lt;/definition&gt;</code></pre><p>layout.css  관리자 페이지&amp;mypage부분</p>
<pre><code> /* Admin, My페이지 공통
----------------------------------*/               
.side-bar{
    margin:10px 10px 10px 0;
    padding:10px;
    /*사이드바의 높이가 컨텐트의 높이와 동일하게 자동 조절*/
    margin-bottom:-9999px;
    padding-bottom:9999px;
    background-color:#cecccc;
}
.side-bar ul{
    margin:0;
    padding:0;
}             
.side-bar ul li{
    padding:5px 10px 5px 10px;
}
.size-bar ul li img{
    margin-left:3px;
}
.side-height{
    /*사이드바의 높이가 컨텐트의 높이와 동일하게 자동 조절하기 위해 흘러넘친 배경색은 숨김*/
    overflow:hidden;
    margin-bottom:10px;
}
.menu-btn{
    width:100%;
    height:50px;
    font-size:12pt;
    background-color:#fcfcfc;
    border-color:#b7b5b5;
    border-radius:5px;
}
 /* My페이지
----------------------------------*/                 
.camera{
    width:50px;
    height:50px;
    border-radius:100%;
    background-color:#d3bb72;
    position:relative;
    top:-60px;
    left:140px;
    border:2px solid #fff;
    cursor:pointer;
    padding:13px 0 0 10px;
}                
.my-photo{
    object-fit:cover;
    object-position:top;
    border-radius:50%;
}   
/* 좌측 메뉴 레이아웃(Admin,MyPage)
----------------------------------*/
#page_nav{
    float:left;
    width:25%;
    min-height:530px;
}
#page_body{
    width:75%;
    float:left;
}
.page_clear{
    clear:both;
}</code></pre><p>src/main/resources &gt; static &gt; images 폴더 만들주고 필요한 이미지(face.png, camera.png) 넣어주기</p>
<p>MemberMapper.java</p>
<pre><code>//회원번호를 이용한 회원정보 구하기
    @Select(&quot;SELECT * FROM spmember m 
            JOIN spmember_detail d ON m.mem_num=d.mem_num &quot;
            + &quot;WHERE m.mem_num=#{mem_num}&quot;) 
    public MemberVO selectMember(Integer mem_num); 
    //int라고 해도 됨 //여기 mem_num이 위의 #{mem_num}임</code></pre><p>MemberServiceImpl.java</p>
<pre><code>    @Override
    public MemberVO selectMember(Integer mem_num) {
        return memberMapper.selectMember(mem_num);
    }</code></pre><p>kr.spring.util &gt; FileUtil.java 넣어준다</p>
<pre><code>package kr.spring.util;

import java.io.FileInputStream;
import java.io.IOException;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class FileUtil {
    private static final Logger logger = LoggerFactory.getLogger(FileUtil.class);
    public static byte[] getBytes(String path) {
        FileInputStream fis = null;
        byte[] readbyte = null;
        try {
            fis = new FileInputStream(path);
            readbyte = new byte[fis.available()]; 
            fis.read(readbyte);
        } catch (Exception e) {
            logger.error(e.toString());
        }finally {
            if(fis!=null)try {fis.close();}catch(IOException e) {}
        }
        return readbyte;
    }
}</code></pre><p>바이트배열 ..어..음 ..네</p>
<p>MemberVO.java</p>
<pre><code>private byte[] photo; 
    private String photo_name;
    private Date reg_date;
    private Date modify_date;</code></pre><p>추가하고</p>
<p>src &gt; main &gt; webapp &gt; image_bundle 폴더 만들어서 이미지(face) 넣어줘</p>
<p>src/main/java &gt; kr.spring.view 패키지 만들고 imageView 넣깅</p>
<pre><code>package kr.spring.view;

import java.io.ByteArrayInputStream;
import java.io.InputStream;
import java.io.OutputStream;
import java.util.Map;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.commons.io.IOUtils;
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.view.AbstractView;

@Component
public class ImageView extends AbstractView {

    @Override
    protected void renderMergedOutputModel(Map&lt;String, Object&gt; model,
            HttpServletRequest request, HttpServletResponse response)
                    throws Exception {

        byte[] file = (byte[]) model.get(&quot;imageFile&quot;);
        String filename = (String) model.get(&quot;filename&quot;);

        String ext = filename.substring(filename.lastIndexOf(&quot;.&quot;));
        if(ext.equalsIgnoreCase(&quot;.gif&quot;)) {
            ext = &quot;image/gif&quot;;
        }else if(ext.equalsIgnoreCase(&quot;.png&quot;)) {
            ext = &quot;image/png&quot;;
        }else {
            ext = &quot;image/jpeg&quot;;
        }

        response.setContentType(ext);
        response.setContentLength(file.length);

        String fileName = new String(
                filename.getBytes(&quot;utf-8&quot;),&quot;iso-8859-1&quot;);    

        response.setHeader(&quot;Content-Disposition&quot;, 
                &quot;attachment; filename=\&quot;&quot; + fileName + &quot;\&quot;;&quot;);
        response.setHeader(&quot;Content-Transfer-Encoding&quot;, &quot;binary&quot;);

        OutputStream out = response.getOutputStream();
        InputStream input = null;
        try {
            input = new ByteArrayInputStream(file);
            IOUtils.copy(input, out);
            out.flush();
        }finally {
            if(out!=null) out.close();
            if(input!=null) input.close();
        }
    }

}</code></pre><p>선생님이 준 파일인데요 .. 음 ?</p>
<p>byte[]을 읽어와서 stream처리??0? &lt;-- 알아와. 무슨 말인지. ㅠㅠ엉엉</p>
<p>MemberController.java</p>
<pre><code>/* =======================
     *    프로필 사진 출력
     * =======================*/
    //프로필 사진 출력(로그인 전용)
    @RequestMapping(&quot;/member/photoView.do&quot;)
    public String getProfile(HttpSession session,HttpServletRequest request,Model model) {
        MemberVO user = (MemberVO)session.getAttribute(&quot;user&quot;);
        logger.debug(&quot;&lt;&lt;photoView&gt;&gt; : &quot; + user);

        if(user==null) {//로그인 안 된 경우
            //기본이미지 불러오기(업로드한 프로필 사진이 없는 경우)
            byte[] readbyte = FileUtil.getBytes(request.getServletContext().getRealPath(&quot;/image_bundle/face.png&quot;));
            model.addAttribute(&quot;imageFile&quot;,readbyte);
            model.addAttribute(&quot;filename&quot;,&quot;face.png&quot;);            
        }else {//로그인 된 경우
            MemberVO memberVO = memberService.selectMember(user.getMem_num());
            viewProfile(memberVO,request,model);
        }

        return &quot;imageView&quot;;
    }

    //프로필 사진 처리를 위한 공통 코드
    public void viewProfile(MemberVO memberVO,HttpServletRequest request,Model model) {
        if(memberVO==null || memberVO.getPhoto_name()==null) {
            //기본이미지 불러오기(업로드한 프로필 사진이 없는 경우)
            byte[] readbyte = FileUtil.getBytes(request.getServletContext().getRealPath(&quot;/image_bundle/face.png&quot;));
            model.addAttribute(&quot;imageFile&quot;,readbyte);
            model.addAttribute(&quot;filename&quot;,&quot;face.png&quot;);
        }else {//업로드한 프로필 사진이 있는 경우
            model.addAttribute(&quot;imageFile&quot;,memberVO.getPhoto());
            model.addAttribute(&quot;filename&quot;,memberVO.getPhoto_name());
        }
    }</code></pre><p>하면 my페이지에서 기본이미지의 프로필 사진이 보인다.</p>
<p>아오 점심먹고 오니까 집중력 개 없어 하기싫다 증말 ^^!! byte배열을 stream인지 string인지 바꾼다할때부터 네?????임 .</p>
<p>ㅋㅋ</p>
<p>집가고싶어요..</p>
<p>희재랑 민서랑 희진언니 따라서 블로그에다 공부 좀 할라했더만</p>
<p>쉽^^찌^^않^^ㅇ ㅏ</p>
<p>하지만~</p>
<p>나는 할수이서<del>~</del>! </p>
<p>화이링링♥</p>
<p>오류 고치기 타임 끝났다 ㅡㅡ 다시 달려 흑 </p>
<p>header.jsp</p>
<pre><code>&lt;c:if test=&quot;${!empty user}&quot;&gt;
   &lt;img src=&quot;${pageContext.request.contextPath}/member/photoView.do&quot; width=&quot;25&quot; height=&quot;25&quot; class=&quot;my-photo&quot;&gt;
&lt;/c:if&gt;</code></pre><p>헤더에도 프로필 사진이 보인다 </p>
<p>어? 끝났다 </p>
<p>ㅎㅎ</p>
<p>안녕</p>