# JavaScript

자바스크립트 (이하 JS)를 다룸에 있어서 기본적으로 알고 있어야 할 지식을 서술 한다.

## 알아보기

문서를 읽기 전 미리 확인하고 알아두면 좋은 페이지들을 링크 해 두었다.

  * [JavaScript Garden](http://bonsaiden.github.io/JavaScript-Garden/ko/)
  * [자바스크립트 최적화: 응답성 좋은 인터페이스](https://jicjjang.github.io/2017/05/19/javascript-optimize-6/)
      * setTimeout vs setInterval 에서 setTimeout 만 써야하는 이유에 대하여 볼 것

### 생각해 보기

  * bind, apply, call 의 차이점은 ?
  * typeof 와 instanceof 를 적절히 사용하는 방법은?
  * JS(ES5) 는 scope 가 몇 종류나 될까?
  * DTO 역할을 맡은 객체 내 key value pair 를 반복문으로 확인하는 방법은 ?
  * 확인한 내용과 eslint 혹은 tslint 와의 연관성은?

### 꼭 지킵시다

    * 코드 작성 시 tab space 는 공백 2글자로 사용한다.

## Polyfill
모든 웹브라우저가 ECMAScript5 (이하 ES5) 혹은 ECMAScript6 (이하 ES6) 내용을 지원하지는 않는다.

과거에는 모든 polyfill 코드를 넣고, prototype 을 검사하여 없으면 해당 기능을 확장(extend) 하였으나, 이럴 경우 사용자에게 전달되는 bundle 크기가 늘어나는 단점이 있어, 필요한 것만 선택적으로 추가 하게 되었다.

하지만 이 역시 이미 많은게 갖춰진 최신 웹브라우저 에서는 의미없는 행위이기에 아래와 같이 [polyfill.io](https://polyfill.io/v3/) 에서 제공되는 내용을 이용 한다.

```html
<html>
  <head></head>
  <body>
    <script crossorigin="anonymous" src="https://polyfill.io/v3/polyfill.min.js?features=default%2CString.prototype.padStart"></script>
  </body>
</html>
```

참고로 위 내용은 [polyfill bundle 생성기](https://polyfill.io/v3/url-builder/) 기준으로, default 와 String.prototype.padStart 메서드를 추가하는 내용이다.

### UA 방식에 대한 고찰

polyfill.io 는 User-Agent 정보를 바탕으로 웹브라우저가 지원하는 API 여부를 확인해서 필요한 것만 전달한다.

그 것을 체크하여 전달하는 방법에 대해서는 다음 블로그 글을 참고 한다.

  * [Polyfill을 사용하는 보다 쉬운 방법](http://hacks.mozilla.or.kr/2014/12/an-easier-way-of-using-polyfills/)
  * [Smarter polyfill loading with polyfill.io](https://gomakethings.com/smarter-polyfill-loading-with-polyfill-io/)

## Function Size

프로그램의 기능 및 동작을 담당하는 함수, 메서드(Method)의 **코드 길이는 한 화면을 넘기지 않도록** 한다.

한 화면의 기준은, 15인치 맥북 화면을 기준으로 한다.

vscode 를 맥북 메인 화면에 옮기고 화면 확대/축소를 하지 않았다 가정 했을 때 **약 50 라인** 이다.

만약 코딩 진행 후 그 높이를 넘긴다면, 리팩토링을 고민 해 보아야 할 것이다.

아래는 관련된 내용에 대해 답변이 들어 온 스택오버플로우 질문이다.

  * [What Should be the maximum length of a function](https://softwareengineering.stackexchange.com/questions/27798/what-should-be-the-maximum-length-of-a-function)

## Immutable

불변성(Immutable)은 예상치 못한 Side Effect 를 줄이기 위한 기법 중 하나이다.

JS SW 내 모든 객체(Object)는 참조형(Reference Type) 으로 동작되므로 이를 그대로 전달 할 경우 외부에서 예상치 못한 변형이 가해질 수 있다.

따라서 아래와 같은 경우, 객체를 복사 하여 전달 해야 한다.

  * 현재 클래스의 property 들을 참고 하기 위해 해당 인스턴스를 통째로 넘길 때
  * effect module 을 통해 데이터가 변경 될 때
  * 이벤트나 콜백으로 자기 자신의 데이터를 전달 할 때

이 때는 필요한 property 만 묶어서 객체 (혹은 인터페이스)로 넘긴다.

### obejct assign vs spread

불변성을 수행함에 있어 Object.assign 혹은 spread (스프레드) 문법을 활용한다.

이 때 어느 것을 쓸지는 논란의 여지가 있으니 아래 내용을 먼저 확인 하도록 한다.

  * [MDN - spread 문법](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
  * [Object.assign vs Object Spread (영문)](https://thecodebarbarian.com/object-assign-vs-object-spread.html)

요약하자면

  * Object.assign 은 setter property 에 대한 trigger 를 일으킬 수 있다.

수행 속도등을 검증한 내용도 있는데 속도 및 문법 간소화 측면에 있어 객체 복사엔 spread 문법을 권장한다.

아래는 기존 객체가 가진 프로퍼티에 값을 설정함에 있어 객체로 보낼 때의 예제이다.

이렇게 하면 상기에 설명한 대로 setter property 에 대한 trigger 를 수행 할 수 있다. 다만 이렇게 쓸 일이 없으므로 권장하지 않는다.

```js
// 일반적인 불변성을 위해선 권장하지 않음
const obj = {
    _haha: '',
    set haha(val) {
      console.log('set haha', val);
      this._haha = val;
    },
    get haha() {
      return this._haha;
    }
  };

  obj.haha = '하하하!';

  console.log(obj.haha); // 하하하!

  const obj2 = Object.assign(obj, { haha: '웃자!'} );

  console.log(obj2.haha); // 웃자!
  console.log(obj === obj2); // true
```

## 반복문 안에서의 비동기 및 클로저

반복문을 사용하면서 그 안의 index 를 매 반복마다 다르게 쓰려고 다음과 같이 작성하는 경우가 있다.
```js
for (var i = 0; i < 10; i++) {
  setTimeout(() => console.log(i), 200);
}
```

당연히 결과는 뜻대로 되지 않는다. 그래서 다음과 같이 Function Scope 를 만들어서 수행하게 된다.

```js
for (var i = 0; i < 10; i++) {
  (idx =>  setTimeout(() => console.log(idx), 200))(i);
}
```

하지만 이럴 경우엔 아래와 같이 함수를 별도로 만들어서 사용한다.
```js
function log(i) {
  setTimeout(() => console.log(i), 200);
}

for (var i = 0; i < 10; i++) {
  log(i);
}
```

만약 반복문의 주체가 Array 라면 아래와 같이 forEach 를 이용하고, 각 요소의 내용을 변경 할 때는 map 을 이용한다.
```js
const arr = [1,2,3,4,5,6,7,8,9,10];

function log(i) {
  setTimeout(() => console.log(i), 200);
}

// do not this !!
for (var i = 0; i < arr.length; i++) {
  log(i);
}

// good
arr.forEach((item, idx) => log(idx));

// data manipulation
const aRemake = arr.map((item, idx) => item * 2);
```

위와 같이 반복문의 주체가 HashMap 혹은 Array 와 같은 Collection 일 경우, JS API 에서 기본적으로 제공되는 기능을 적극 활용하여 작성토록 한다.

그 것이 성능적으로 좋을 뿐만 아니라 코드 가독성도 높아진다.
