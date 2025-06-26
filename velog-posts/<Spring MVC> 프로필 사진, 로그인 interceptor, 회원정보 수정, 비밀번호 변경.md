<p>MemberMapper.java </p>
<p>받은 프로필 사진을 바이트 배열로 바꿔줘야해</p>
<pre><code>    //프로필 이미지 업데이트
    @Update(&quot;UPDATE spmember_detail SET photo=#{photo},photo_name=#{photo_name} WHERE mem_num=#{mem_num}&quot;)
    public void updateProfile(MemberVO member);</code></pre><p>photo_name는 확장성을 위해서 넣은거 생략도 가능하다!</p>
<p>어노테이션 지우고 MemberService.java에도 똑같이 추가해줘 그리고</p>
<p>MemberServiceImpl.java</p>
<pre><code>    @Override
    public void updateProfile(MemberVO member) {
        memberMapper.updateProfile(member);

    }</code></pre><p>MemberController.java</p>
<pre><code>     /* =======================
     *    프로필 사진 업데이트
     * =======================*/
    @RequestMapping(&quot;/member/updateMyPhoto.do&quot;)
    @ResponseBody
    public Map&lt;String,String&gt; updateProfile(MemberVO memberVO,HttpSession session){
        Map&lt;String,String&gt; mapAjax = new HashMap&lt;String,String&gt;();

        MemberVO user = (MemberVO)session.getAttribute(&quot;user&quot;);
        if(user==null) {
            mapAjax.put(&quot;result&quot;, &quot;logout&quot;);
        }else {
            memberVO.setMem_num(user.getMem_num());
            memberService.updateProfile(memberVO);

            mapAjax.put(&quot;result&quot;, &quot;success&quot;);
        }

        return mapAjax;
    }</code></pre><p>MemberVO.java</p>
<p>자바빈에서 파일을 바이트배열로 변환하는 작업을 해줘야해 그래야 컨트롤러의 코드를 줄일 수 있어</p>
<p>바이트배열 -&gt; blob 변환은 mybatis가 해줌!</p>
<pre><code>    //====이미지를 byte[]로 변환(BLOB 처리)====//
    //(!!주의) 폼에서 파일 업로드 파라미터네임은 반드시 upload로 지정해야 함. photo 아니야!!!!!(타입 안맞아서)
    public void setUpload(MultipartFile upload) throws IOException{
        //MultipartFile -&gt; byte[]
        setPhoto(upload.getBytes());
        //파일 이름
        setPhoto_name(upload.getOriginalFilename());
    }</code></pre><p>src/main/resources &gt; static &gt; js &gt; profile.js 를 생성</p>
<p>open with 해서 generic text editor로 열어</p>
<p>profile.js</p>
<pre><code>$(function(){
    //MyPage 프로필 사진 등록 및 수정
    //수정 버튼 이벤트 처리
    $('#photo_btn').click(function(){
        $('#photo_choice').show();
        $(this).hide();
    });
});</code></pre><p>mypage_nav.jsp에 링크 걸고 테스트 후 마저 구현</p>
<pre><code>&lt;script type=&quot;text/javascript&quot; src=&quot;${pageContext.request.contextPath}/js/profile.js&quot;&gt;&lt;/script&gt;</code></pre><p>profile.js</p>
<pre><code>$(function(){
    //MyPage 프로필 사진 등록 및 수정
    //수정 버튼 이벤트 처리
    $('#photo_btn').click(function(){
        $('#photo_choice').show();
        $(this).hide();
    });

    //처음 화면에 보여지는 이미지 읽기 
    let photo_path = $('.my-photo').attr('src'); //처음에 보여지는 이미지
    let my_photo; //업로드하고자 선택한 이미지 저장
    $('#upload').change(function(){
        my_photo = this.files[0];
        if(!my_photo){ //선택했다가 취소하는경우. myphoto가 없는 경우
            $('.my-photo').attr('src',photo_path);
            return;
        }

        if(my_photo.size &gt; 1024 * 1024){
            alert(Math.round(my_photo.size/1024) + 'kbytes(1024kbytes까지만 업로드 가능)');
            $('.my-photo').attr('src',photo_path);
            $(this).val('');
            return;
        }

        //이미지 미리보기 처리
        let reader = new FileReader();
        reader.readAsDataURL(my_photo);

        reader.onload=function(){
            $('.my-photo').attr('src',reader.result);
        };

    });


    //파일 업로드 처리
    $('#photo_submit').click(function(){
        if($('#upload').val()==''){ //photo아니고 upload!!!
            alert('파일을 선택하세요');
            $('#upload').focus();
            return; //submit버튼이 아니여서 return 해야해
        }

        //서버에 파일 전송
        let form_data = new FormData();
        form_data.append('upload',/*$('#upload')[0].files[0] 변수에 담겼으니 이름 변경*/my_photo);
        $.ajax({
            url:'../member/updateMyPhoto.do',
            type:'post',
            data: form_data,
            dataType:'json',
            contentType:false,
            processData:false,
            success:function(param){
                if(param.result == 'logout'){
                    alert('로그인 후 사용하세요');
                }else if(param.result == 'success'){
                    alert('프로필 사진이 수정되었습니다.');
                    //교체된 이미지 저장
                    photo_path = $('.my-photo').attr('src');
                    $('#upload').val('');
                    $('#photo_choice').hide();
                    $('#photo_btn').show();
                }
            },
            error:function(){
                alert('네트워크 오류 발생');
            }
        });

    });//end of click

    //취소 버튼 처리
    $('#photo_reset').click(function(){
        $('.my-photo').attr('src',photo_path);
        $('#upload').val('');
        $('#photo_choice').hide();
        $('#photo_btn').show();
    });

});</code></pre><p>mypage_nav.jsp</p>
<pre><code>    &lt;ul&gt;
        &lt;li&gt;
            &lt;input type=&quot;button&quot; class=&quot;menu-btn&quot; value=&quot;비밀번호변경&quot; 
            onclick=&quot;location.href='${pageContext.request.contextPath}/member/changePassword.do'&quot;&gt;
        &lt;/li&gt;
        &lt;li&gt;
            &lt;input type=&quot;button&quot; class=&quot;menu-btn&quot; value=&quot;채팅&quot; 
            onclick=&quot;location.href='${pageContext.request.contextPath}/talk/talkList.do'&quot;&gt;
        &lt;/li&gt;
        &lt;li&gt;
            &lt;input type=&quot;button&quot; class=&quot;menu-btn&quot; value=&quot;회원탈퇴&quot; 
            onclick=&quot;location.href='${pageContext.request.contextPath}/member/delete.do'&quot;&gt;
        &lt;/li&gt;
    &lt;/ul&gt;</code></pre><p>ul을 추가해줍니덩</p>
<p>MemberController.java</p>
<pre><code>     /* =======================
     * My페이지
     * =======================*/
    @RequestMapping(&quot;/member/myPage.do&quot;)
    public String myPage(HttpSession session, Model model) {

        MemberVO user = (MemberVO)session.getAttribute(&quot;user&quot;);
        //회원 정보 반환
        MemberVO member = memberService.selectMember(user.getMem_num());

        //회원 정보 
        model.addAttribute(&quot;member&quot;,member); 


        return &quot;myPage&quot;; 
    }</code></pre><p>MemberView.jsp</p>
<pre><code>&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=UTF-8&quot;
    pageEncoding=&quot;UTF-8&quot;%&gt;
&lt;%@ taglib prefix=&quot;c&quot; uri=&quot;http://java.sun.com/jsp/jstl/core&quot; %&gt;
&lt;!-- My페이지 시작 --&gt;
&lt;div class=&quot;page-main&quot;&gt;
    &lt;h2&gt;회원상세정보&lt;input type=&quot;button&quot; value=&quot;회원정보수정&quot; onclick=&quot;location.href='update.do'&quot;&gt;&lt;/h2&gt;
    &lt;ul&gt;
        &lt;li&gt;이름 : ${member.name}&lt;/li&gt;
        &lt;li&gt;별명 : ${member.nick_name}&lt;/li&gt;
        &lt;li&gt;전화번호 : ${member.phone}&lt;/li&gt;
        &lt;li&gt;이메일 : ${member.email}&lt;/li&gt;
        &lt;li&gt;우편번호 : ${member.zipcode}&lt;/li&gt;
        &lt;li&gt;주소 : ${member.address1} ${member.address2}&lt;/li&gt;
        &lt;li&gt;가입날짜 : ${member.reg_date}&lt;/li&gt;
        &lt;c:if test=&quot;${!empty member.modify_date}&quot;&gt;
        &lt;li&gt;정보 수정일 : ${member.modify_date}&lt;/li&gt;
        &lt;/c:if&gt; 
    &lt;/ul&gt;
&lt;/div&gt;
&lt;!-- My페이지 끝 --&gt;</code></pre><p>로그인이 안된 상태에서 로그인이 필요로하는 페이지에 들어갔을때, 로그인이 되게끔 구현하는 방법</p>
<p>kr.spring.interceptor &lt; 패키지 만들고 </p>
<p>LoginCheckInterceptor &lt; class 만들어준다</p>
<p>LoginCheckInterceptor.java</p>
<pre><code>package kr.spring.interceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.servlet.HandlerInterceptor;

public class LoginCheckInterceptor implements HandlerInterceptor{
    private static final Logger logger = LoggerFactory.getLogger(LoginCheckInterceptor.class);

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response,Object handler)
                                                                throws Exception{ //object는 요청하는 컨트롤러
        logger.debug(&quot;&lt;&lt;LoginCheckInterceptor 진입&gt;&gt;&quot;);

        HttpSession session = request.getSession();
        //로그인 여부 검사
        if(session.getAttribute(&quot;user&quot;)==null) {
            //로그인이 되지 않은 상태
            response.sendRedirect(request.getContextPath()+&quot;/member/login.do&quot;);
            return false; //로그인 안된경우 요청된 페이지가 보여지지 않게끔!
        }
        return true; //로그인 된 경우 요청된 페이지가 수행
    }
}</code></pre><p>AppConfig.java</p>
<p>만든 interceptor 등록</p>
<pre><code>package kr.spring.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.view.tiles3.TilesConfigurer;
import org.springframework.web.servlet.view.tiles3.TilesView;
import org.springframework.web.servlet.view.tiles3.TilesViewResolver;

