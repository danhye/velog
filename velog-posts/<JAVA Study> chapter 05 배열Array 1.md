<h3 id="배열array">배열(array)</h3>
<p>: 같은 타입의 여러 변수를 하나의 묶음으로 다루는 것</p>
<p>타입[] 변수이름; or 타입 변수이름[];</p>
<p>타입[] 변수이름; // 배열을 선언(참조변수 선언)
변수이름 = new 타입[길이]; //배열을 생성(실제 저장공간 생성)</p>
<p><strong>타입[] 변수이름 = new 타입[길이]</strong> //한줄로 가능 </p>
<h3 id="인덱스">인덱스</h3>
<p>index의 범위는 0 ~ (배열길이 -1)
배열의 길이는 0일 수도 있다.
배열은 한번 생성하면 <strong>길이를 변경할 수 없다</strong>. 
따라서 이미 생성된 <strong>배열의 길이</strong> 즉, _배열이름.length_은 상수다. <strong>변경 불가능</strong></p>
<p><em>ArrayIndexOutOfBoundsException</em> : 배열의 index가 유효한 범위를 벗어났다는 에러</p>
<h3 id="배열의-초기화">배열의 초기화</h3>
<pre><code>//for문을 활용한 배열 초기화, 직접 값 저장 방법은 생략 ~
//방법 1
int[] score = new int[]{50,60,70,80,90}; // 배열의 생성과 초기화 동시에

//방법 2
int[] score = {50,60,70,80,90}; //new int[] 생략 가능

//방법 3
int[] score;
score = new int[]{50,60,70,80,90};

//방법 4 - 메서드 활용
int add(int[] arr) { /* add메서드 내용 생략 */};

int result = add(new int[]{100,90,80,70,60};

//번외 - 길이가 0인 배열
int[] score = new int[0];
int[] score = new int[]{};
int[] score = {]; //new int[]가 생략됨</code></pre><h3 id="배열의-출력">배열의 출력</h3>
<pre><code>int[] iArr = { 100, 90, 80 ,70, 60 };
System.out.println(Arrays.toString(iArr)); //[100 , 90, 80, 70, 60]이 출력

System.out.println(iArr); // 참조변수를 출력하면 '배열의 주소'가 출력 ex) 타입@주소

char[] chArr = {'a', 'b', 'c'};
System.out.println(chArr); //abc가 출력</code></pre><p>예외!
char배열은 참조변수를 출력했을 때, 
배열의 주소가 아닌 각 요소가 구분자 없이 그대로 출력된다. </p>
<p>[5-2 실습]
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/926d125f-2cdd-463a-b2b3-337446faa4bd/image.png" /></p>
<h3 id="배열의-복사">배열의 복사</h3>
<p>배열은 한번 생성하면 그 길이를 변경할 수 없기 때문에 더 많은 저장공간이 필요하다면, 큰 배열을 새로 만들고 이전 배열로부터 내용을 복사해야한다.</p>
<p><strong>for문을 이용해서 배열 복사</strong></p>
<pre><code>int arr[] = new int[5];

int[] tmp = new int[arr.length*2]; //기존 배열보다 길이 2배인 배열 생성

for(int i=0;i&lt;arr.length;i++){
    tmp[i] = arr[i]; //arr[i]의 값을 tmp[i]에 저장
}

arr = tmp; //참조변수 arr이 새로운     배열을 가리키게 한다.</code></pre><p><img alt="" src="https://velog.velcdn.com/images/danhye821/post/abe2f4c9-a864-4f33-880a-544afeafed07/image.jpg" />
<strong>System.arraycopy()를 이용해서 배열 복사</strong> - 효율적</p>
<p><strong>System.arraycopy(arr, 0, newArr, 0, arr.length);</strong>
: arr[0]에서 newArr[0]으로 arr,length개의 데이터를 복사</p>
<p>[5-4 실습]
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/755e3915-78f3-4d3e-ba08-9ed6bb5e7e81/image.png" /></p>
<p>[5-5 실습] - 총합과 평균
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/2d76783f-686b-4c6f-8d54-7258f168ef0c/image.png" />
[5-6 실습] - 최대값과 최소값
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/1d245703-b9d2-458b-8db3-d65153182598/image.png" />
[5-7 실습] - 섞기(shuffle)
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/32e382d0-4151-4a86-a53e-7dd4258e2222/image.png" />
[5-8 실습] - 섞기(shuffle)
<img alt="" src="https://velog.velcdn.com/images/danhye821/post/49b7b3ed-817e-4b93-921c-e2be2902c5d0/image.png" /></p>