# DAY05  

## Index  
1. [visual 영역에 애니메이션 추가](#visual-영역에-애니메이션-추가)  
   * [배경 이미지 설정](#배경-이미지-설정)
2. [로그인 페이지 구현](#로그인-페이지-구현)  
   * [구조 설계](#구조-설계)  
   * [시멘틱 요소](#시멘틱-요소)  
   * [네이밍](#네이밍)  
   * [구현](#구현)  
3. [Web Forms 2.0](#web-forms-20)
   * [`<input>` 요소의 `type`](#input-요소의-type)  
   * [`<select>` 요소](#select-요소)  
4. [참고 내용](#참고-내용)  
   * [CSS 단위](#css-단위)
5. [참고 문서](#참고-문서)  

---

## visual 영역에 애니메이션 추가  
visual 영역에 애니메이션을 추가한다. 추가하는 애니메이션은 가상 요소를 사용하여 적용한다.  

### 배경 이미지 설정  

#### `position`을 사용하여 가상요소 좌표 설정  
`absolute`, `relative`, `fixed` 모두 `offset` 값을 설정하지 않으면 제자리에 위치한다. 따라서, `offset` 값을 부여해야만 기준(부모요소, `viewport`, 자신위치)에 따라 위치가 결정된다.  
> 참고 : `translate(0, 0)`는 자신의 위치가 기본 값. (`position`이 없어도 동작함.)  

기본적으로 가상요소는 `inline`이지만 크기 조절을 위해 `block` 요소로 변경해야한다. 이 역할을 `absolute`가 해주고 있어서 `display: block`을 추가하지 않는다.

```css
.visual::before, .visual::after{
  content:"";
  width: 100%;
  height: 120px;
  position: absolute;
  top: 0;
  left: 0;
}
```

#### 가상요소에 이미지 설정
가상요소 (`::before`, `::after`)를 사용해서 애니메이션을 적용할 영역에 배경 이미지를 설정한다. 이미지 적용할 때, **다중 배경 이미지** 를 사용한다. **다중 배경 이미지** 는 여러 개의 배경을 동시에 적용 가능한 기능이며 먼저 선언한 이미지가 위에 위치하게 된다. 적용시 각각의 이미지들을 '**,**'로 구분한다.

```css
.visual::before{
  background: url(images/ani_flower_01.png) no-repeat  0 -15px, url(images/ani_flower_02.png) no-repeat 670px 0;
}
.visual::after{
  background: url(images/ani_flower_03.png) no-repeat  300px 0, url(images/ani_flower_04.png) no-repeat 800px 20px;
}
```

#### 애니메이션 적용  
`opacity`값을 변경하는 `flower-ani` 애니메이션을 적용한다. 두 개의 가상요소 모두 동일한 애니메이션을 사용하기 때문에 대표속성으로 함께 적용하고, 추가적인 설정 값은 개별속성을 사용해서 적용한다.  

```css
.visual::before, .visual::after{
  ...
  animation: flower-ani 2s infinite alternate;
}
.visual::before{
  ...
}
.visual::after{
  ...
  animation-delay: 1s;
}
```
[ [Index로 바로가기](#index) ]  

---

##  로그인 페이지 구현  

### 구조 설계  
시안에서 구현하려고 하는 로그인 영역을 **논리적인 흐름을** 기준으로 구조 설계.  
> 로그인  
>> 아이디  
>> 입력상자 폼  
>> 비밀번호  
>> 입력상자 폼  
>> 버튼 : 로그인  
>  
> 텍스트 링크

[ [Index로 바로가기](#index) ]  

### 시멘틱 요소  
어떤 의미를 갖는지, 적용하려는 의미를 갖는 요소가 존재하는지 확인 후 사용하거나 없을 경우 대체할 요소 선택.  
> `<h2>` : 제목 역할 (로그인)   
>> 같은 레벨 요소들의 정보 전달로 끝나는게 아니라 요청과 응답 작업이 반복 실행 되는 영역.  
>> `<form>`  
>>> `<fieldset>` : 묶어놓는 역할 중 전용 그룹화 요소 사용.  
>>>> `<legend>` : 그룹화 영역을 설명하는 요소 사용.  
>>>> 입력 받기 위한 영역 구성.  
>>>> 연관 관계를 알려주기 위해 **`id`** 같은 설명 값을 입력상자에 연결해 줘야함.  
>>>> 아래와 같이 `<input>`과 `<label>`을 연결시켜준 방식을 **명시적 레이블링** 라고 함.  
>>>> `<label for="user-id">` `<input id="user-id">`  
>>>> `<label for="user-pw">` `<input id="user-pw">`  
>>>> `<input>` 요소에 `title` 속성을 주어 연결하는 방법을 **비명시적 레이블링** 라고 함.  
>>>> 접근성을 위해서 **명시적 레이블링** 사용을 권장함.  
>>>> `<input>` 또는 `<button>` : 로그인 버튼.  
>  
> `<a>` 요소 사용하여 처리함 : 텍스트 링크.  
> `<ul>` 요소 사용하여 `<li>`로 처리하는 게 좋음.  

[ [Index로 바로가기](#index) ]  

### 네이밍  
선택한 요소들에 `class`와 `id` 이름을 선언한다.  
> `div.login` : 전체를 묶는 영역.  
> 만약, `<section>` 사용해서 묶으면, 제목 레벨에 대한 고민을 안해도 됨.  
> 하지만, `<section>` 요소는 하위 섹션이 존재할 경우에 적합.  
>> `h2.login-heading`  
>>> `fieldset`  
>>>> `legend`  
>>>> `label` `input#user-id`  
>>>> `label` `input#user-pw`  
>>>> `button.btn-login`  
>  
> `div.member`  
>> `a.signup` `a.find`  

[ [Index로 바로가기](#index) ]  

### 구현  
설계한 내용을 기반으로 구현한다.  
#### 버튼/동작.  
> `action` : `form`에 있는 정보들을 전송할 주소 설정하는 속성.  
> 여기서는 `pseudo-protocol` 방식을 사용해 `javascript`를 실행함.  
> `type="submit"` : 기본값이며 정보를 전송한다.  
> `type="clear"` : 값을 초기화한다.  

```html
<form action="javascript:alert('회원님 반갑습니다.');">
  <fieldset>
    <legend>회원 로그인 폼</legend>
    <button type="submit" class="btn-login">로그인</button>
  </fieldset>
</form>
```

#### 아이디/패스워드 영역.  
> `<label>`과 `<input>` 연결해주고 `<input>` 유형 설정.  
> `required` : 입력 값이 필수임을 지정하는 속성.  
```html
<div class="user-id">
  <label for="user-id">아이디</label>
  <input type="email" id="user-id" placeholder="guest@fastcampus.co.kr" required>
</div>
<div class="user-pw">
  <label for="user-pw">비밀번호</label>
  <input type="password" id="user-pw" placeholder="8자리 이상" required>
</div>
```

[ [Index로 바로가기](#index) ]  

---

## Web Forms 2.0

### `<input>` 요소의 `type`

#### `text`  
기본값으로 한 줄 텍스트 입력 칸을 생성한다. 한글 영문 상관없이 문자 길이로 판단하며 최소 문자 길이 설정하는 속성은 없다.  
> `maxlength` : 최대 입력 길이.  

```html  
<input type="text" maxlength="6">
```  

#### `search`  
검색 창을 생성한다. `datalist`요소와 함께 사용하면 유용하다.  
> `datalist`  
> `html5`에 추가된 요소로 form 요소에서 미리 지정된 옵션 목록을 제공함.  
> 사용자들이 특정 글자를 입력하면 그에 해당하는 미리 지정된 목록을 보여줌.  
> `<option>`요소를 포함함.  

```html
<input type="search" autofocus list="search-suggestions">
<datalist id="search-suggestions">
  <option label="DM" value="Depeche Mode">
  <option label="Moz" value="Morrissey">
  <option label="NO" value="New Order">
  <option label="TC" value="The Cure">
</datalist>
```  
> 위의 예제에서 함께 사용한 `autofocus`.  
> 페이지 접속시 자동으로 포커스를 갖도록 하는 속성.  
> 자동적으로 포커스가 이동되어 유용할 수도 있지만 접근성에 문제가 생길 수 있음.  

#### `tel`  
전화번호 입력 창을 생성한다. 모바일 환경의 경우 입력상자 선택시 숫자패드가 나타난다.  
> `pattern` : 입력 값이 통과해야 할 정규 표현식을 명시함.  
> 해당 속성을 사용했다면 반드시 `title` 속성을 써서 이 패턴에 대한 설명을 제공해야함.  

```html
<input type="tel" pattern="[0-9]{10}" name="tel" title="Phone Number?!?!">
```

#### `password`  
`text` 속성과 같지만, 문자를 숨겨서 표시한다.  
> `placeholder` : 한 단어나 짧은 구로 이루어진 입력 값에 대한 힌트.  

```html
<input type="password" placeholder="Password">
```

#### `url`  
웹 주소 입력 창을 생성한다.  

```html
<input type="url" id="url" name="earl" required>
```

#### `email`  
email 주소 창을 생성한다. 입력 값이 email 형식이 맞는지 자동적으로 체크한다.  

```html
<input type="email" placeholder="foo@bar.com">
```

#### 날짜 관련 유형들  
`datetime` : 날짜 시간 창을 생성한다.(년, 월, 일, 시, 분, 초, 초의 분할) 표준시간을 기준으로 한다.  
> 모든 브라우저에서 지원하지 않으며, `datetime` 유형은 브라우저에서 인식하지만 `datetime`에 대한 UI가 없는 상태임.  

`datetime-local` : 날짜 시간 창을 생성한다.(년, 월, 일, 시, 분, 초, 초의 분할) 표준 시간 없음.  
`date` :  날짜 입력 창을 생성한다.  
`month` : 년과 달의 입력 창을 생성한다.  
`week` : 년과 주의 입력 창을 생성한다.  
`time` : 시간 입력 창을 생성한다.
> 날짜 관련 유형들은 `HTML5`에 새로 추가된 유형으로 모든 브라우저에서 지원가능하지 않음.  
> 날짜 선택을 위해 나타나는 캘린더는 마우스로만 선택 가능.  
> 네이티브 캘린더를 지원하면 네이티브 사용하는 것을 권장.  

```html
<input type="datetime">
<input type="datetime-local">
<input type="date">
<input type="month">
<input type="week">
<input type="time">
```

#### `number`  
숫자 입력을 위한 창을 생성한다.  
> 숫자 제한을 줄 수 있음.  
> `min`: 최소 값.  
> `max` : 최대 값.  
> `step` : 숫자 간격.  

```html
<input type="number" min="99" max="101">
```

#### `range`  
정확한 값이 중요하지 않는 숫자를 입력하는 창을 생성한다.  
> `<output>` : 계산이나 사용자 행동의 결과를 나타내는 요소.  

```html
<input type="range" required name="range">
<output onforminput="value=range.value">0</output>
```

#### `color`  
색상 선택 창을 생성한다.  
```html
<input type="color">
```

#### `checkbox`  
체크박스를 생성한다. 선택 항목 중에 여러개 선택이 가능하다.  
> `checked` : 기본적으로 체크로 설정하기 위한 속성.  

```html
<input type="checkbox" name="animals" value="cat" checked>
<input type="checkbox" name="animals" value="dog">
```

#### `radio`  
라디오 버튼을 생성한다. 선택 항목 중 1가지만 선택이 가능하다.  
```html
<input type="radio" name="animals" value="cat">
<input type="radio" name="animals" value="dog">
```

#### `file`  
파일 선택 창을 생성한다.  
> `multiple` : 다중 선택을 지원하는 속성.  

```html
<input type="file" multiple>
```
[ [Index로 바로가기](#index) ]

### `<select>` 요소  
드롭다운 목록을 만들 때 사용하는 요소이다. 선택항목은 `<option>`요소를 사용하여 지정한다.  
> `autofocus` : 자동으로 포커스 이동하도록 함.  
> `disabled` : 비활성화 속성.  
> `form` : `select`요소와 연관되어 있는 `form`요소의 ID.  
> `multiple` : 다중 속성 지원하는 속성.  
> `name` : 요소의 이름.  
> `required` : 옵션 선택이 필수임을 지정하는 속성.  
> `size` : 목록에서 한 번에 볼 수 있는 행의 개수를 나타내는 속성.  

```html
<select name="select">
  <option value="value1">Value 1</option>
  <option value="value2" selected>Value 2</option>
  <option value="value3">Value 3</option>
</select>
```
위에서 설명한 입력 유형들에 대한 테스트 사이트  
> [Web Forms 2.0 demo page - Mike Taylor](https://miketaylr.com/pres/html5/forms2.html)  

[ [Index로 바로가기](#index) ]  

---

## 참고 내용  

### CSS 단위  
크게 상대 단위와 절대 단위로 나누어진다.  
1. 상대 단위.  
 * `em` : 폰트의 기본 크기 값에 비례한 단위.  
 * `rem` : 최상위 요소의 폰트 크기 값에 비례한 단위.  
 * `vw` : 뷰포트 너비에 비례한 단위.  
 * `vh` : 뷰포트 높이에 비례한 단위.  
2. 절대 단위.  
 * `px` : 픽셀 단위.  
 * `in` : 인치 단위.  
 * `cm` : 센티미터 단위.  

> CSS 레퍼런스 참고시 속성 값 입력 유형  
> `<number>` : 단위가 붙지 않은 숫자.  
> `<length>` : 단위를 포함한 값.  
> `<percentage>` : 퍼센트 값.  

[ [Index로 바로가기](#index) ]  

---
### 참고 문서  
* [W3C Common input element attributes](http://html5.clearboth.org/common-input-element-attributes.html#common-input-element-attributes)  
* [[html] input type 속성 :: 지구별 안내서](http://aboooks.tistory.com/295)  
* [input type="datetime" - HTML | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/datetime)  
* [select - HTML (하이퍼텍스트 마크업 언어) | MDN](https://developer.mozilla.org/ko/docs/Web/HTML/Element/select)  
* [codepen](http://codepen.io/#)  