import kr.spring.interceptor.LoginCheckInterceptor;

//자바 코드 기반 설정 클래스
@Configuration
public class AppConfig implements WebMvcConfigurer{
    private LoginCheckInterceptor loginCheck;

    @Bean
    public LoginCheckInterceptor interceptor2() {
        loginCheck = new LoginCheckInterceptor();
        return loginCheck;
    }

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(loginCheck).addPathPatterns(&quot;/member/myPage.do&quot;);
//모든 요청에 interceptor가 동작되지 않고 요청한 주소에서만 동작됨. myPage.do를 요청했을때만!!
    }


    @Bean
    public TilesConfigurer tilesConfigurer() { 
        //TilesConfigurer 빈은 타일 정의의 위치를 정하고, 불러오고, 일반적으로 타일들을 배치해 주는 역할을 한다. 
        final TilesConfigurer configurer = new TilesConfigurer();
        //해당 경로에 xml 설정 파일을 넣음
        configurer.setDefinitions(new String[] {
                &quot;/WEB-INF/tiles-def/main.xml&quot;,
                &quot;/WEB-INF/tiles-def/member.xml&quot;
        });
        return configurer;
    }
    @Bean
    public TilesViewResolver tilesViewResolver() {
        //TilesViewResolver 빈은 논리적 뷰 이름으로 타일 정의를 결정한다.
        final TilesViewResolver tilesViewResolver = new TilesViewResolver();
        tilesViewResolver.setViewClass(TilesView.class);
        return tilesViewResolver;

    }
}</code></pre><p>그럼 로그아웃 상태에서 로그인이 필요한 페이지를 직접 호출했을때, 에러가 나지 않고 로그인 페이지로 넘어간다.</p>
<p>MemberController.java</p>
<pre><code> /* =======================
     *    회원정보수정
     * =======================*/
    //수정 폼 호출
    @GetMapping(&quot;/member/update.do&quot;)
    public String formUpdate(HttpSession session,Model model) {
        MemberVO user = (MemberVO)session.getAttribute(&quot;user&quot;);

        MemberVO memberVO = memberService.selectMember(user.getMem_num());
        model.addAttribute(&quot;memberVO&quot;, memberVO);

        return &quot;memberModify&quot;;
    }</code></pre><p>webapp&gt; WEB-INF&gt; views&gt; member&gt; memberModify.jsp만들어주기</p>
