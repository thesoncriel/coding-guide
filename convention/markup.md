# Markup
본 문서는 html5 를 이용한 마크업 방법을 기술한다.

기본적으로 html 을 이용한 tag 작성법을 다루며, 필요 시 참고하기 위한 약간의 css 및 js 코드가 들어갈 수 있다.

## Emmet
html 작성 시 어떤 IDE 에서든 기본적으로 emmet 을 활용 하여 작성 한다.

emmet 의 자세한 사용법은 아래를 참고 한다.

[emmet document](https://docs.emmet.io/)

## 기본 구조
emmet 문법 기준으로 ! + tab를 수행한 것을 바탕으로 작성 되며 아래와 같은 내용을 기본적으로 사용 한다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, minimum-scale=1.0, maximum-scale=1.0, user-scalable=0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>제목</title>
</head>
<body>

</body>
</html>
```

### head 내 태그 위치
* 각종 meta 태그는 title 상단에 둔다.
* link 태그는 title 하단에 둔다
* style 태그는 link 태그 아래에 두되 만약 link 가 없다면 title 아래에 둔다.
  * style 내 type 은 생략 한다.

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, minimum-scale=1.0, maximum-scale=1.0, user-scalable=0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>제목</title>
  <link rel="stylesheet" href="index.css">
  <style>
    body {
      margin: 0;
    }
  </style>
</head>
```

### script 태그 위치
* 빠른 렌더링을 위해 모든 script 태그는 body 내 최 하단에 둔다.
* jquery 같이 미리 load 되어야 할 것들을 상단에 둔다.
* 페이지별로 적용되는 업무용 script 는 async 를 붙여 준다.
* 그 외 페이지에 직접적으로 쓰여지는 internal script 는 가장 마지막 순서로 둔다.
* 모든 script 태그에는 특별한 사유가 없는 한, type 을 생략 한다.

```html
<body>
  <div>
    <h1>제목</h1>
    <p></p>
  </div>
  <script src="https://cdn.blah.com/jquery.js"></script>
  <script src="/js/page.js" async></script>
  <script>
    (function(){
      document.addEventListener('DOMContentLoaded', function() {
        document.getElementsByName('p')[0].innerHTML = 'hi there !';
      });
    })();
  </script>
</body>
```

## Semantic Structure
html5 에서는 공식적으로 시멘틱 요소를 통한 시멘틱 마크업을 지원하기에 그에 따라 기초적인 문서구조를 갖추도록 한다.
기본적인 내용은 아래 페이지의 가이드를 참조 하였다.
[www.semrush.com](www.semrush.com)

### Default
크게 헤더, 메인, 푸터 3가지로 나뉜다.

```html
<body>
  <header></header>
  <main></main>
  <footer></footer>
</body>
```

그리고 각 기본 영역은 아래와 같이 페이지 컨텐츠의 최대 폭(width)을 제한하는 기본 레이아웃인 container 를 둘 수 있다.

```html
<body>
  <header>
    <div class="container"></div>
  </header>
  <main>
    <div class="container"></div>
  </main>
  <footer>
    <div class="container"></div>
  </footer>
</body>
```

### Header
현재 사이트에서 공통적으로 쓰이거나 보여야 할 것들이 포함된다.
아래는 header에 대한 기본적인 문서 구조를 나열 한다.

```html
<header>
  <div class="container">
    <h1>사이트 제목</h1>
    <nav>
      <ul>
        <li><a href="">메뉴1</a></li>
        <li><a href="">메뉴2</a></li>
      </ul>
    </nav>
    <div class="profile"></div>
  </div>
</header>
```

* nav: 상단 네비게이션을 정의 한다. 내부에 ul > li 로 구성된 하위 목록이 포함 되어야 한다.
* h1: 해당 사이트의 이름, 제목 혹은 로고. 현재 사이트에 대한 포괄적인 제목. h1 태그는 여기에서 단 한번만 쓰도록 한다.
* .profile: 사용자 프로필에 대한 정보를 제공 한다. 프로필 이미지 및 이름, 로그인/로그아웃, 사용자 정보 수정 바로가기 등이 포함될 수 있다.

### Main
사용자에게 직접적으로 필요한 정보들을 보여준다.
아래는 Main 에 대한 기본적인 문서 구조를 나열 한다.

```html
<main>
  <div class="container">
    <article>
      <h2>페이지 제목</h2>
      <p>개략적인 내용</p>

      <section>
        <h3>영역 제목</h3>
        <p>컨텐츠 내용</p>
      </section>

      <section>
        <h3>영역 제목</h3>
        <p>컨텐츠 내용</p>
      </section>
    </article>

    <aside>
      <a href="">사이드 광고</a>
    </aside>
  </div>
</main>
```

* article: 페이지 내 사용자에게 의미 있는 컨텐츠가 포함되는 전체 영역을 정의 한다.
  * h2: 페이지의 제목을 정의 한다. 이 제목은 현재 페이지 내에서만 유효해야 한다.
    * 소제목은 h2 > small 과 같은 형식으로 덧붙인다.
* section: 컨텐츠의 영역을 정의 한다.
  * h3: 각 컨텐츠별 제목을 정의 한다. 이 제목은 현재 영역 내에서만 유효해야 한다.
    * 소제목은 h3 > small 과 같은 형식으로 덧붙인다.
* aside: 현재 컨텐츠와 무관한 것들을 정의 한다. (ex: 광고, 이벤트 등)

### Footer
사이트에서 공통적으로 쓰이는 것들이 들어가나, 대체로 header의 내용 보다는 다소 덜 중요한 것들이 들어간다.
아래는 footer 의 기본적인 구조이다.

```html
<footer>
  <div class="container">
    <section>
      <h6>제공자 이름</h6>
      <ul>
        <li>정보1</li>
        <li>정보2</li>
      </ul>
      <address>제공자 주소</address>
      <small>저작권 정보</small>
    </section>
    <nav>
      <ul>
        <li><a href="">링크1</a></li>
        <li><a href="">링크2</a></li>
      </ul>
    </nav>
    <aside>
      <a href="">하단 광고</a>
    </aside>
  </div>
</footer>
```

* section: 제공자에 대한 정보가 들어간다.
  * h6: 제공자명 (예: 회사명 등)을 기입 한다.
* ul, p, address: 제공자에 대한 기타 정보를 기입 한다.
  * small: 저작권 정보가 들어간다.
* nav: footer 에 들어가는 각종 부가적인 링크가 포함된다. (ex: 개인정보취급방침 등)
* aside: 최하단에 출력되는 광고를 정의 한다. (ex: 하단에 따라다니는 배너 등)

## Forms
사용자 입력을 받아서 처리하는 UI는 모두 기본적으로 form 요소로 감싸준다. 또한 기본적으로 submit button 을 form 내부에 둠으로써 input 요소에서 입력 후 데스크탑 기준, enter 키를 치면 자동적으로 onSubmit 이벤트가 동작되도록 한다.

모바일은 각 가상 키보드 내 다음, 확인 등의 입력 완료 키가 상이하나 submit button 을 둠으로써 동일한 동작을 기대할 수 있다.

아래는 form 요소에 대한 기본적인 구조로써 inline event 는 예시로서 참고 하도록 한다.

form 요소의 onSubmit 이벤트가 처리되는 자세한 내역은 각 framework 및 library 를 참조 한다.

```html
<form onsubmit="eventMethod()">
  <div class="input-group">
    <label for="txt_name">이름</label>
    <input id="txt_name" type="text" name="name">
  </div>
  <button type="submit">보내기</button>
</form>
```

* form: 해당 요소를 이용하여 사용자에게 입력 받은 후 처리되는
  * action 은 일반적으로 ajax 를 통한 비동기로 이뤄지므로 action 속성은 생략 한다.
* .input-group: label 과 input 을 묶어 주는 역할.
  * label: 해당 요소의 명칭. 어떤 것을 위한 레이블인지 명시하기 위해 for 속성을 채워준다. 단, 두개 이상의 입력을 가리킬 경우 가장 가까운 텍스트 입력 요소를 가르킨다.
  * input, select, textarea 등 각종 입력 요소들. id 와 name 은 반드시 채워주도록 한다.
    * 입력 요소들에 대한 id 명칭 규칙은 하단을 참조.
    * name 은 사용자 입력이 완료된 후 내부 business login 에 쓰이는 Object 의 Property Name 으로 지정 한다.
* button[type=submit]: form 요소의 onSubmit을 일으키기 위한 수단. 사용자가 직접 클릭할 수도 있고, 보이진 않으나 enter 키로 유도할 수도 있으므로 반드시 넣어 준다.

### input ID naming
입력 요소들에 대한 id 명명 규칙이다. 기본적인 규칙은 다음과 같다.
타입_inputNameForFuture

즉 타입을 좌측에둔 뒤 언더바(_)를 붙인 후 뒷쪽은 name 속성값을 따르되 camel-case 로 작성 한다.

| type                 | prefix | examples |
|:---------------------|:-------|:---------|
| input[type=text]     | txt    | txt_name, txt_docTitle |
| input[type=search]   | txt    | txt_search, txt_findUser |
| input[type=number]   | num    | num_age, num_dataCount |
| input[type=date]     | date   | date_birth, date_createdAt |
| input[type=email]	   | email  | email_id, email_userEmail |
| input[type=password] | pw	    | pw_password, pw_userJumin |
| input[type=file]     | file   | file_doc, file_memberInfo |
| input[type=checkbox] | chk    | chk_opt, chk_selectValue |
| input[type=radio]	   | rad    | rad_choose, rad_prodOne |
| select               | sel    | sel_region, sel_companyName |
| textarea             | txta   | txta_content, txta_boardCont |

참고로 input text 및 search 는 사실상의 차이가 없다. 다만, CSS 후속 작업에 도움이 될 수 있을 것이다.

[input type=“text” vs input type=“search” in HTML5](https://stackoverflow.com/questions/11589770/input-type-text-vs-input-type-search-in-html5)

### Radio Buttons & Check Boxes
라디오 버튼 및 체크박스 같이 어떤 것이 체크된 내용에 대하여 마크업할 때는 그 요소의 체크 상태에 따라 스타일을 다르게 적용할 수 있도록 고려 해야 한다.

아래는 그 예시 이다.
```html
<!-- 체크박스 -->
<label for="chk_opt0" class="chk-wrap">
  <input type="checkbox" name="opt0" id="chk_opt0">
  <span>
    <i class="icon icon-check"></i>
    <i class="icon icon-check active"></i>
    <span class="label">옵션1</span>
  </span>
</label>
<label for="chk_opt1" class="chk-wrap">
  <input type="checkbox" name="opt1" id="chk_opt1">
  <span>
    <i class="icon icon-check"></i>
    <i class="icon icon-check active"></i>
    <span class="label">옵션1</span>
  </span>
</label>

<!-- 라디오버튼 -->
<label for="rad_opt1" class="chk-wrap">
  <input type="checkbox" name="opt" id="rad_opt1" value="val1">
  <span>
    <i class="icon icon-check"></i>
    <i class="icon icon-check active"></i>
    <span class="label">옵션1</span>
  </span>
</label>
<label for="rad_opt2" class="chk-wrap">
  <input type="checkbox" name="opt" id="rad_opt2" value="val2">
  <span>
    <i class="icon icon-check"></i>
    <i class="icon icon-check active"></i>
    <span class="label">옵션2</span>
  </span>
</label>
```

```scss
// SCSS 혹은 Styled-Component
.chk-wrap {
  display: inline-block;

  & > input {
    position: absolute;
    visibility: hidden;

    & + span {
      & > .active {
        display: none;
      }
    }

    &:checked {
      & + span > i {
        display: none;

        &.active {
          display: inline-block;
        }
      }
    }
  }
}
```

* label: 사용자의 클릭/터치 액션을 받아들이는 영역을 정의 한다.
  * input: 실제 액션이 구동되는 입력 요소. 별도 스타일을 적용시키기 어려우므로 __css의 position 및 visibility 를 이용__ 하여 숨긴다.
    * checkbox 일 경우 name 을 다르게 준다. (뒤에 순서대로 index를 붙임)
    * radio 일 경우 name 을 동일하게 주되 value 값을 넣어 준다.
  * span: input 의 체크 상태에 따라 실제 input 요소 대신 보여야 될 내용.
    * i.icon: 스프라이트 아이콘을 출력
    * span.label: 레이블 텍스트를 출력

## Tables
테이블은 웹표준에 따라 표형태의 데이터를 표현 할 때만 사용토록 한다.

이용 시 기본적인 구조.

```html
<table>
  <thead>
    <tr>
      <th>항목1</th>
      <th>항목2</th>
      <th>항목3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>행1 값1</td>
      <td>행1 값2</td>
      <td>행1 값3</td>
    </tr>
    <tr>
      <td>행2 값1</td>
      <td>행2 값2</td>
      <td>행2 값3</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td colspan="3">요약: 값N</td>
    </tr>
  </tfoot>
</table>
```

* table 내 thead 및 tbody 로 반드시 헤더와 표 내용을 나누도록 한다.
* tfoot 은 필요 시 넣어준다.

만약 Row Header가 필요하다면 다음과 같이 작성한다.

```html
<table>
  <thead>
    <tr>
      <th></th>
      <th>상반기</th>
      <th>하반기</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">11월</th>
      <td>12</td>
      <td>13</td>
    </tr>
    <tr>
      <th scope="row">12월</th>
      <td>14</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
```

## Images
컨텐츠 이미지 출력은 **img** 태그를 이용한다.
이 때 alt 속성은 반드시 채워준다.

대체로 이미지 크기는 반응형(responsiblity)으로 작성을 많이 하므로,
특수한 상황이 아니라면, 해당 크기는 css 로 결정토록 한다.

### Figures
이미지에 부가설명등을 기입할 때는 **figure** 와 **figcaption** 을 이용한다.
```html
<figure>
  <img src="이미지.jpg" alt="이미지 설명">
  <figcaption>오징어가 헤엄치는 모습</figcaption>
</figure>
```

### Image Links
이미지에 단순 링크를 다는 것은 단순히 a 태그를 감싸주면 된다.

다만, 캡션이 붙어야 할 경우엔 다음과 같이 작성한다.
```html
<figure>
  <a href="">
    <img src="" alt="">
  </a>
  <figcaption>설명</figcaption>
</figure>
```

혹은 이미지에 오버렙(Overwrap) 되는 텍스트를 함께 표현할 경우엔 아래를 따른다. (스타일은 생략)
```html
<a href="">
  <figure>
    <img src="" alt="">
    <figcaption>이미지 위에 표현되는 설명</figcaption>
  </figure>
</a>
```

## Icons
이미지 태그는 <i> 태그로 통일하며, <i> 태그는 반드시 아이콘 관련 내용이어야만 한다.

태그로 아이콘을 적용시키는건 아래와 같이 크게 2가지 이다.

  1. 이미 만들어진 CSS Sprite 를 적용
  2. 외부 icon font를 이용할 때 (ex: font awesome 등)

```html
<!-- CSS Sprite 이용 -->
<i class="icon icon-box"></i>
<!-- Font Awesome 이용 -->
<i class="fa fa-box"></i>
```

## Lists
목록은 기본적으로 ul > li 를 통하여 구성한다.  네비게이션이나 디자인시안 상 목록처럼 보이는 내용도 모두 ul > li 로 구성 한다.  단, 순서가 있을 경우 ol > li 로 구성한다.

목록 구조가 **용어**와 **설명** 식일 경우 dl > dt + dd 로 구성한다.

```html
<dl>
  <dt>용어1</dt>
  <dd>그 것에 대한 설명들</dd>
  <dt>용어2</dt>
  <dd>그 것에 대한 설명들 블라블라</dd>
</dl>
```

단, 행(Row) 구조가 별도로 필요하다면 다음과 같이 ul > li > b + span 으로 구성한다.

```html
<ul>
  <li>
    <b>용어1</b>
    <span>
      그 것에 대한 설명들
    </span>
  </li>
  <li>
    <b>용어2</b>
    <span>
      그 것에 대한 설명들 블라블라
    </span>
  </li>
</ul>
```

### Input Lists
입력란을 목록 형태로 출력해야 할 때는 다음을 따른다.

```html
<ul class="input-list">
  <li>
    <label for="txt_first">입력1</label>
    <div class="input-wrap">
      <input id="txt_first" type="text">
    </div>
  </li>
  <li>
    <label for="txt_second">입력2</label>
    <div class="input-wrap">
      <input id="txt_second" type="text">
    </div>
  </li>
</ul>
```

* .input-list: 입력란 목록에 대하여 정의 한다.
* .input-wrap: label 과 input 에 대하여 단(column)을 나눌 일이 있을 때 사용한다. 그와 더불어 반응형으로 스타일 적용 시 조건에 따라 단을 없애는 등의 유동적인 사용이 가능하다.

## Typography
### 텍스트 내 특정 내용 강조 (Emphasized) 할 때

일반적으론 **strong** 태그를 이용한다.

```html
<p>
  블라블라 쓰다가 <strong>이것</strong>이 강조됨.
</p>
```

다만 strong 보다 덜 눈에 띄는 약한 강조는 **em** 태그를 이용한다.

```html
<p>
  이렇게 저렇게 쓰다가 약간 <em>눈에 띄게</em> 해 봅시다.
</p>
```

### BR 요소 없이도 줄바꿈(Line Break)이 필요할 때
아래와 같이 pre 태그를 이용한다.

```html
<pre>엔터가 자동으로 적용되어야 할 문장.
줄바꿈 됩니다.</pre>
```

다만 pre 요소의 기본적인 CSS 를 적용할 경우, 자동 줄바꿈이 안될 것이다. 이 때 아래와 같은 스타일을 적용한다.

```css
pre {
  white-space: pre-line;
}
```

### 삭제선 삽입
del 태그를 이용한다.

```html
<p>
  현재 가격 <del>6500</del> 5900원! 놀라운 가격!
</p>
```
