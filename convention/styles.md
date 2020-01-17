# Styles

본 문서는 CSS3 를 이용한 웹문서 스타일 작성 방법을 기술한다.

웹프론트엔드 프로젝트에 쓰이는 CSS Preprocessor 는 SCSS 및 Styled-Components 2가지로 한정한다.

따라서 본 문서는 기본적으로 SASS (정확히는 SCSS) 문법을 따르며, 필요에 Styled-Components (이하 SC) 문법을 표기하고 있다.

SC의 예제는 TypeScript로 표기하며, 그에 따라 필요한 SW적 내용이 포함될 수 있다.

SASS 및 Styled Components 문법은 아래를 참고 한다.
* [SASS 문법](https://sass-lang.com/guide)
* [Styled Components 문법](https://www.styled-components.com/docs/basics)

## OOCSS
Object Oriented CSS 즉 객체지향 CSS 로서 컴포넌트 기반 개발(Component Based Development - CBD)에 어울리는 스타일 작성법이다.

아래는 참고 링크. 미리 확인하면 좋을 것이다.

* [객체지향 CSS 소개](https://mytory.net/archives/8949)
* [Object Oriented CSS (Slideshow)](http://www.slideshare.net/stubbornella/object-oriented-css)
* [다양한 CSS 네이밍 기법 소개 (OOCSS 만 참고)](https://wit.nts-corp.com/2015/04/16/3538)

기본적으로 클래스명 및 응용 방법은 OOCSS 기법을 응용하되 이후 언급되는 내용들을 덧붙여 적용 시키도록 한다.

## Class Naming
[CSS Class](/naming/css-class) 를 참고 한다.

## 속성 작성 순서
CSS 내에 기술되는 속성은 매우 많다. 그 중 중요도 및 작성 순서는 다음과 같다.

  1. 출력되는 컨텐츠
      * content
  2. 배치 위치
      * position
      * top, left, right, bottom
      * z-index
  3. 표현 방법
      * display
      * align-items, align-content 등 flex box 관련 배치법
      * align-self, flex-grow, flex-shrink 등 flex item 관련 배치법
  4. Box Model 에 따른 크기
      * 방향별 작성 순 = top|side(left|right)|bottom
      * width, height
      * min-width, max-width, min-height, max-height
      * padding
      * border
        * border-radius
      * margin
      * box-sizing
      * overflow
  5. 형태 변환 (순서 없음)
      * transition
      * transform
      * animation
      * opacity
      * filter
  6. 정렬 (순서 없음)
      * float
      * text-align
      * vertical-align
  7. 텍스트 여백 (순서 없음)
      * line-height
      * word-spacing
      * letter-spacing
  8. 색상
      * background
      * color
  9. 기타 텍스트 및 폰트 설정 (순서 없음)
      * font-size, font-family, font-weight
      * text-decoration
      * [white-space](https://aboooks.tistory.com/187)
      * text-overflow
      * 그 외

상기 제시된 순서에 따라 작성된 예시 (일부 과장된 속성이 있음)

```scss
.class-name {
  // 출력되는 컨텐츠
  content: '★';
  // 배치 위치
  position: absolute;
  top: 1px;
  left: 2px;
  right: 3px;
  bottom: 4px;
  z-index: 10;
  // 표현방법
  display: block;
  align-items: center;
  align-self: center;
  // Box Model
  width: 300px;
  height: 200px;
  min-width: 200px;
  max-width: 400px;
  min-height: 100px;
  max-height: 300px;
  padding: 10px 20px;
  border: 5px solid #ccc;
  border-radius: 5px;
  margin: 10px 20px;
  overflow: hidden;
  // 형태변환
  transition: width 1s linear;
  transform: scale(1.2, 0.9);
  animation: ani-haha 1s;
  opacity: 0.8;
  filter: grayscale(0.2);
  // 정렬
  float: left;
  text-align: center;
  vertical-align: middle;
  // 텍스트 여백
  line-height: 1;
  word-spacing: 1em;
  letter-spacing: 1em;
  // 색상
  background: #888;
  color: #333;
  // 기타
  font-size: 15px;
  font-weight: bold;
  text-decoration: underline;
  white-space: nowrap;
  text-overflow: ellipsis;
}
```

### 선택자 배치 순서

CSS 에서는 다양한 선택자 조합이 있으며 이에 대한 순서는 다음을 지키도록 한다.

  1. 자기 자신 속성들 (self properties): 현재 클래스에서 가장 중요한 것이므로 최상단에 위치.
  2. 의사 요소 (pseudo element): 실체가 없지만 본체와 별도로 스타일을 줄 수 있다.
  3. 속성 선택자 (attribute selectors): 현재 요소의 속성값에 따라 다르게 스타일을 준다.
  4. 의사 클래스 (pseudo class): 여러가지 상태와 부모 요소 대비 순서등으로 특별한 스타일을 줄 수 있다.
      - 부모 요소 대비 특정 자식 요소인지 확인
      - 입력 요소의 상태
      - 링크 및 마우스 위치/선택 상태
  5. 형제 선택자 (sibling selector): 주변 요소에 스타일을 줄 수 있다.
  6. 자식 선택자 (child selector): 자식 요소에 스타일을 적용 한다.
  7. 자손 선택자 (descendant selector): 자손 요소에 스타일을 적용 한다.

```scss
.styles {
  // 1. self properties
  display: block;

  // 2. pseudo element
  &::before { }
  &::after { }
  &::placeholder { }
  &::first-letter { }

  // 3. attribute
  &[type=check] { }
  &[data-ga^=ddocdoc] { }

  // 4. pseudo class
  // 4.1. 부모 요소 대비 특정 자식 요소인지 확인
  &:first-child { }
  &:last-child { }
  &:nth-child(1) {}
  // 4.2. 입력 요소의 상태
  &:checked { }
  &:disabled { }
  // 4.3. 링크 및 마우스 위치/선택 상태
  &:link { }
  &:hover { }
  &:focus { }

  // 5. siblings
  & + div { }
  & ~ .bbros { }

  // 5. childrens
  & > span { }
  & > .wrap { }

  // 6. descendants
  img { }
  .list { }
}
```

단, 부득이하게 cascading 원리에 따라 스타일 규칙이 다른 것에 비해 앞서야 한다면 가장 하단에 두도록 한다.

### 미디어 쿼리 삽입

다양한 device 에 맞추어 반응형(responsibility)으로 스타일을 적용함에 있어 미디어쿼리는 필수다.

다음은 width=800px 미만이 모바일 기기라는 가정하에 종류별 작성 순서표 예시다.

| name  | min-width | max-width | device |
|:------|:----------|:----------|:-------|
| Tablet ~ Desktop | 800px | | PC, iPad |
| All Mobile | 0px | 799px | |
| Mobile Medium | 360px | 799px | iPhoneX, Galaxy6 |
| Mobile Small | 0px | 359px | iPhone5, Galaxy3 |

모바일은 크게 360px 기준으로 Small 및 Medium 2가지로 구분되며, 그 기준이 되는 기기는 iPhone6 로써 Rendering Width 가 375px 이다.

웹디자인 시 대체로 mobile first 로 진행되기 때문에 Tablet 은 Desktop 으로 분류하며 이 이상 상세히 구분하진 않는다.

미디어 쿼리 삽입 시 아래와 같이 적용되는 속성 영역 하단에 둔다.

```scss
.selector {
  display: block;
  color: red;

  // Desktop
  @media screen and (min-width: 800px) { }

  // Mobile
  @media screen and (max-width: 799px) { }

  // Mobile Medium
  @media screen and (min-width: 360px) and (max-width: 799px) { }

  // Mobile Small
  @media screen and (max-width: 799px) { }

  // Print
  @media print { }
}
```

### 믹스인 삽입
SCSS 의 mixin 이나 미리 정의된 중복된 코드를 삽입 시 아래와 같이 해당 block 내 최상단에 둔다.

```scss
// 어디선가 선언된 믹스인
@mixin mxBlockElement() {
  display: block;
  border: 1px solid #333;
}

// 믹스인 사용부
.styled-wrap {
  @include mxBlockElement();
  position: relative;
  margin: 4px;
}
```

Styled Components 일 경우는 다음과 같다.

```ts
// 어디선가 선언된 믹스인
const mxBlockElement = css`
  display: block;
  border: 1px solid #333;
`;

// 믹스인 사용부
const StyledWrap = styled.div`
  ${mxBlockElement}
  position: relative;
  margin: 4px;
`;
```

### 미디어 쿼리를 믹스인으로 삽입

미디어 쿼리 그 자체로는 코드에 적용 시 가독성이 떨어지기 때문에 아래와 같이 믹스인을 적용시키도록 한다.

삽입 시 자기 자신 속성 아랫쪽에 위치 시키도록 한다.

```scss
@mixin mxBreakpoint-desktop() {
  @media screen and (min-width: 800px) {
    @content
  }
};

@mixin mxBreakpoint-mobile() {
  @media screen and (max-width: 799px) {
    @content
  }
};

@mixin mxBreakpoint-mobileMedium() {
  @media screen and (min-width: 360px) and (max-width: 799px) {
    @content
  }
};

@mixin mxBreakpoint-mobileSmall() {
  @media screen and (max-width: 799px) {
    @content
  }
};

// 사용 예제
.ex {
  display: block;

  // 삽입 시 자기 자신 속성 아랫쪽에 위치 시킨다.
  @include mxBreakpoint-desktop() { }
  @include mxBreakpoint-mobile() { }
  @include mxBreakpoint-mobileMedium() { }
  @include mxBreakpoint-mobileSmall() { }
}
```

Styled Components 는 다음과 같다.

```ts
function getMediaQuery(min: number, max?: number) {
  return `@media screen and (min-width: ${min}px)${max ? ' and (max-width: ' + max + 'px)' : ''}`;
}

/**
 * 화면 사이즈별 미디어 쿼리를 적용 시킨다.
 */
export const mxBreakpoint = {
  /**
   * 데스크탑 미디어쿼리. 800px 이상
   */
  desktop: getMediaQuery(800),
  /**
   * 모바일 미디어쿼리. 0 ~ 799px
   */
  mobile: getMediaQuery(0, 799),
  /**
   * 모바일 미디어쿼리 - 화면 큰 것들 (ex: 갤럭시S5, 아이폰6)
   */
  mobileMedium: getMediaQuery(360, 799),
  /**
   * 모바일 미디어쿼리 - 화면 작은 애들 (ex: 아이폰4~5)
   */
  mobileSmall: getMediaQuery(0, 359),
  /**
   * 세로로 긴 것들 (ex: 아이폰X)
   */
  longHeight: '@media screen and (min-height: 700px)',
};

// 사용 예제
const Wrap = styled.div`
  display: block;

  ${/* 삽입 시 자기 자신 속성 아랫쪽에 위치 시킨다. */}
  ${mxBreakpoint.desktop} { }
  ${mxBreakpoint.mobile} { }
  ${mxBreakpoint.mobileMedium} { }
  ${mxBreakpoint.mobileSmall} { }
`;
```

## Selector Specificity
선택자 명시도는 아래와 같은 선택자 유형에 따라 점수가 매겨지고 우선순위가 정해지는 것을 의미 한다.

다음을 먼저 참고 하자.

  * [CSS Specificity](https://www.w3schools.com/css/css_specificity.asp)
  * [CSS 선택자 이해](http://www.nextree.co.kr/p8468/)

아래는 명시도 점수를 도표로 요약 한 것이다.

| selector | score | note |
|:---------|:------|:-----|
| !important | 10000 | 약속된 유틸리티 외엔 **절대** 쓰지 않는다. |
| inline style | 1000 | 가급적 자제 한다. |
| #id | 100 | class name 으로 대체 한다. |
| .class | 10	| |
| tagname	| 1 | |
| universal | 0 | css 초기화 때 외에는 쓰지 않는다. |

주의해야 할 점은 위 표에서 보시다시피 **!important** 는 반드시 꼭 필요한 곳에서만 써야하고 가급적 쓰지 말아야 할 속성이다.
의도치 않게 마구 덮어씌워 side effect 를 발생시키는 주범이 될 수 있기 때문이다.

아래는 선택자 조합 시 발생되는 명시도 점수 예시이다.

```scss
* {} // 0점
div + p {} // 2점
.spec {} // 10점
#wrap > .spec {} // 110점
#wrap { background: red !important; } // ※주의: 무조건 덮어 씌운다!
```

## Selector Optimization
웹브라우저에서 웹문서를 CSS와 함께 렌더링 시, 선택자에 따라 렌더링 속도와 그에 따른 UX가 달라지기에 선택자 작성 시 항상 최적화 할 수 있는 방법을 이용한다.

아래 내용을 먼저 확인 한다.

  * [효율적인 CSS 작성하기](https://webclub.tistory.com/361)

요약하자면..

  * Tag 및 Class 규칙을 이용할 것.
  * 자손(Descendant) 선택자 보다 자식(Child) 선택자를 사용 할 것.
  * descendant selector: div p
  * child selector: div > p
  * ID, class 및 tag 외 attribute 나 전체 (*) 선택자와 같은 universal selector 는 자제 한다.
      * 단 attribute selector 는 tag 및 class selector 와 연동하여 사용하는 것은 권장 된다.

가급적 class name 하나로 특정해서 사용하되 tag 를 이용할 시 child(>), adjacent sibling(+), general sibling(~) 등과 같이 특정지을 수 있도록 한다.

```scss
// bad
.wrap {
  color: #333;
  div {
    color: #ccc;
  }
}
// good
.wrap {
  color: #333;
  & > div {
    color: #ccc;
  }
}
// best
.wrap {
  color: #333;
  & > .inner-wrap {
    color: #ccc;
  }
}
```

### Styled Components 사용 시

SC는 렌더링 되면 컴포넌트 태그 자체적으로 class 를 만들어서 적용된다. 따라서 template string 내부 스타일에 하위 태그, 혹은 클래스를 선택자로 가리킬 때는 아래와 같이 사용한다.

```ts
// bad
const Wrap = styled.div`
  color: #333;
  div {
    color: #ccc;
  }
`;
// good
const Wrap = styled.div`
  color: #333;
  & > div {
    color: #ccc;
  }
`;
// best
const Wrap = styled.div`
  color: #333;
  & > .inner-wrap {
    color: #ccc;
  }
`;
```

## Nesting Depth

하위 요소에 대한 선택자를 구성 시 최대 3개를 초과하지 않도록 한다.

SCSS 에서 클래스명 작성 시 아래와 같이 중첩(nesting) 될 수 있다.

```scss
.nesting-wrap {
  background: #fff;
  & > .child-wrap {
    background: #f80;
    & > .under-wrap {
      background: #f00;
    }
  }
}
```

이 것은 아래와 같이 compile 될 것이다.

```css
.nesting-wrap {
  background: #fff;
}
.nesting-wrap > .child-wrap {
  background: #f80;
}
.nesting-wrap > .child-wrap > .under-wrap {
  background: #f00;
}
```

이전에 강조했듯이, 자손(descendant) 선택자 대신 자식(child) 선택자를 쓰기를 권장한다.

하지만 다음 예제와 같이 마크업 복잡도가 늘어나게 될 경우 Compile 된 CSS 코드량도 늘어나게 되므로 자손(child) 선택자가 불가피하게 필요한 경우가 발생될 수 있다.

```html
<div class="wrap">
  <div class="list">
    <div class="item">
      <h4>아이템 제목</h4>
      <div class="right-wrap">
        <figure>
          <a href="">
            <img src="" alt="이미지" class="image">
          </a>
          <figcaption>이미지 설명</figcaption>
        </figure>
      </div>
    </div>
  </div>
</div>
```

```scss
// 네스팅이 많아짐에 따라 코드가 많아짐
.wrap {
  & > .list {
    & > .item {
      & > .right-wrap {
        & > figure {
          & > a {
            & > .image {
              display: block;
            }
          }
        }
      }
    }
  }
}

// 좀더 간결하게..
.wrap {
  .image {
    display: block;
  }
}
```

애초에 저렇게 Depth 가 깊어지고 복잡해진다면 Refactoring 을 통해 컴포넌트 분할을 하는 것이 옳다.

아래는 React 일 경우의 예제이다. (개략적으로 구분된 것이니 참고만 한다.)

```tsx
import React, { FunctionComponent as FC } from 'react';
import styled from 'styled-components';

interface FigureProps {
  href: string;
  src: string;
  caption: string;
}

const FigureWrap = styled.figure`
  & > a {
    & > .image {
      display: block;
    }
  }
`;

const Figure: FC<FigureProps> = props => (
  <FigureWrap>
    <a href={props.href}>
      <img src={props.src} alt="이미지" className="image" />
    </a>
    <figcaption>{props.caption}</figcaption>
  </FigureWrap>
);

interface ItemProps extends FigureProps {
  title: string;
}

const ItemWrap = styled.div`
`;

const Item: FC<ItemProps> = props => (
  <ItemWrap>
    <div class="item-wrap">
      <h4>{props.title}</h4>
      <div class="right-wrap">
        <Figure {...props} />
      </div>
    </div>
  </ItemWrap>
);

interface ListProps {
  items: ItemProps[];
}

const ListWrap = styled.div`
`;

const List: FC<ListProps> = props => (
  <ListWrap>
    <div className="list">
    {props.items.map((item, idx) =>
      <Item key={idx} {...item} />
    )}
    </div>
  </ListWrap>
);
```

## Do Not This

아래와 같은 코딩은 하지 않도록 한다.

### 네스팅을 이용하여 클래스명을 나누기

SCSS 나 Styled Components 의 네스팅 기능을 이용하여 클래스명을 나누는 경우가 있는데, 이럴 경우 해당 클래스명을 코드에서 찾을 때 번거로워 질 수 있으므로 하지 않는다.

```scss
// wrong
.object {
  &-value {
    &-checking {
      // attrs..
    }
  }
  &-setting {
    &-disabled {
      // attrs..
    }
  }
}

// good
.object-value-checking {
  // attrs..
}
.object-setting-disabled {
  // attrs..
}
```

클래스명에 대시(-) 를 2번 이상 넣지 말기

보통 대시를 2번 이상 넣는 것은 다음과 같은 경우를 말한다.

```scss
.object-value--disabled {
  // attrs..
}
.object-value {
  &--disabled {
    // attrs..
  }
}
```

대체로 BEM (Block Element Module) 작성 명칭을 이용한 것인데, 상태에 따라 필요한 클래스와 그 클래스명이 매우 길어진다.

또 한 상태(state)는 따로 개별 클래스로 사용하므로 저렇게 쓰지 않는다.

아래는 상기 예시에 대하여 올바르게 쓰는 방법이다.

```scss
.object-value {
  &.disabled {
    // attrs..
  }
}
```

## 읽어보기
### Cross Browsing

CSS3 는 Device 및 Web Browser 에 따라 지원여부가 다르다. 자주 확인되는 내용은 아래 링크를 참고 한다.

  * [Flexible Box](https://caniuse.com/#feat=flexbox)
      * Styled Components 를 이용할 시 자동으로 vender prefix 를 적용 시켜준다.
  * [Filter Effects](https://caniuse.com/#search=filter)
      * IE 및 Edge 는 안된다 봐야 한다. 단, MS 가 예고 했듯이 Edge 만큼은 Blink 기반으로 변경 되면 잘 적용 될 것이라 보인다.

### H/W Acceleration

HTML5 및 CSS3 이용 시 아래와 같은 속성을 사용하면 H/W 가속을 적용시켜 더 부드러운 렌더링 및 반응성을 이끌어낼 수 있다.

  * css3 의 translate, transform, perspective, filter, animation 속성 적용
  * video 또는 canvas 요소

더 자세한 내용은 아래 url 을 통해 확인 한다.

  * [Naver D2 - 하드웨어 가속에 대한 이해와 적용](https://d2.naver.com/helloworld/2061385)
  * [CSS GPU 애니메이션 제대로하기](https://wit.nts-corp.com/2017/08/31/4861)

요약 하자면

  * H/W 가속 렌더링 레이어 (이하 레이어)의 크기가 지나치게 크지 않도록 해야 한다.
  * 필요한 곳에 알맞게 쓰지 않으면 의도치 않은 Memory Leak 이 발생되어 오히려 UX를 저하 시킬 수 있다.