<p>memberModify.jsp</p>
<pre><code>&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=UTF-8&quot;
    pageEncoding=&quot;UTF-8&quot;%&gt;
&lt;!-- layout에 쏙 들어가는 구조라서 밑의 html코드는 필요없성 --&gt;
&lt;%@ taglib prefix=&quot;form&quot; uri=&quot;http://www.springframework.org/tags/form&quot;%&gt;
&lt;!-- 회원정보수정 시작 --&gt;
&lt;div class=&quot;page-main&quot;&gt;
    &lt;h2&gt;회원정보수정&lt;/h2&gt;
    &lt;form:form modelAttribute=&quot;memberVO&quot; action=&quot;update.do&quot; id=&quot;member_modify&quot;&gt;
        &lt;form:errors element=&quot;div&quot; cssClass=&quot;error-color&quot;/&gt;
        &lt;ul&gt;
            &lt;li&gt;
                &lt;form:label path=&quot;name&quot;&gt;이름&lt;/form:label&gt;
                &lt;form:input path=&quot;name&quot;/&gt;
                &lt;form:errors path=&quot;name&quot; cssClass=&quot;error-color&quot;/&gt;
            &lt;/li&gt;
            &lt;li&gt;
                &lt;form:label path=&quot;nick_name&quot;&gt;별명&lt;/form:label&gt;
                &lt;form:input path=&quot;nick_name&quot;/&gt;
            &lt;/li&gt;
            &lt;li&gt;
                &lt;form:label path=&quot;phone&quot;&gt;전화번호&lt;/form:label&gt;
                &lt;form:input path=&quot;phone&quot;/&gt;
                &lt;form:errors path=&quot;phone&quot; cssClass=&quot;error-color&quot;/&gt;
            &lt;/li&gt;
            &lt;li&gt;
                &lt;form:label path=&quot;email&quot;&gt;이메일&lt;/form:label&gt;
                &lt;form:input path=&quot;email&quot;/&gt;
                &lt;form:errors path=&quot;email&quot; cssClass=&quot;error-color&quot;/&gt;
            &lt;/li&gt;
            &lt;li&gt;
                &lt;form:label path=&quot;zipcode&quot;&gt;우편번호&lt;/form:label&gt;
                &lt;form:input path=&quot;zipcode&quot;/&gt;
                &lt;input type=&quot;button&quot; onclick=&quot;execDaumPostcode()&quot; value=&quot;우편번호 찾기&quot; class=&quot;default-btn&quot;&gt;
                &lt;form:errors path=&quot;zipcode&quot; cssClass=&quot;error-color&quot;/&gt;
            &lt;/li&gt;
            &lt;li&gt;
                &lt;form:label path=&quot;address1&quot;&gt;주소&lt;/form:label&gt;
                &lt;form:input path=&quot;address1&quot;/&gt;
                &lt;form:errors path=&quot;address1&quot; cssClass=&quot;error-color&quot;/&gt;
            &lt;/li&gt;
            &lt;li&gt;
                &lt;form:label path=&quot;address2&quot;&gt;상세주소&lt;/form:label&gt;
                &lt;form:input path=&quot;address2&quot;/&gt;
                &lt;form:errors path=&quot;address2&quot; cssClass=&quot;error-color&quot;/&gt;
            &lt;/li&gt;
        &lt;/ul&gt;
        &lt;div class=&quot;align-center&quot;&gt;
            &lt;form:button class=&quot;default-btn&quot;&gt;수정&lt;/form:button&gt;
            &lt;input type=&quot;button&quot; value=&quot;홈으로&quot; class=&quot;default-btn&quot; 
            onclick=&quot;location.href='${pageContext.request.contextPath}/main/main.do'&quot;&gt;
        &lt;/div&gt;
    &lt;/form:form&gt;
&lt;/div&gt;

&lt;!-- 회원정보수정 끝 --&gt;</code></pre><p>밑에 우편번호 api도 있는데 길어서 잘랐다. 똑같애~ register랑</p>
<p>member.xml</p>
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
    &lt;definition name=&quot;memberModify&quot; extends=&quot;myPage&quot;&gt; 
        &lt;put-attribute name=&quot;title&quot; value=&quot;회원정보수정&quot;/&gt;
        &lt;put-attribute name=&quot;body&quot; value=&quot;/WEB-INF/views/member/memberModify.jsp&quot;/&gt; 
    &lt;/definition&gt;
    &lt;!-- myPage --&gt;
    &lt;definition name=&quot;myPage&quot; template=&quot;/WEB-INF/views/template/layout_mypage.jsp&quot;&gt;
        &lt;put-attribute name=&quot;title&quot; value=&quot;MyPage&quot;/&gt;
        &lt;put-attribute name=&quot;header&quot; value=&quot;/WEB-INF/views/template/header.jsp&quot;/&gt;
        &lt;put-attribute name=&quot;nav&quot; value=&quot;/WEB-INF/views/template/mypage_nav.jsp&quot;/&gt;
        &lt;put-attribute name=&quot;body&quot; value=&quot;/WEB-INF/views/member/memberView.jsp&quot;/&gt;
        &lt;put-attribute name=&quot;footer&quot; value=&quot;/WEB-INF/views/template/footer.jsp&quot;/&gt;
    &lt;/definition&gt;

