# DAY09  

## Index  
1. [신규 이벤트 영역](#신규-이벤트-영역-이전-다음-페이지-버튼-구현)
   * [적용 기법 1: IR 기술](#적용-기법-1-ir-기술)
   * [적용 기법 2: SPRITE 기술](#적용-기법-2-sprite-기술)

2. [관련 사이트 영역: 리스트 드롭 다운 구현](#관련-사이트 영역-리스트-드롭-다운-구현)  
   * [적용 기법: Transition 기술](#적용-기법-transition-기술)  
   * [선택자 재정의 팁](#선택자-재정의-팁)  

     ​

3. [인기사이트](#2인기사이트)
   * [설계](#21설계)  
   * [스타일링 css 파트](#22-스타일링-css파트)  
     * [넘버링](#221-넘버링)
     * [사이트 변동 이미지 배치](#사이트-변동-이미지-배치)
     * [Detail](#detail)

4. [미디어쿼리](#미디어쿼리)  

5. [Table tag](#table-tag)  

6. [브라우저 동영상](#브라우저-동영상)

---

## 신규 이벤트 영역: 이전, 다음 페이지 버튼 구현
![신규 이벤트 영역](https://cloud.githubusercontent.com/assets/22453170/22326873/e90c85e8-e3f7-11e6-9c2f-5da7395f2104.png)  

### 적용 기법 1: IR 기술
이미지에 해당하는 적절한 대체 텍스트를 제공하여 접근성을 향상하는 기법
_참고: 이미지가 렌더링되지 않았을 때에도 텍스트를 읽어보고 내용을 파악할 수 있도록 도와주는 역할을 하기 때문에 대체 텍스트를 이미지 주변부에 위치하도록 해야 한다._


1. 시멘틱 마크업하기
```html
<div class="btn-event">
  <button type="button" class="btn-event-prev">이전 이벤트 보기</button>
  <button type="button" class="btn-event-next">다음 이벤트 보기</button>
</div>
<!--참고: button 태그의 기본 속성은 submit이기 때문에 서버에 요청하지 않으려면 button type을 button으로 바꿔주어야 함-->
```

2. 텍스트로 마크업한 부분에 배경 이미지 입히기  
```css
.btn-event-prev, .btn-event-next {
  background: url(images/back_forward.png) no-repeat;
}
```

3. 마크업한 텍스트를 요소 영역 밖으로 보내기  

 ![요소 영역 밖](https://cloud.githubusercontent.com/assets/22453170/22328335/037ec3bc-e3ff-11e6-99af-19d081f784f3.png)
```css
.btn-event-prev, .btn-event-next {
  background: url(images/back_forward.png) no-repeat;
  width: 19px;
  height: 18px;
```
렌더링하고 싶은 이미지 크기만큼 버튼 태그의 높이, 넓이를 지정한다. 텍스트는 해당 영역 안에 크기가 맞지 않기 때문에 흘러 넘치게 된다.

4. 넘친 텍스트를 숨기는 트릭 2가지  
   4-1. indent trick   
   텍스트를 요소 영역 넓이만큼 들여쓰기해서 텍스트가 없는 것처럼 보이게 만들 수 있다. 이때 이미지가 지정한 높이, 넓이보다 커질 경우를 대비해서 개행되지 않도록 만든다 (`nowrap`). 흘러 넘친 내용은 `overflow` 기본 속성을 `overflow: hidden;`으로 바꿔서 보이지 않도록 만든다.
```css
.btn-event-prev, .btn-event-next {
  background: url(images/back_forward.png) no-repeat;
  width: 19px;
  height: 18px;
  text-indent: 19px;
  white-space: nowrap;
  overflow: hidden;
}
```
4-2. padding trick  
요소 영역의 높이를 0으로 만든다. 대신 padding-top을 해당 높이만큼 주어 텍스트를 밀어 낸다. indent trick과 동일하게 넘친 내용은 `overflow: hidden;`을 지정하여 보이지 않도록 한다.
```css
.btn-event-prev, .btn-event-next {
  background: url(images/back_forward.png) no-repeat;
  width: 19px;
  height: 0px;
  padding-top: 18px;
  overflow: hidden;
}
```

### 적용 기법 2: SPRITE 기술
서버 성능 저하를 막기 위해 이미지 중에서 원하는 부분만 노출하는 기법

0. 스프라이트 적용 전
   ![스프라이트 적용 전](https://cloud.githubusercontent.com/assets/22453170/22329986/363ddd9e-e407-11e6-9114-ef98eb94e9d4.png)

```css
.btn-event-prev, .btn-event-next {
  width: 19px;
  height: 18px;
  background-image: url(images/back_forward.png);
  background-repeat: no-repeat;
}
```
두 요소의 크기도 같고 배경 이미지도 같다. 배경 이미지 좌표는 기본이 (0,0)이기 때문에 해당 이미지의 동일한 부분이 노출되고 있다.

1. 배경 이미지 좌표 옮기기

![스프라이트 적용 후](https://cloud.githubusercontent.com/assets/22453170/22330614/79c49eec-e40a-11e6-8ea7-6655122f3ad7.png)

위 사진처럼 보이려면 뒤에 보이는 `.btn-event-next`만 해당 이미지의 다른 부분을 보여주도록 설정하면 된다.

1-1. % 단위 활용
```css
.btn-event-next {
  background-position: 100% 0;
}
```
배경 이미지 좌표와 해당 요소 좌표가 일치하는 좌표를 찾아 지정한다.

1-2. px 단위 활용
```css
.btn-event-next {
  background-position: -35px 0;
}
```
자신의 위치에서 이미지가 보일 수 있는 위치를 찾아 입력한다. 이 경우에는 원래 있던 자리(0,0)보다 왼쪽으로 이동해야 하기 때문에 음수값을 지정했다.

## 관련 사이트 영역: 리스트 드롭 다운 구현
![리스트 드롭 다운](https://cloud.githubusercontent.com/assets/22453170/22330969/349c7b94-e40c-11e6-9ba2-23320a611eb6.png)

### 적용 기법: Transition 기술
요소에 적용한 변화가 일정한 시간 동안 보이게 하는 기법  
_참고: 애니메이션과 달리 해당 요소에 hover, focus 등의 트리거가 있어야 transition 효과가 실행된다._

1. 초기 상태 지정하기
```css
.related-list {
  background: #fff;
  margin-top: 10px;
  border: 1px solid #aaa;
  border-radius: 3px;
  overflow: hidden;
  height: 25px;
}
```
마우스를 올리기 전에는 리스트의 가장 첫 번째 li 요소인 '생산성 본부'만 보이도록 높이를 25px로 지정한다.

2. 변화된 상태 지정하기
```css
.related-list:hover {
  padding: 10px 0;
  height: 125px;
}
```
마우스를 올리면 전체 리스트가 보이도록 전체 높이인 125px로 높이를 재지정한다.

3. Transition 태그 사용하기
```css
.related-list {
  background: #fff;
  margin-top: 10px;
  border: 1px solid #aaa;
  border-radius: 3px;
  height: 25px;
  overflow: hidden;
  transition: all 1s;
  /*참고: 변화된 속성 전부를 css에서 찾아서 변화할 수 있도록 all을 쓰거나 변화시킨 속성 height만 써도 된다.*/
}
```
`transition`에 변화를 줄 요소와 지속 시간을 설정한다.

## 선택자 재정의 팁
### 선택자 재정의가 필요한 경우 주의사항
```css
/*h1과 .title이 같은 요소라고 가정.*/

.example h1 {
  font-size: 1.6rem;
}
/*클래스명(10점)+태그(1점)=11점*/

.title {
  font-size: 2.0rem;
}
/*클래스명(10점)*/
```
동일한 요소를 다시 선택해서 글자 크기를 재정의한 것처럼 보인다. 그러나 주석에서 계산해 놓은 것처럼 `.title`의 점수가 낮기 때문에 재정의한 상태가 반영되지 않을 것이다.

```css
.example .title {
  font-size: 2.0rem;
}
/*클래스명(10점)+클래스명(10점)=20점*/
```
따라서 위의 코드처럼 부모 클래스명을 같이 선택하여 점수를 높여서 재정의해야 한다.

```css
.title {
  font-size: 2.0rem!important;
}
```
또는 `!important`를 활용해서 점수와는 상관없이 무조건적으로 우선순위를 높일 수도 있다. 그러나 이는 선택자 점수 체계를 무력화하는 요소이기 때문에 신중하게 사용해야 한다.

## 2.인기사이트





![인기사이트 레이아웃](https://cloud.githubusercontent.com/assets/25189066/22331034/8d66603c-e40c-11e6-9bb1-685fb4c79938.PNG)



## 2.1설계

1.논리설계

인기사이트-인기사이트목록-더보기 순으로 설계

2.시멘틱 설계

의미들이 있는지 없는지에 따라서 마크업이 달라집니다.

```html
  <div class="favorite">
        <h2 class="favorite-heading">인기 <span>사이트</span></h2>
        <ol class="favorite-list">
          <li class="no1">W3C<em class="up">상승</em></li>
          <li class="no2">웹 접근성 연구소<em class="down">하락</em></li>
          <li class="no3">패스트캠퍼스<em class="stop">멈춤</em></li>
          <li class="no4">CSS 젠가든<em class="up">상승</em></li>
        </ol>
        <a href="#" class="favorite-more bullet" title="인기 사이트">더보기</a>
      </div>
+사이트 순위 나내내는 그림에 각각의 클래스를 부여해서 컨트롤 합니다.
												(의미가 있으면 strong or em으로 강조 span)
+순위 그림과 기본적인 리스트 목록의 숫자를 어떻게 처리할지?
```

## 2.2 스타일링 CSS파트

 기본적인 레이아웃은 앞서 했던 이벤트 예제와 새소식과 비슷합니다.(더보기와 헤딩요소)

이번 예제에서 주의깊게 볼 점은

1. 목록에서 숫자를 기본 스타일을 숨기고 커스터마이징 하여 숫자를 생성하는 것!
2. 사이트 변동을 나타내는 이미지를 어떻게 적절히 배치할지

### 2.2.1 넘버링

#### 2.2.1.a 기존 스타일 안 보이게 하기

```css
list-style-type:none;으로 접근하면 none은 완전히 안 보이게 하는 것이므로(청각 장애인분들이 숫자를 보지 못합니다.) 다른 방법으로 접근 ,접근성을 생각해서
앞서 했었던 방법인 overflow-hidden으로 접근합니다.
.favorite-list{
  overflow: hidden;
}
그렇지만 브라우저별로 숨김 컨텐츠를 다르게 해석하여 컨텐츠 박스 크기가 달라지는 경우가 있어서 후에
li목록의 크기를 지정해줍니다
```



#### 2.2.1.b 숫자 스타일 커스터마이징

before요소를 사용하여 앞쪽에 같은 스타일을 만들어 줍니다.

+새로 배운 속성 counter

```css
.favorite-list li{
  margin:10px 0;
  counter-increment:number;
  /*첫번째 리스트에 1 두번째 리스트에 2*/
  height:16px;
  position: relative;

}
```

적용시킬 부모요소에 coutner-increment 이름을 선언해주고

적용시키는 요소에 그 이름과 어떤 스타일로 적용시킬지를 적어줍니다.

```css
content:counter(number,decimal);
```

counter가 없으면 일일이 클래스를 부여해줘서 따로 넘버링 할 것을 함수로 접근하여 편하게 작업가능

```css
.favorite-list li::before{
  content:counter(number,decimal);
  color:#fff;
  background: #aaa;
  padding:0 5px;
  font-size:1.2rem;
  border-radius:3px;
  margin-right:5px;
}

카운터 속성을 이용하면 no1 no2클래스로 개별로 접근할 필요 없이 넘버링이 됩니다.
```



*부가 counter설명

content는 내용을 생성하는 속성

content:counter는 숫자를 자동으로 생성하는 역할을 합니다.



counter-reset: 카운터이름/시작숫자

counter-increment: 카운터이름/증감숫자

counter(),counters()라는 함수로 사용하고

counter(name.style) : 이름만 쓸 경우 기본값, decimal이 적용

[counters설명](http://aboooks.tistory.com/262)



### 사이트 변동 이미지 배치



이곳에서 이미지 배치 역시 ir기법으로 배경으로 이미지를 넣는 방법을 이용합니다.

우선 리스트내에서 오른쪽으로 정렬 시켜야 됩니다.

정렬시키는 방법

1. float:right float가장 편하다. (오른쪽 정렬 특히) 크기가 list랑 딱 맞는

   ​		but 반응형에는 부적합,  이미지 크기가 텍스트랑 정렬시킬 때

   ​		margin으로 맞춰줘야 된다.

2. 플렉스 박스를 이용하여 속성들로 가운데 정렬

   ​	but flex는 아직 최신 브라우저들마나 지원

3. 포지션 이용한 방법(수업때 한 방법)

   부모요소인 리스트에 position relative를 줘서 위치 기준을 잡고

   position absoulte로 위치를 잡는데 정렬을 맞춰 주기 위해서 top 50%로 맞춰줍니다

    ![1](https://cloud.githubusercontent.com/assets/25189066/22330569/40986b6c-e40a-11e6-8534-a23969e6fb46.png)

   ![2](https://cloud.githubusercontent.com/assets/25189066/22330570/409a52f6-e40a-11e6-86de-62ef3eb49f93.png)

```css
.favorite-list em{
  background: url(images/rank.png)no-repeat;
  font-style: normal;
  position: absolute;
  top: 50%;
  right: 0;
  margin-top: -5px;
  width:9px;
  height: 11px;
  text-indent: 9px;
  white-space: nowrap;
  overflow: hidden;
  /*width랑 똑같은 크기/ 플러스 화이트스패이스 노랩*/
}
em.stop{
  background-position: 0 50%;
  /*임포턴트로 써주거나 앞쪽에 태그를 써줍니다. 우선순위!!! */
}
em.down{
  background-position: 0 100%;
}
```

- text-indent : white-space nowrap 개행시키지 않아야 제대로 된 대체텍스트 전달하기 쉽습니다



### Detail

```css
.event-heading,.related-heading,.favorite-heading{
  font-size:1.6rem;
  font-family: "Noto Sans Bold";
}
.event-heading span,.related-heading span,.favorite-heading span{
  color:#f00;
}
+더보기나 제목요소 같은부분에 같은 속성이 중복으로 들어가는  요소들은 모아서 한 꺼번에 줍니다:D
```

1.재사용 패턴이 많은 경우 box라고 클래서 지정해 놓고 마크업에 클래스를 추가하여 스타일 적용하는 방법: 모듈화 편함 ,다만 시멘틱을 건드려야 된다

2.클래스들을 ,찍으며 한곳에 모아 놓는 방법:시멘틱하지만 조금 수고스럽습니다.

상황에 따라서 1,2번으로 적절히 사용합시다.

```css
list-style-type:none;으로 접근하면 none은 완전히 안 보이게 하는 것이므로(청각 장애인분들이 숫자를 보지 못합니다.) 다른 방법으로 접근 ,접근성을 생각해서
앞서 했었던 방법인 overflow-hidden으로 접근합니다.
.favorite-list{
  overflow: hidden;
}
그렇지만 브라우저별로 숨김 컨텐츠를 다르게 해석하여 컨텐츠 박스 크기가 달라지는 경우가 있어서 후에
li목록의 크기를 지정해줍니다
```

- 선택영역을 넓히고 싶다면 li대신 a로 작업합니다.

------

# 미디어쿼리

## 이용 목적

```
사용 기기에따라 각각 다른 레이아웃을 작성하거나, 다른 미디어 종류에 따라 css코드를 작성, 즉 반응형 웹페이지를 작성할 때 유용한 문구다.   
```

## 주의사항

```html
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
```

> HTML안에 위 값이 설정되지 않으면 모바일에서 작용되지 않을 수 있다(모바일을 위한 코드)  
> viewport 선언하지 않을 경우 모바일에서 데스크탑으로 인식할 수 있다.  

## 준비사항

```html
<link rel="stylesheet" href="css/media.css">
```

> media CSS 폴더와 연결.  

```css
@media all and (min-width:769px)
```

> CSS에 미디어쿼리를 사용하겠다고 선언.  
>  사용자 해상도가 768px 이상일 때 이 코드가 실행 되도록 선언.

```html
 <!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>미디어 쿼리</title>
  <link rel="stylesheet" href="css/media.css">
</head>
<body>
  <h1>미디어 쿼리의 활용</h1>
</body>
</html>  
```

> html 선언  

```css
/*인코딩 선언*/
@charset "UTF-8";
body{
  background: yellow;
}
/*공통, 너비 769px 이상일 때*/
h1{
  background-repeat: no-repeat;
  white-space: nowrap;
    overflow: hidden;
}
/*Desktop Device */
h1{
  width: 100px;
  height: 20px;
  text-indent: 100px;
  background: url(../images/footer_logo.png) no-repeat;
}
@media all and (min-width:769px) {
/*사용자 해상도가 769px 기준일 때 Up, Down시코드가 실행됨. */
  body{
    background: blue;
  }
  h1{
    width: 165px;
    height: 35px;
    text-indent: 165px;
    background-image: url(../images/logo.png);
  }
}
```

> CSS 선언  
> 위 내용은  `min-width:769px`이므로 너비 768px값 기준, 이상이면 색생,로고의 기본값 `@media all and (min-width:769px)`상단의 값 이하이면 `@media all and (min-width:769px)`값 이하의 값이 적용된다.  
>
> ### 미디어쿼리는 viewport에 따라 반응한다.

------

# Table tag

## 이용목적

```
표를 만들어 데이터를 정리하기 위함.  
```

## 기본 구성요소

```
th: table head의 약어, 표의 제목(굵은 글씨, 중앙 정렬)  
tr: table row의 약어, 표의 가로줄(보통 글씨, 왼쪽 정렬)  
td: table data의 약어, 표의 셀(보통 글씨, 왼쪽 정렬)
```

![default](https://cloud.githubusercontent.com/assets/25189138/22330934/08abd732-e40c-11e6-9def-ac232a0acd2e.png)

### HTML5는 지원하지 않는속성

```
align, bgcolor, border, cellpadding, cellsapacing, frame, rules, `summary`, width.   
```

### 마크업

![table2](https://cloud.githubusercontent.com/assets/25189138/22330688/d73d5d20-e40a-11e6-88fe-09dff2843633.png)

### OUTPUT

  <table border="1" aria-describedby="t-desc">

```
<caption>2017년 1월 시험 점수</caption>
<tr>
  <th scope="col">성명</th>
  <th scope="col" id="en">영어</th>
  <th scope="col">수학</th>
</tr>
<tr>
  <th scope="row" id="name1">홍길동</th>
  <td headers="en name1">90</td>
  <td>80</td>
</tr>
<tr>
  <th scope="row">김철수</th>
  <td>70</td>
  <td>100</td>
</tr>
<tr>
  <th scope="row">합계</th>
  <td>160</td>
  <td>180</td>
</tr>
```

  </table>

- tr 4개 이므로 4행, tr의 내부에 th,td 모두합해 3개 이므로 3셀 `4행 3열`인 표가 완성이 되었다.      

> summary 속성  
>
> > table 요약을 지정한다. 테이블 요약은 복잡한 구성의 테이블에 대한 설명 정보를 의미하는데 HTML5에서는 권장되지 않는요소
>
> border 속성
>
> > 테이블 테두리선지정. `기본적으로 테두리선이 없기때문에` 보이게 하려면 border속성을 지정해야함  
>
> caption 요소
>
> > table제목을 표시한다. 가장 먼저 읽혀져야 하는 제목 요소니 table 요소 바로 다음에 위치한다. `table 당 한개의 caption 만 설정할 수 있다.`
>
> scope 속성
>
> > 헤더의 셀 범위를 지정함.  
> > `scope="row"`헤더 셀이 가로 행에 적용된다.  
> > ![row](https://cloud.githubusercontent.com/assets/25189138/22331570/845ab5d0-e40f-11e6-9b55-9df0cad2c4cd.png)    
> > `scope="column"`헤더 셀이 세로 행에 적용된다.  

------

# 브라우저 동영상

## 시작 방법

```
   HTML5 video 요소를 사용하여 웹 페이지에 동영상 플레이어를 추가하는 가장 기본적인 방법은 한 줄의 HTML로 수행됩니다. controls 특성을 추가하면 사용자가 동영상 재생을 제어할 수 있습니다. `CSS 스타일시트를 사용하여 요소의 스타일과 위치를 지정할 수 있습니다.`  
```

```html
<video src="demo.mp4" controls autoplay >HTML5 Video is required for this example</video>
```

> src
>
> > 재생할 동영상 파일을 가리킵니다. 동영상을 재생하려면 비디오 파일의 URL에 src 특성을 할당합니다.   
>
> controls
>
> > 브라우저에 기본 제공 재생 컨트롤을 표시하도록 지정합니다. 최소한 재생 및 일시 중지 컨트롤, 동영상에서 앞/뒤로 건너뛰는 단추 또는 진행률 표시줄, 시간 카운터가 표시되어야 합니다.
>
> autoplay
>
> > 동영상을 로드 후 바로 재생하도록 하는 부울 특성입니다.
>
> source
>
> > 동영상 콘텐츠 형식에 대한 "자동 맞춤"을 제공합니다.대개 Windows Internet Explorer의 경우 .mp4 파일이 선택되고, 다른 브라우저의 경우에는 .ogg/.ogv 형식이 선택됩니다. 이 예에서는 다음과 같은 세 가지 파일 형식을 갖는 동영상 요소를 보여 줍니다.  

```html
<video controls poster="demo.jpg">
  <source src="demo.mp4" type="video/mp4" />
  <source src="demo.webm" type="video/webm"/>
  <source src="demo.ogv" type="video/ogg"/>             
  <p>Fallback code if video isn't supported</p>/
</video>
```

- `mp4, webm 및 ogg 동영상의 세 가지 형식이 나열됩니다.` 브라우저에 따라 동영상 요소에서 재생 가능한 형식을 선택합니다. 아무 형식도 재생할 수 없거나 HTML5 동영상이 지원되지 않는 경우 작업이 완료되지 않고 video 태그 사이에 포함된 텍스트가 표시됩니다. 이 "대체" 동작을 사용하여 메시지를 표시하거나 포함된 플레이어를 추가할 수 있습니다.   
- HTML5 동영상을 지원하지 않을 경우 `동영상 콘텐츠에 대한 링크만 제공`합니다.  

```html
HTML5 Video is required for this example.
<a href="demo.mp4">Download the video</a> file.
```

### 참고 문헌

[table 요소](http://webdir.tistory.com/317)  
[미디어 쿼리요소](http://aboooks.tistory.com/365)  
[브라우저 비디오](https://msdn.microsoft.com/ko-kr/library/hh924820(v=vs.85).aspx)
[CSS Trick: SPRITE기술](https://css-tricks.com/css-sprites/)
[Pure CSS Menu: Drop Down](http://codepen.io/andornagy/pen/xhiJH)
