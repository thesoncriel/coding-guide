# CSS Class

CSS 클래스명은 기본적으로 [kebab-case](https://zetawiki.com/wiki/%EC%BC%80%EB%B0%A5-%EC%BC%80%EC%9D%B4%EC%8A%A4_kebab-case)를 이용한다.

본 문서는 자주쓰이는 클래스 명칭을 정리 해 놓았다.

하위 클래스 명칭 중 일부는 [bootstrap](https://getbootstrap.com/docs/4.3/getting-started/introduction/) 에서 쓰이는 것들을 참고 하였다.

## Component Class

대표적인 컴포넌트 명칭으로서 접두어(prefix)로 사용 한다.

해당 컴포넌트가 UI 에 직접적으로 노출되는 경우에만 사용 한다.

| prefix                | desc.                                                                                | examples                       | note                                          |
| :-------------------- | :----------------------------------------------------------------------------------- | :----------------------------- | :-------------------------------------------- |
| .button<br/>.btn      | 버튼 컴포넌트.<br/>&lt;a&gt; 및 &lt;button&gt; 태그에만 적용 한다.                   | .button-rounded<br/>.btn-sm    |                                               |
| .label                | 레이블 컴포넌트.<br/>&lt;label&gt; 및 &lt;span&gt; 태그에 적용 한다.                 |                                |                                               |
| .dropdown<br/>.select | 드롭다운 컴포넌트.<br/>DOM을 직접 구성하여 만들거나 &lt;select&gt; 태그에 적용 한다. |                                |                                               |
| .heading              | 제목 컴포넌트.<br/>&lt;h1~6&gt; 및 &lt;strong&gt;, &lt;div&gt; 태그에 적용 한다.     |                                |                                               |
| .input<br/>.textarea  | &lt;input&gt; 및 &lt;textarea&gt; 태그에만 적용 한다.                                | .input-name<br/>.textarea-desc |                                               |
| .img                  | &lt;img&gt; 태그에 적용 한다.                                                        | .img-profile                   |                                               |
| .table                | &lt;table&gt; 태그에 적용 한다.                                                      |                                |                                               |
| .list<br/>.grid       | &lt;ul&gt; 혹은 &lt;ol&gt;, &lt;div&gt; 태그에 적용 한다.                            |                                | 리스트 형태의 컴포넌트에 쓰인다.              |
| .item                 | &lt;li&gt;, &lt;div&gt; 태그에 적용 한다.                                            |                                | 리스트 내 개별 아이템 컴포넌트에 대해 쓰인다. |

## State Class

컴포넌트 상태에 따라 다른 스타일을 줄 때 사용한다.

클래스명 작성 시 다른 클래스명과 혼합하지 않고 단독으로 작성 해야 한다.

그리고 사용시엔 다른 컴포넌트 클래스와 조합하여 사용 한다.

```scss
// wrong
.stsh-active {
  // attrs..
}

// good
.stsh {
  &.active {
    // attrs..
  }
}
```

```html
<!-- 보통 상태 -->
<div class="stsh"></div>
<!-- 활성화 상태 -->
<div class="stsh active"></div>
```

### 상태 클래스 예시

| state      | desc.                             |
| :--------- | :-------------------------------- |
| .active    | 활성화 됨.                        |
| .checked   | 체크됨.                           |
| .selected  | 선택됨.                           |
| .disabled  | 비활성화 됨.                      |
| .show      | 나타남                            |
| .hide      | 숨겨짐                            |
| .open      | 열려짐                            |
| .close     | 닫혀짐                            |
| .collapsed | 아코디언 UI 에서 각 항목이 열려짐 |

## Layout Class

페이지 및 컴포넌트의 레이아웃을 정의할 때 사용한다.
| name | desc. |
| :--------- | :-------------------------------- |
| .row &gt; .col | 단(column) 구성 시 사용된다.<br/>row는 Wrapper 로 쓰이며, col은 단을 직접적으로 구현 한다. |
| .d-table &gt; div &gt; div | table layut 을 이용할 때 쓰인다. |

## Wrapper

특정 섹션이나 컴포넌트, 컨텐츠 등을 감싸는 역할을 한다. 접미사 (suffix)로 사용 한다.

| suffix     | desc.                                                                                                           | examples                         | note                                     |
| :--------- | :-------------------------------------------------------------------------------------------------------------- | :------------------------------- | :--------------------------------------- |
| .page      | 페이지를 정의한다.                                                                                              | .intro-page<br/>.board-page      | 모든 페이지가 동일 스타일을 공유 할 경우 |
| .container | 특정 영역에 대하여 최대폭(width)를 제한하여 가운데 정렬을 위한 역할.<br/>Flux Pattern 의 Container 와는 다르다. |                                  | 다른 클래스명 조합은 허용치 않는다.      |
| .section   | 영역을 구분 한다. 일반적으로 &lt;section&gt; 태그와 함께 사용되며 그 영역에서 별도의 스타일이 필요할 때 쓰인다. | .topic-section<br/>.news-section |                                          |
| .wrap      | 하나 혹은 그 이상의 컴포넌트를 감싸는 역할.                                                                     | .input-wrap<br/>.label-wrap      |                                          |
| .panel     | 패널. 제목/본문/하단 으로 이뤄진 컴포넌트 이다.                                                                 | .panel-payment                   |                                          |

## Group

컴포넌트 혹은 레이블을 그룹 지을 때 사용한다.

아래와 같이 접미사(suffix)로 -group 혹은 -grp 를 이용한다.

| name                        | desc.                                                                 | note |
| :-------------------------- | :-------------------------------------------------------------------- | :--- |
| .form-group                 | label 과 1개 이상의 input 으로 그룹 짓는다.                           |      |
| .button-group<br/>.btn-grp  | 1개 이상의 버튼을 그룹 짓는다.<br/>내부는 버튼으로만 구성되어야 한다. |      |
| .input-group<br/>.input-grp | 2개 이상의 입력 및 버튼 요소를 그룹 짓는다.                           |      |