&lt;/tiles-definitions&gt;</code></pre><p>회원정보수정부분 추가</p>
<p>MemberMapper.java</p>
<pre><code>    //회원정보 수정
    @Update(&quot;UPDATE spmember SET nick_name=#{nick_name} WHERE mem_num=#{mem_num}&quot;)
    public void updateMember(MemberVO member);
    public void updateMember_detail(MemberVO member);</code></pre><p>짧은것만 어노테이션으로 명시</p>
<p>긴거는 xml에서 ~</p>
<p>MemberMapper.xml</p>
<pre><code>&lt;!-- 회원정보수정 --&gt;
    &lt;update id=&quot;updateMember_detail&quot; parameterType=&quot;memberVO&quot;&gt;
        UPDATE
            spmember_detail
        SET
            name=#{name},
            phone=#{phone},
            email=#{email},
            zipcode=#{zipcode},
            address1=#{address1},
            address2=#{address2},
            modify_date=SYSDATE
        WHERE 
            mem_num=#{mem_num}
    &lt;/update&gt;</code></pre><p>MemberMapper.java </p>
<p>받은 프로필 사진을 바이트 배열로 바꿔줘야해</p>
<pre><code>//프로필 이미지 업데이트
@Update(&quot;UPDATE spmember_detail SET photo=#{photo},photo_name=#{photo_name} WHERE mem_num=#{mem_num}&quot;)
public void updateProfile(MemberVO member);</code></pre><p>photo_name는 확장성을 위해서 넣은거 생략도 가능하다!</p>
<p>어노테이션 지우고 MemberService.java에도 똑같이 추가해줘 그리고</p>
<p>MemberServiceImpl.java</p>
<pre><code>@Override
public void updateProfile(MemberVO member) {
    memberMapper.updateProfile(member);

}</code></pre><p>MemberController.java</p>
<pre><code> /* =======================
 *    프로필 사진 업데이트
 * =======================*/
@RequestMapping(&quot;/member/updateMyPhoto.do&quot;)
@ResponseBody
public Map&lt;String,String&gt; updateProfile(MemberVO memberVO,HttpSession session){
    Map&lt;String,String&gt; mapAjax = new HashMap&lt;String,String&gt;();

    MemberVO user = (MemberVO)session.getAttribute(&quot;user&quot;);
    if(user==null) {
        mapAjax.put(&quot;result&quot;, &quot;logout&quot;);
    }else {
        memberVO.setMem_num(user.getMem_num());
        memberService.updateProfile(memberVO);

        mapAjax.put(&quot;result&quot;, &quot;success&quot;);
    }

    return mapAjax;
}</code></pre><p>MemberVO.java</p>
<p>자바빈에서 파일을 바이트배열로 변환하는 작업을 해줘야해 그래야 컨트롤러의 코드를 줄일 수 있어</p>
<p>바이트배열 -&gt; blob 변환은 mybatis가 해줌!</p>
<pre><code>//====이미지를 byte[]로 변환(BLOB 처리)====//
//(!!주의) 폼에서 파일 업로드 파라미터네임은 반드시 upload로 지정해야 함. photo 아니야!!!!!(타입 안맞아서)
public void setUpload(MultipartFile upload) throws IOException{
    //MultipartFile -&gt; byte[]
    setPhoto(upload.getBytes());
    //파일 이름
    setPhoto_name(upload.getOriginalFilename());
}</code></pre><p>src/main/resources &gt; static &gt; js &gt; profile.js 를 생성</p>
<p>open with 해서 generic text editor로 열어</p>
<p>profile.js</p>
<p>$(function(){
    //MyPage 프로필 사진 등록 및 수정
    //수정 버튼 이벤트 처리
    $('#photo_btn').click(function(){
        $('#photo_choice').show();
        $(this).hide();
    });
});
mypage_nav.jsp에 링크 걸고 테스트 후 마저 구현</p>

<p>profile.js</p>
<p>$(function(){
    //MyPage 프로필 사진 등록 및 수정
    //수정 버튼 이벤트 처리
    $('#photo_btn').click(function(){
        $('#photo_choice').show();
        $(this).hide();
    });</p>
<pre><code>//처음 화면에 보여지는 이미지 읽기 
let photo_path = $('.my-photo').attr('src'); //처음에 보여지는 이미지
let my_photo; //업로드하고자 선택한 이미지 저장
$('#upload').change(function(){
    my_photo = this.files[0];
    if(!my_photo){ //선택했다가 취소하는경우. myphoto가 없는 경우
        $('.my-photo').attr('src',photo_path);
        return;
    }

    if(my_photo.size &gt; 1024 * 1024){
        alert(Math.round(my_photo.size/1024) + 'kbytes(1024kbytes까지만 업로드 가능)');
        $('.my-photo').attr('src',photo_path);
        $(this).val('');
        return;
    }

    //이미지 미리보기 처리
    let reader = new FileReader();
    reader.readAsDataURL(my_photo);

    reader.onload=function(){
        $('.my-photo').attr('src',reader.result);
    };

});


//파일 업로드 처리
$('#photo_submit').click(function(){
    if($('#upload').val()==''){ //photo아니고 upload!!!
        alert('파일을 선택하세요');
        $('#upload').focus();
        return; //submit버튼이 아니여서 return 해야해
    }

    //서버에 파일 전송
    let form_data = new FormData();
    form_data.append('upload',/*$('#upload')[0].files[0] 변수에 담겼으니 이름 변경*/my_photo);
    $.ajax({
        url:'../member/updateMyPhoto.do',
        type:'post',
        data: form_data,
        dataType:'json',
        contentType:false,
        processData:false,
        success:function(param){
            if(param.result == 'logout'){
                alert('로그인 후 사용하세요');
            }else if(param.result == 'success'){
                alert('프로필 사진이 수정되었습니다.');
                //교체된 이미지 저장
                photo_path = $('.my-photo').attr('src');
                $('#upload').val('');
                $('#photo_choice').hide();
                $('#photo_btn').show();
            }
        },
        error:function(){
            alert('네트워크 오류 발생');
        }
    });

});//end of click

//취소 버튼 처리
$('#photo_reset').click(function(){
    $('.my-photo').attr('src',photo_path);
    $('#upload').val('');
    $('#photo_choice').hide();
    $('#photo_btn').show();
});</code></pre><p>});</p>
<p>mypage_nav.jsp</p>
<pre><code>&lt;ul&gt;
    &lt;li&gt;
        &lt;input type=&quot;button&quot; class=&quot;menu-btn&quot; value=&quot;비밀번호변경&quot; 
        onclick=&quot;location.href='${pageContext.request.contextPath}/member/changePassword.do'&quot;&gt;
    &lt;/li&gt;
    &lt;li&gt;
        &lt;input type=&quot;button&quot; class=&quot;menu-btn&quot; value=&quot;채팅&quot; 
        onclick=&quot;location.href='${pageContext.request.contextPath}/talk/talkList.do'&quot;&gt;
    &lt;/li&gt;
    &lt;li&gt;
        &lt;input type=&quot;button&quot; class=&quot;menu-btn&quot; value=&quot;회원탈퇴&quot; 
        onclick=&quot;location.href='${pageContext.request.contextPath}/member/delete.do'&quot;&gt;
    &lt;/li&gt;
&lt;/ul&gt;</code></pre><p>ul을 추가해줍니덩</p>
<p>MemberController.java</p>
<pre><code> /* =======================
 * My페이지
 * =======================*/
@RequestMapping(&quot;/member/myPage.do&quot;)
public String myPage(HttpSession session, Model model) {

    MemberVO user = (MemberVO)session.getAttribute(&quot;user&quot;);
    //회원 정보 반환
    MemberVO member = memberService.selectMember(user.getMem_num());

    //회원 정보 
    model.addAttribute(&quot;member&quot;,member); 


    return &quot;myPage&quot;; 
}</code></pre><p>MemberView.jsp</p>
<pre><code>&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=UTF-8&quot;
    pageEncoding=&quot;UTF-8&quot;%&gt;
&lt;%@ taglib prefix=&quot;c&quot; uri=&quot;http://java.sun.com/jsp/jstl/core&quot; %&gt;
&lt;!-- My페이지 시작 --&gt;
&lt;div class=&quot;page-main&quot;&gt;
    &lt;h2&gt;회원상세정보&lt;input type=&quot;button&quot; value=&quot;회원정보수정&quot; onclick=&quot;location.href='update.do'&quot;&gt;&lt;/h2&gt;
    &lt;ul&gt;
        &lt;li&gt;이름 : ${member.name}&lt;/li&gt;
        &lt;li&gt;별명 : ${member.nick_name}&lt;/li&gt;
        &lt;li&gt;전화번호 : ${member.phone}&lt;/li&gt;
        &lt;li&gt;이메일 : ${member.email}&lt;/li&gt;
        &lt;li&gt;우편번호 : ${member.zipcode}&lt;/li&gt;
        &lt;li&gt;주소 : ${member.address1} ${member.address2}&lt;/li&gt;
        &lt;li&gt;가입날짜 : ${member.reg_date}&lt;/li&gt;
        &lt;c:if test=&quot;${!empty member.modify_date}&quot;&gt;
        &lt;li&gt;정보 수정일 : ${member.modify_date}&lt;/li&gt;
        &lt;/c:if&gt; 
    &lt;/ul&gt;
&lt;/div&gt;
&lt;!-- My페이지 끝 --&gt;</code></pre><p>로그인이 안된 상태에서 로그인이 필요로하는 페이지에 들어갔을때, 로그인이 되게끔 구현하는 방법</p>
<p>kr.spring.interceptor &lt; 패키지 만들고 </p>
<p>LoginCheckInterceptor &lt; class 만들어쥰당
LoginCheckInterceptor.java</p>
<pre><code>package kr.spring.interceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.servlet.HandlerInterceptor;

public class LoginCheckInterceptor implements HandlerInterceptor{
    private static final Logger logger = LoggerFactory.getLogger(LoginCheckInterceptor.class);

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response,Object handler)
                                                                throws Exception{ //object는 요청하는 컨트롤러
        logger.debug(&quot;&lt;&lt;LoginCheckInterceptor 진입&gt;&gt;&quot;);

        HttpSession session = request.getSession();
        //로그인 여부 검사
        if(session.getAttribute(&quot;user&quot;)==null) {
            //로그인이 되지 않은 상태
            response.sendRedirect(request.getContextPath()+&quot;/member/login.do&quot;);
            return false; //로그인 안된경우 요청된 페이지가 보여지지 않게끔!
        }
        return true; //로그인 된 경우 요청된 페이지가 수행
    }
}</code></pre><p>AppConfig.java</p>
<p>만든 interceptor 등록</p>
<pre><code>package kr.spring.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.view.tiles3.TilesConfigurer;
import org.springframework.web.servlet.view.tiles3.TilesView;
import org.springframework.web.servlet.view.tiles3.TilesViewResolver;

import kr.spring.interceptor.LoginCheckInterceptor;

//자바 코드 기반 설정 클래스
@Configuration
public class AppConfig implements WebMvcConfigurer{
    private LoginCheckInterceptor loginCheck;

    @Bean
    public LoginCheckInterceptor interceptor2() {
        loginCheck = new LoginCheckInterceptor();
        return loginCheck;
    }

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(loginCheck).addPathPatterns(&quot;/member/myPage.do&quot;);
//모든 요청에 interceptor가 동작되지 않고 요청한 주소에서만 동작됨. myPage.do를 요청했을때만!!
    }


    @Bean
    public TilesConfigurer tilesConfigurer() { 
        //TilesConfigurer 빈은 타일 정의의 위치를 정하고, 불러오고, 일반적으로 타일들을 배치해 주는 역할을 한다. 
        final TilesConfigurer configurer = new TilesConfigurer();
        //해당 경로에 xml 설정 파일을 넣음
        configurer.setDefinitions(new String[] {
                &quot;/WEB-INF/tiles-def/main.xml&quot;,
                &quot;/WEB-INF/tiles-def/member.xml&quot;
        });
        return configurer;
    }
    @Bean
    public TilesViewResolver tilesViewResolver() {
        //TilesViewResolver 빈은 논리적 뷰 이름으로 타일 정의를 결정한다.
        final TilesViewResolver tilesViewResolver = new TilesViewResolver();
        tilesViewResolver.setViewClass(TilesView.class);
        return tilesViewResolver;

    }
}</code></pre><p>그럼 로그아웃 상태에서 로그인이 필요한 페이지를 직접 호출했을때, 에러가 나지 않고 로그인 페이지로 넘어간다.</p>
<p>MemberController.java</p>
<pre><code> /* =======================
     *    회원정보수정
     * =======================*/
    //수정 폼 호출
    @GetMapping(&quot;/member/update.do&quot;)
    public String formUpdate(HttpSession session,Model model) {
        MemberVO user = (MemberVO)session.getAttribute(&quot;user&quot;);

        MemberVO memberVO = memberService.selectMember(user.getMem_num());
        model.addAttribute(&quot;memberVO&quot;, memberVO);

        return &quot;memberModify&quot;;
    }</code></pre><p>webapp&gt; WEB-INF&gt; views&gt; member&gt; memberModify.jsp만들어주기</p>
<p>memberModify.jsp</p>
<pre><code>&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=UTF-8&quot;
    pageEncoding=&quot;UTF-8&quot;%&gt;
&lt;!-- layout에 쏙 들어가는 구조라서 밑의 html코드는 필요없성 --&gt;
&lt;%@ taglib prefix=&quot;form&quot; uri=&quot;http://www.springframework.org/tags/form&quot;%&gt;
&lt;!-- 회원정보수정 시작 --&gt;
&lt;div class=&quot;page-main&quot;&gt;
    &lt;h2&gt;회원정보수정&lt;/h2&gt;
    &lt;form:form modelAttribute=&quot;memberVO&quot; action=&quot;update.do&quot; id=&quot;member_modify&quot;&gt;
        &lt;form:errors element=&quot;div&quot; cssClass=&quot;error-color&quot;/&gt;
        &lt;ul&gt;
            &lt;li&gt;
                &lt;form:label path=&quot;name&quot;&gt;이름&lt;/form:label&gt;
                &lt;form:input path=&quot;name&quot;/&gt;
                &lt;form:errors path=&quot;name&quot; cssClass=&quot;error-color&quot;/&gt;
            &lt;/li&gt;
            &lt;li&gt;
                &lt;form:label path=&quot;nick_name&quot;&gt;별명&lt;/form:label&gt;
                &lt;form:input path=&quot;nick_name&quot;/&gt;
            &lt;/li&gt;
            &lt;li&gt;
                &lt;form:label path=&quot;phone&quot;&gt;전화번호&lt;/form:label&gt;
                &lt;form:input path=&quot;phone&quot;/&gt;
                &lt;form:errors path=&quot;phone&quot; cssClass=&quot;error-color&quot;/&gt;
            &lt;/li&gt;
            &lt;li&gt;
                &lt;form:label path=&quot;email&quot;&gt;이메일&lt;/form:label&gt;
                &lt;form:input path=&quot;email&quot;/&gt;
                &lt;form:errors path=&quot;email&quot; cssClass=&quot;error-color&quot;/&gt;
            &lt;/li&gt;
            &lt;li&gt;
                &lt;form:label path=&quot;zipcode&quot;&gt;우편번호&lt;/form:label&gt;
                &lt;form:input path=&quot;zipcode&quot;/&gt;
                &lt;input type=&quot;button&quot; onclick=&quot;execDaumPostcode()&quot; value=&quot;우편번호 찾기&quot; class=&quot;default-btn&quot;&gt;
                &lt;form:errors path=&quot;zipcode&quot; cssClass=&quot;error-color&quot;/&gt;
            &lt;/li&gt;
            &lt;li&gt;
                &lt;form:label path=&quot;address1&quot;&gt;주소&lt;/form:label&gt;
                &lt;form:input path=&quot;address1&quot;/&gt;
                &lt;form:errors path=&quot;address1&quot; cssClass=&quot;error-color&quot;/&gt;
            &lt;/li&gt;
            &lt;li&gt;
                &lt;form:label path=&quot;address2&quot;&gt;상세주소&lt;/form:label&gt;
                &lt;form:input path=&quot;address2&quot;/&gt;
                &lt;form:errors path=&quot;address2&quot; cssClass=&quot;error-color&quot;/&gt;
            &lt;/li&gt;
        &lt;/ul&gt;
        &lt;div class=&quot;align-center&quot;&gt;
            &lt;form:button class=&quot;default-btn&quot;&gt;수정&lt;/form:button&gt;
            &lt;input type=&quot;button&quot; value=&quot;홈으로&quot; class=&quot;default-btn&quot; 
            onclick=&quot;location.href='${pageContext.request.contextPath}/main/main.do'&quot;&gt;
        &lt;/div&gt;
    &lt;/form:form&gt;
&lt;/div&gt;

&lt;!-- 회원정보수정 끝 --&gt;</code></pre><p>밑에 우편번호 api도 있는데 길어서 잘랐다. 똑같애~ register랑</p>
<p>member.xml</p>
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
    &lt;definition name=&quot;memberModify&quot; extends=&quot;myPage&quot;&gt; 
        &lt;put-attribute name=&quot;title&quot; value=&quot;회원정보수정&quot;/&gt;
        &lt;put-attribute name=&quot;body&quot; value=&quot;/WEB-INF/views/member/memberModify.jsp&quot;/&gt; 
    &lt;/definition&gt;
    &lt;!-- myPage --&gt;
    &lt;definition name=&quot;myPage&quot; template=&quot;/WEB-INF/views/template/layout_mypage.jsp&quot;&gt;
        &lt;put-attribute name=&quot;title&quot; value=&quot;MyPage&quot;/&gt;
        &lt;put-attribute name=&quot;header&quot; value=&quot;/WEB-INF/views/template/header.jsp&quot;/&gt;
        &lt;put-attribute name=&quot;nav&quot; value=&quot;/WEB-INF/views/template/mypage_nav.jsp&quot;/&gt;
        &lt;put-attribute name=&quot;body&quot; value=&quot;/WEB-INF/views/member/memberView.jsp&quot;/&gt;
        &lt;put-attribute name=&quot;footer&quot; value=&quot;/WEB-INF/views/template/footer.jsp&quot;/&gt;
    &lt;/definition&gt;

&lt;/tiles-definitions&gt;</code></pre><p>회원정보수정부분 추가</p>
<p>​</p>
<p>MemberMapper.java</p>
<pre><code>//회원정보 수정
@Update(&quot;UPDATE spmember SET nick_name=#{nick_name} WHERE mem_num=#{mem_num}&quot;)
public void updateMember(MemberVO member);
public void updateMember_detail(MemberVO member);</code></pre><p>짧은것만 어노테이션으로 명시</p>
<p>긴거는 xml에서 ~</p>
<p>​</p>
<p>MemberMapper.xml</p>
<pre><code>&lt;!-- 회원정보수정 --&gt;
&lt;update id=&quot;updateMember_detail&quot; parameterType=&quot;memberVO&quot;&gt;
    UPDATE
        spmember_detail
    SET
        name=#{name},
        phone=#{phone},
        email=#{email},
        zipcode=#{zipcode},
        address1=#{address1},
        address2=#{address2},
        modify_date=SYSDATE
    WHERE 
        mem_num=#{mem_num}
&lt;/update&gt;</code></pre><p>MemberServiceImpl.java</p>
<pre><code>    @Override
    public void updateMember(MemberVO member) {
        memberMapper.updateMember(member);
        memberMapper.updateMember_detail(member);
    }</code></pre><p>트랜잭션 처리. 묶어준당 . </p>
<p>MemberController.java</p>
<pre><code>     /* =======================
     *    회원정보수정
     * =======================*/
    //수정 폼 호출
    @GetMapping(&quot;/member/update.do&quot;)
    public String formUpdate(HttpSession session,Model model) {
        MemberVO user = (MemberVO)session.getAttribute(&quot;user&quot;);

        MemberVO memberVO = memberService.selectMember(user.getMem_num());
        model.addAttribute(&quot;memberVO&quot;, memberVO);

        return &quot;memberModify&quot;;
    }
    //전송된 데이터 처리
    @PostMapping(&quot;/member/update.do&quot;)
    public String submitUpdate(@Valid MemberVO memberVO, BindingResult result,HttpSession session) {
        logger.debug(&quot;&lt;&lt;회원정보수정&gt;&gt; : &quot; + memberVO);

        //유효성 체크 결과 오류가 있으면 폼을 호출
        if(result.hasErrors()) {
            return &quot;memberModify&quot;;
        }

        MemberVO user = (MemberVO)session.getAttribute(&quot;user&quot;);
        memberVO.setMem_num(user.getMem_num());

        //회원정보수정
        memberService.updateMember(memberVO);

        //세션에 저장된 정보 변경
        user.setNick_name(memberVO.getNick_name());
        user.setEmail(memberVO.getEmail());

        return &quot;redirect:/member/myPage.do&quot;;
    }</code></pre><p>회원정보수정까지 된다그</p>
<p>AppConfig.java</p>
<pre><code>    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(loginCheck).addPathPatterns(&quot;/member/myPage.do&quot;)
                                           .addPathPatterns(&quot;/member/update.do&quot;);
        //모든 요청에 interceptor가 동작되지 않고 요청한 주소에서만 동작됨. myPage.do를 요청했을때만!!                            
    }</code></pre><p>update.do를 요청했을때도 interceptor처리 해주기위해서 추가</p>
<p>MemberController.java</p>
<pre><code>     /* =======================
         *    비밀번호 변경
         * =======================*/
    @GetMapping(&quot;/member/changePassword.do&quot;)
    public String formChangePassword() {
        return &quot;memberChangePassword&quot;;
    }</code></pre><p>WEB-INF&gt; member&gt; memberChangePassword.jsp만들고</p>
<p>memberChangePassword.jsp</p>
<pre><code>&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=UTF-8&quot;
    pageEncoding=&quot;UTF-8&quot;%&gt;
&lt;!-- layout에 쏙 들어가는 구조라서 밑의 html코드는 필요없성 --&gt;
&lt;%@ taglib prefix=&quot;form&quot; uri=&quot;http://www.springframework.org/tags/form&quot;%&gt;
&lt;!-- 비밀번호 변경 시작 --&gt;
&lt;div class=&quot;page-main&quot;&gt;
    &lt;h2&gt;비밀번호 변경&lt;/h2&gt;
    &lt;form:form modelAttribute=&quot;memberVO&quot; action=&quot;changePassword.do&quot; id=&quot;member_change&quot;&gt;
        &lt;form:errors element=&quot;div&quot; cssClass=&quot;error-color&quot;/&gt;
        &lt;ul&gt;
            &lt;li&gt;
                &lt;form:label path=&quot;now_passwd&quot;&gt;현재 비밀번호&lt;/form:label&gt;
                &lt;form:password path=&quot;now_passwd&quot;/&gt;
                &lt;form:errors path=&quot;now_passwd&quot; cssClass=&quot;error-color&quot;/&gt;
            &lt;/li&gt;
            &lt;li&gt;
                &lt;form:label path=&quot;passwd&quot;&gt;새비밀번호&lt;/form:label&gt;
                &lt;form:password path=&quot;passwd&quot;/&gt;
                &lt;form:errors path=&quot;passwd&quot; cssClass=&quot;error-color&quot;/&gt;
            &lt;/li&gt;
            &lt;li&gt;
                &lt;label for=&quot;confirm_passwd&quot;&gt;새비밀번호 확인&lt;/label&gt; &lt;!-- 자바빈에 없기때문에 이렇게 명시하는그야 --&gt;
                &lt;input type=&quot;password&quot; id=&quot;confirm_passwd&quot;&gt;
                &lt;span id=&quot;message_id&quot;&gt;&lt;/span&gt;
            &lt;/li&gt;
        &lt;/ul&gt;
        &lt;div class=&quot;align-center&quot;&gt;
            &lt;form:button class=&quot;default-btn&quot;&gt;전송&lt;/form:button&gt;
            &lt;input type=&quot;button&quot; value=&quot;홈으로&quot; class=&quot;default-btn&quot; 
            onclick=&quot;location.href='${pageContext.request.contextPath}/main/main.do'&quot;&gt;
        &lt;/div&gt;
    &lt;/form:form&gt;
&lt;/div&gt;

&lt;!-- 비밀번호 변경 끝 --&gt;</code></pre><p>MemberVO.java</p>
<pre><code>    //비밀번호 변경시 현재 비밀번호를 저장하는 용도로 사용
    @Pattern(regexp=&quot;^[A-Za-z0-9]{4,12}&quot;)
    private String now_passwd;</code></pre><p>자바빈에 추가</p>
<p>member.xml</p>
<pre><code>    &lt;definition name=&quot;memberChangePassword&quot; extends=&quot;myPage&quot;&gt; &lt;!-- 마이페이지 레이아웃 사용 --&gt;
        &lt;put-attribute name=&quot;title&quot; value=&quot;비밀번호변경&quot;/&gt;
        &lt;put-attribute name=&quot;body&quot; value=&quot;/WEB-INF/views/member/memberChangePassword.jsp&quot;/&gt; 
    &lt;/definition&gt;</code></pre><p>ChangePassword.js 파일 만들어</p>
<pre><code>$(function(){
    //비밀번호 변경 체크
    $('#passwd').keyup(function(){
        if($('#confirm_passwd').val()!='' &amp;&amp; 
            $('#confirm_passwd').val()!=$(this).val()){
            $('#message_id').text('비밀번호 불일치').css('color','red');    
        }else if($('#confirm_passwd').val()!='' &amp;&amp; 
            $('#confirm_passwd').val()==$(this).val()){
                $('#message_id').text('비밀번호 일치').css('color','#000');
        }
    });

    $('#confirm_passwd').keyup(function(){
        if($('#passwd').val()!='' &amp;&amp; 
            $('#passwd').val()!=$(this).val()){
            $('#message_id').text('비밀번호 불일치').css('color','red');    
        }else if($('#passwd').val()!='' &amp;&amp; 
            $('#passwd').val()==$(this).val()){
                $('#message_id').text('비밀번호 일치').css('color','#000');
        }
    });    

    $('#member_change').submit(function(){
        if($('#now_passwd').val().trim()==''){
            alert('현재 비밀번호를 입력하세요');
            $('#now_passwd').val('').focus();
            return false;
        }
        if($('#passwd').val().trim()==''){
            alert('새비밀번호를 입력하세요');
            $('#passwd').val('').focus();
            return false;
        }
        if($('#confirm_passwd').val().trim()==''){
            alert('새비밀번호 확인을 입력하세요');
            $('#confirm_passwd').val('').focus();
            return false;
        }
        if($('#passwd').val()!=$('#confirm_passwd').val()){
            $('#message_id').text('비밀번호 불일치').css('color','red');
            return false;
        }
    });

});</code></pre><p>memberChangePassword.jsp에 링크달깅</p>
<pre><code>&lt;script type=&quot;text/javascript&quot; src=&quot;${pageContext.request.contextPath}/js/changePassword.js&quot;&gt;&lt;/script&gt;</code></pre><p>MemberMapper.java</p>
<pre><code>    //비밀번호 수정
    @Update(&quot;UPDATE spmember_detail SET passwd=#{passwd} WHERE mem_num=#{mem_num}&quot;)
    public void updatePassword(MemberVO member);</code></pre><p>MemberServiceImpl.java</p>
<pre><code>    @Override
    public void updatePassword(MemberVO member) {
        memberMapper.updatePassword(member);

    }</code></pre><p>MemberController.java</p>
<pre><code> /* =======================
         *    비밀번호 변경
         * =======================*/
    //비밀번호변경 폼 호출
    @GetMapping(&quot;/member/changePassword.do&quot;)
    public String formChangePassword() {
        return &quot;memberChangePassword&quot;;
    }
    //전송된 데이터 처리
    @PostMapping(&quot;/member/changePassword.do&quot;)
    public String submitChangePassword(
            @Valid MemberVO memberVO,BindingResult result,HttpSession session,HttpServletRequest request,Model model) {
        logger.debug(&quot;&lt;&lt;비밀번호변경 처리&gt;&gt; : &quot; + memberVO);

        //now_passwd와 passwd 유효성 체크 (패턴 안맞으면 걸림. 정규표현식으로 제한한거) 오류가 있으면 폼 호출
        if(result.hasFieldErrors(&quot;now_passwd&quot;) || result.hasFieldErrors(&quot;passwd&quot;)) {
            return formChangePassword();
        }

        MemberVO user = (MemberVO)session.getAttribute(&quot;user&quot;);
        memberVO.setMem_num(user.getMem_num());

        MemberVO db_member = memberService.selectMember(memberVO.getMem_num());
        //폼에서 전송한 현재 비밀번호와 DB에서 받아온 비밀번호 일치 여부 체크
        if(!db_member.getPasswd().equals(memberVO.getNow_passwd())) {
            //DB에 등록되어 있는 비밀번호와 입력한 비밀번호가 불일치 경우
            result.rejectValue(&quot;now_passwd&quot;, &quot;invalidPassword&quot;);
            return formChangePassword();
        }

        //비밀번호 변경
        memberService.updatePassword(memberVO);

        //=== 자동로그인 기능 해제 ===
        //설정되어 있는 자동로그인 기능 해제(모든 브라우저에 설정된 자동로그인 해제)
        memberService.deleteAu_id(memberVO.getMem_num());

        //View에 표시할 메세지 처리
        model.addAttribute(&quot;message&quot;,&quot;비밀번호변경 완료(*재접속시 설정되어 있는 자동로그인 기능 해제)&quot;);
        model.addAttribute(&quot;url&quot;,request.getContextPath() + &quot;/member/myPage.do&quot;);

        return &quot;common/resultView&quot;;
    }</code></pre><p>WEB-INF&gt; views&gt; common&gt; resultView.jsp 생성</p>
<p>resultView.jsp</p>
<pre><code>&lt;%@ page language=&quot;java&quot; contentType=&quot;text/html; charset=UTF-8&quot;
    pageEncoding=&quot;UTF-8&quot;%&gt;
&lt;script&gt;
    alert('${message}');
    location.href='${url}';
&lt;/script&gt;</code></pre><p>비밀번호 변경할 수 있당</p>
<p>AppConfig.java</p>
<pre><code>    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(loginCheck).addPathPatterns(&quot;/member/myPage.do&quot;)
                                           .addPathPatterns(&quot;/member/update.do&quot;)
                                           .addPathPatterns(&quot;/member/changePassword.do&quot;);
        //모든 요청에 interceptor가 동작되지 않고 요청한 주소에서만 동작됨. myPage.do를 요청했을때만!!                            
    }</code></pre>