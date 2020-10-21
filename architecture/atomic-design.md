# Atomic Design Guide

> 아래 내용은 실 업무에 사용한 경험이 있으나 개인별로 호불호가 갈릴 수 있는 요소가 많습니다.
>
> 따라서 내용 중 동의하거나 참고 할 부분만 눈여겨 보시길 권장 드립니다. 😅

Atomic Desgin 은 컴포넌트 분리 방법론 이다.

아토믹 디자인이란 각 페이지 부터 컴포넌트 까지 쪼개어지는 단위를 마치 화학구조의 그것을 닮도록 구조를 만드는 것에서 비롯된 패턴이다.

# 개요

먼저 아래의 내용을 읽고 오길 권장한다.

- [Atomic Design 개요 및 설명](https://brunch.co.kr/@ultra0034/63)

본디 아토믹 디자인은 위 글처럼 원래 **마크업(markup) 작업을 하기 위한 기준** 이다.

그러나 웹프론트엔드는 각각의 기능별로 컴포넌트를 만들어서 사용하기에 이에 맞춰 좀 더 확장된 기준을 제시한다.

## View 에만 초점이 맞춰짐

아토믹은 View 단을 쪼개는 기준을 제시 할 뿐이지, 비즈니스 로직은 염두에 두고 있지 않다.

비즈니스 로직에 대한 쪼개기는 별도 Flux Architecture 혹은 그에 준하는 아키텍처를 활용하여 처리한다.

여기선 오직 View 측 UI 컴포넌트의 분리에 대해서만 다룬다.

# 작성 방법

이제부터 설명할 아래 내용들을 요약하면 아래와 같은 조건으로 정리된다.

1. 5단계로 쪼개며 각각 atoms, combines, complexes, containers, pages 이다.
2. HOC 이용 시 atomic level 이 유지된다.
3. 표현 컴포넌트 하나당 내부에 허용되는 요소를 제한 한다. (특수한 경우가 아니라면 최대 5개)
4. 컴포넌트간의 margin 은 그걸 벌리기 원하는 컴포넌트나 List Wrapper, Template 에게 맡긴다.
5. List 컴포넌트안에는 가급적 1가지의 Item 컴포넌트만 가지도록 작성한다.
6. 컴포넌트는 `Controlled dumb Component` 형식으로 만든다. [참고](https://stackoverflow.com/questions/34348165/react-conditional-render-pattern)
7. 조건별 컴포넌트 출력은 `A or B` 형식을 지킨다.
8. 컴포넌트 내부에 데이터 조작 및 비즈니스 로직이 포함되면 안된다.
9. 내부 Props 는 `Props` 로 고정, 외부 공유 시 `{컴포넌트명}Props` 로 export 한다.
10. export 는 Page 를 제외, default 를 쓰지 않는다.
11. import 시 circular dependancy 주의

# 각 단계별 명칭

위 블로그 내용처럼 5가지 구분 단계로 나뉘되 각 단계는 아래와 같이 실제 적용 기준을 변경하여 사용한다.

| atomic level | 명칭 (원본) | 명칭 (실사용)        | 설명                                                                                                                                                                                                              | 예시                | 하위 요쇼 최대 허용 개수 |
| :----------- | :---------- | :------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------ | :----------------------- |
| 1            | Atoms       | 동일                 | 단일 컴포넌트 (Single Component). 더 이상 쪼개질 수 없는 최소한의 단위.                                                                                                                                           | Button, InputBox 등 | 5                        |
| 2            | Molecules   | Combines             | 조합 컴포넌트 (Combine Component). Atom 컴포넌트가 최소 1개 이상 조합됨                                                                                                                                           | InputGroup 등       | 5                        |
| 3            | Organisms   | Complexes            | 복합 컴포넌트 (Complex Component). 최소 1개 이상의 Molecule 컴포넌트와 함께 다른 Atom, Molecule 컴포넌트가 조합됨.<br/>스타일링이 포함될 수 있는 최고 단계.<br />Organisms 컴포넌트 끼리 조합되어도 이 곳에 둔다. |                     | 5                        |
| 4            | Templates   | `없음` -> Containers | 대신 Container 를 두며 이들은 하나의 업무를 구성하는 최소 단위가 된다. Template 과는 관계가 없다. 자세한 건 아래 내용 참고                                                                                        |
| 5            | Pages       | 동일                 | 하나의 화면을 구성할 수 있는 단위. Query 및 URL Parameter 를 Container 에게 전달 할 수 있다.<br />Page 내부에 Container 를 가질 수 있으나 그 외 하위 단계의 컴포넌트를 내포하는 것은 가급적 지양한다.             |                     | 제한 없음                |

위 표에서도 보시다시피 아토믹 디자인에서 제시하는 Template 단계는 사실상 본 문서상의 규칙엔 존재하지 않는다.

대신 그 단계는 Container 를 두나, 이는 서로 다른 의미로써 사용됨을 잊지 말자!

## Presentational Component

이들은 Atoms, Combine, Complexes 에 해당된다.

이러한 atomic level 1~3단계 컴포넌트들은 스타일링을 포함시킬 수 있는 유일한 컴포넌트이다.

4~5단계의 Container, Page 에는 가급적 스타일링을 포함시키지 않도록 한다.

스타일링을 포함하므로 스스로가 자기 자신의 스타일에 대해선 전적으로 책임질 수 있어야한다.

margin 이나 padding 같은 주변 컴포넌트와의 간격 조정 등도 외부 props에 의존하지 않도록 고민하여 작성하자.

또한 지나친 확장성은 고려하지 않는다.

예컨데, 각종 글자색과 배경색, 테두리 등이 다르고 나머지 기능이 동일하더라도 이들에게 무리하게 props 를 주지 말고, 각자 개별 컴포넌트로 만들도록 한다.

즉 `너무 공용화를 염두에 두며 개발하지 말것` ..이다.

반응형(responsible/scalable)은 한 컴포넌트에서 media query 로 대처하되, 적응형(adaptive)은 이 후 내용을 참고 한다.

> 공용화를 염두에 두며 개발하지 말라는 의미는 공통화에 목매인 나머지, 본디 그 컴포넌트가 잘 해야할 기능 이상의 것을 굳이 할 필요가 없다는 의미 이다.
> 그리고 이 것과 **보기 좋은 코드를 만드는 것**과는 별개의 문제다.
>
> 그 어떤 코딩을 하든 깔끔하고 보기 좋은 코드를 작성하기 위해 고민해야 하는건 당연한 것이다.

### 스타일 중복은 어떻게 ??

방법은 아래와 같다.

1. 하나의 공용 스타일을 styled-component 기준, `css` 로 만들어 import 시켜 이용한다.
2. theme 기능을 이용하여 공용 style 을 정의하고 컴포넌트 스타일 과정에 props 와 함께 가져와서 사용한다.
3. 하나의 `Base Component` 를 작성하고 아무 기능 없이 export 하여 atom 컴포넌트를 만든다. 그리고 이 것을 combine 이상 컴포넌트에서 `styled(...)`를 이용하여 추가적인 스타일을 정의한다.
4. 하나의 `Base Component` 를 React.createRef 와 함께 작성하고 3번 처럼 활용한다.
   - 스타일 쬐끔 다르고 기능을 공통으로 가져가야 할 경우 사용.

개인적으론 4번을 권장한다. 이유는 이렇게 필요한 기능을 덧대어 추가적인 책임을 부여하는 패턴이 `Decorator Pattern`에 대응 되기 때문이다.

4번 채택시 단점으론, 여기저기 마구 가져와 확장시켜 쓸 수도 있긴 하지만, 이럴 땐 다시 분리해서 정의하면 된다.

## Container

컨테이너 컴포넌트는 Redux 의 Store 나 Context 의 State 에 접근하여 하위 컴포넌트에 제공 될 수 있는 유일한 컴포넌트다.

그 목적은 `하나의 작은 업무가 완성되는 단위` 로 정의한다.

Redux 같은 Flux Architecture 가 설정되어 있거나, 자신이, 혹은 상위 요소에서 Context 로 State Provide 해 주는 상황이라는 가정하에 그 컨테이너는 어디서든 하나의 업무로써 정상 동작 되어야 한다.

즉, 하나의 컨테이너는 특정 영역(Section) 이나 페이지(Page) 에 종속적(Dependancy) 이면 안된다.

컨테이너는 Page 를 제외한 모든 atomic level 의 요소들을 포함할 수 있으며, 필요하다면 하위 컨테이너를 가지는 것도 허용된다.

### 규칙

1. 가장 중요한 규칙은 `하나의 작은 업무가 완성되는 단위` 로써 존재 해야 한다는 것.
2. 컴포넌트 스스로가 `자신들의 작은 업무` 를 완성하기 위해 API 호출이 필요하다면, 이들은 Container 가 될 수 있다.
   - AutoComplete, 혹은 외부 자료를 이용한 선택 상자 (Selectbox)같은 경우 이다.
   - 이들이 서버로 부터 받는 자료는 외부 세계에선 관심이 없으며 오히려 자동완성의 사용자 입출력 데이터 뿐이다.
   - 즉 관심사 분리(Separation of Concerns)와 캡슐화(Encapsulation)를 위해 필요하다.
   - 또 한 이런 경우, 과도한 props drilling 이나 부모 컴포넌트 입장에서 불필요한 props 전달을 최소화 할 수 있다.
   - 그런 경우가 아닌 일반적인 parent ~ children 관계일 뿐일 땐 오히려 props drilling 이 직관적이다.
3. Container 는 `사용된 컴포넌트가 가지던 atomic level 을 유지`한다.
   - 가령 atom 단계의 컴포넌트를 이용하여 Container 를 만들었다면, 이 컨테이너는 `atom` 이 된다.
   - 다른 combine, complex 도 마찬가지.
4. Did-mount, intersection 발동 시 자료 호출 및 하위 컴포넌트에 자료를 내려줄 수 있다.
   - 물론 하위 컴포넌트의 이벤트에 따른 action 및 dispatch 도 가능하다.

## Page

페이지 컴포넌트는 Routing System (React Route, Next Route 등) 에서 직접적으로 접근 가능한 유일한 컴포넌트다.

하나의 View 업무를 마무리 짓는 entry point 역할을 하기 때문이다.

따라서 이들은 절대 다른 컴포넌트에 소속되는 일이 없다.

이들은 아래와 같은 일을 주로 맡는다.

1. 현재 페이지 호출 시 함께 전달된 Query Parameter 를 받아서 하위 컨테이너 컴포넌트에게 적절히 분배한다.
2. PageContainer 을 불러와 현재 페이지와 관련된 제목이나 전반적인 레이아웃 처리를 맡는다.
3. (당연하지만) 가급적 스타일링 코드가 포함되면 안된다.
4. (가능한 한) 하위에 컨테이너 컴포넌트만 가지도록 한다. 추가적인 레이아웃 처리가 필요하다면 그걸 처리하는 Container 나 Wapper Component 를 따로 만들고 아래에 하위 컨테이너를 두는 방향으로 하자.
   - 만약 그 레이아웃 처리가 한 페이지를 모두 덮는 (Wrapping) 형태라면 차라리 PageContainer 에게 그 기능을 위임한다.

아래는 사용 예시 이다.

```tsx
const StylePage: FC = () => {
  const { keyword } = useQueryParams<StylePageQuery>();

  return (
    <PageContainer title="스타일의 완성!">
      <KeywordSearchContainer keyword={keyword} />
      <TopStyleSectionContainer />
    </PageContainer>
  );
};

export default StylePage;
```

## 특수 컴포넌트

아래는 특수한 상황에서 쓰이는 컴포넌트 종류이다.

### PageContainer

페이지 컨테이너는 페이지 컴포넌트에서만 쓰이는 특별한 컴포넌트이다.

이들은 아래와 같은 역할 및 특징을 가진다.

1. 페이지 레이아웃 잡아주기
2. header, footer, navigation 등 기본적으로 반복되는 요소들의 렌더링에 대한 책임
3. title, description, open graph 등의 메타 데이터 관리
   - 필요하다면, title 을 제외한 나머지 자료는 Redux Store 나 Context State 에서 제공받을 수 있다.
4. 일반적으로 `/components/pageContainers` 내에 위치 한다.

기본적으로 공용으로 사용되지만, 만약 페이지마다 레이아웃이나 메타 데이터등의 요소가 달라진다면 (당연히) 페이지별로 따로 페이지 컨테이너를 만들어 운용한다.

**주의:** 현 페이지에서 쓰일 여러 Context 를 묶어서 보내지 않는다. 컨텍스트는 관련된 각각의 Container 가 책임을 져야 한다!

아래는 작성 예시 이다.

```tsx
interface Props {
  title?: string;
}

// 공통으로 가장 많이 쓰이는 페이지 컨테이너는 아래와 같이 PageContainer 으로 명칭을 고정한다.
export const PageContainer: FC<Props> = ({ title, children }) => {
  // 필요하다면 컨텍스트 or 스토어를 이용할 수 있다.
  const ogMeta = useSelector(selOgMeta);

  // 필요에 따라 fragment 가 아닌 일반 Wapper 를 써도 된다.
  return (
    <>
      {/* 필요하다면 Helmet 부분만 따로 분리하여 컴포넌트화 할 수 있다. */}
      <Helmet>
        <meta content={ogMeta.descrition} property="og:description" />
        <meta content={ogMeta.image} property="og:image" />
        <meta content="StyleShare" property="og:site_name" />
        <meta content="스타일쉐어" property="og:title" />
        <meta content="website" property="og:type" />
        <title>{title}</title>
        <script>{getGtmScripts()}</script>
        <script>{getThirdPartyScripts()}</script>
      </Helmet>
      {/* 페이지 헤더에는 일반적으로 GNB 가 들어간다. */}
      <PageHeaderContainer />
      {/* 반드시 <main> 요소를 이용한 Layout Wrapper 가 존재해야 하며, children 을 넘겨 주어야 한다. */}
      <MainLayout>{children}</MainLayout>
      <PageFooter />
    </>
  );
};
```

### Adaptive Component

프론트엔드 업무를 진행하다보면, 반응형(Responsive)이 아니라 적응형(Adaptive) 컴포넌트가 필요할 때가 있다.

이런 경우엔 아래와 같이 정의 해둔 `withAdaptiveRender` HOC 를 이용한다.

```tsx
// ./feature/components/atoms/MobileViewPanel.tsx

// 공통 Props 인터페이스 선언. 보통 모바일 컴포넌트에 선언되어 있다.
export interface ViewPanelProps {
  name: string;
  age: number;
}

// 모바일 컴포넌트 선언
export const MobileViewPanel: FC<ViewPanelProps> = (props) => {
  // codes...
};
```

```tsx
// ./feature/components/atoms/DesktopViewPanel.tsx
import { ViewPanelProps } from './MobileViewPanel';

// 데스크탑 컴포넌트 선언
export const DesktopViewPanel: FC<ViewPanelProps> = (props) => {
  // codes...
};
```

```ts
// ./feature/components/atoms/ViewPanelAdaptive.ts
import { ViewPanelProps, MobileViewPanel } from './MobileViewPanel';
import { DesktopViewPanel } from './DesktopViewPanel';

// 적응형 컴포넌트 선언.
// 반드시 generic 으로 공통된 Props 를 넣어준다.
// 접미어로 Adaptive 를 반드시 명시 해준다.
export const ViewPanelAdaptive = withAdaptiveRender<ViewPanelProps>({
  mobile: MobileViewPanel,
  desktop: DesktopViewPanel,
});
```

```tsx
// ./feature/components/combines/ClientView.tsx
import { ViewPanelAdaptive } from '../atoms';

// 사용 예시
export const ClientView: FC = () => {
  return (
    <section>
      <ViewPanelAdaptive name="쏜쌤" age={29} />
    </section>
  );
};
```

만들어진 적응형 컴포넌트는 그 atomic level 이 원천 컴포넌트의 레벨과 동일 하다.

즉 `atom + atom = atom` 이며, `combine + combine = combine` 이다.

다만 다른 레벨일 경우 그 중 가장 높은 레벨의 것을 참조하게 된다.

즉 `atom + combine = combine` 이며, `complex + atom = complex` 이다.

이 규칙에 따라, atomic level 이 서로 다른 컴포넌트를 적응형으로 만들면, 보다 높은 레벨의 컴포넌트가 위치하는 곳에 두도록 한다.

추가적으로, 적응형 컴포넌트가 위치하는 곳은 일반적으로 `components` 내에 두도록 한다.

## 정리

각 5단계의 레벨별 컴포넌트는 자신만의 역할이 확실히 분리되어 있다.

이들 역할을 반드시 지킬 수 있도록 노력한다.

# High Order Component 를 이용하면 atomic level 을 유지 한다.

언급된 `Adaptive Component` 나 `Container` 와도 같은 맥락이다.

필요에 따른 HOC 사용에 따른 atomic level 은, 원천(origin) 컴포넌트의 atomic level 을 따른다.

즉, `atom + hoc = atom` 이며, `complex + hoc = complex` 이다.

```tsx
// components/atoms/FollowButton.tsx
export const FollowButton: FC<Props> = ({ onClick }) => {
  return (
    <button type="button" onClick={onClick}>
      팔로우 하기!
    </button>
  );
};

// components/atoms/FollowButtonWithTracking.tsx
// atom 컴포넌트가 hoc 를 거친 것은 atom 으로 간주된다.
export const FollowButtonWithTracking = withTracking(FollowButton)({
  className: 'home_follow-button-click',
});
```

# 최대 5개의 요소

컴포넌트 하나당 최대 5개의 요소를 허용한다.

그 요소는 JSX 기준, 아래와 같다.

- ReactDOM 을 직접 가져다 쓴 것
- 스타일링이 적용된 컴포넌트
- 외부 라이브러리의 컴포넌트 (ex: Material-UI, Bootstrap 등)

```tsx
// ReactDOM 직접 이용

export const SimpleInput: FC<Props> = ({ value, onChange }) => {
  return <input type="text" value={value} onChange={onChange} />;
};
```

```tsx
// Styled-Component 이용

const Input = styled.input`
  /* ...code */
`;

export const SimpleInput: FC<Props> = ({ value, onChange }) => {
  return <Input type="text" value={value} onChange={onChange} />;
};
```

```tsx
// Material-UI 이용
import { TextField } from '@material-ui/core';

export const SimpleInput: FC<Props> = ({ value, onChange }) => {
  return <TextField type="text" value={value} onChange={onChange} />;
};
```

# 컴포넌트간의 여백 설정

디자인 시안대로 작업할 시 디자이너의 의도대로 여백을 주어야 할 경우가 생긴다.

내부 여백(padding)은 당연히 컴포넌트 스스로가 책임지는게 맞으며, 외부 여백(margin)은 아래와 같은 2가지 경우로 나눌 수 있다.

1. 컴포넌트 스스로가 외부와 여백이 벌어지길 바라는 경우
2. 컴포넌트 이용하는 주체가 여백을 조정하길 바라는 경우

아래는 경우별 예시이며, 상황에 따라 적절한 방법을 선택한다.

참고로 일반적으로 책임지는 외부 여백은 상하 배치일 경우 `margin-bottom`, 좌우 배치일 경우 `margin-right` 를 말한다.

heading 이나 title 같은 경우는 padding 을, flex 를 쓸 경우 height 에 상하 중앙정렬을 하는 등, 스스로가 외부 여백에 간섭받지 않도록 작성한다.

## 컴포넌트 스스로가 책임진다

이러한 경우는 대체로 같은 컴포넌트를 사용한다면 같은 여백이 유지된다는 조건이 성립할 때 이다.

```tsx
// ReplyItem.tsx

// 벌리고 싶은 여백은 스스로가 책임 진다.
const Wrap = styled.div`
  background: skyblue;
  margin-bottom: 16px;

  &:last-child {
    margin-bottom: 0;
  }
`;

export const ReplyItem: FC<Props> = ({ value, onChange }) => {
  return (
    <Wrap>
      <strong>댓글 작성:</strong>
      <br />
      <textarea onChange={onChange}>{value}</textarea>
    </Wrap>
  );
};
```

```tsx
// ReplyList.tsx

// 자기 영역외 바깥과의 여백만 이 부모 컴포넌트가 책임 진다.
const Section = styled.section`
  margin-bottom: 24px;
`;

export const ReplySection: FC<Props> = ({ items }) => {
  return (
    <Section>
      {items.map((item, idx) => (
        <ReplyItem key={idx} {...items} />
      ))}
      <a href="/to-more">더 보기</a>
    </Section>
  );
};
```

## 컴포넌트 사용처에서 책임진다

같은 컴포넌트를 사용하는데 사용처에 따라 다르거나, 사용되는 컴포넌트간의 여백이 일정 할 경우에 해당된다.

```tsx
// ReplyItem.tsx

// 자신 스스로는 여백을 책임지지 않는다.
const Wrap = styled.div`
  background: skyblue;
`;

export const ReplyItem: FC<Props> = ({ value, onChange }) => {
  return (
    <Wrap>
      <strong>댓글 작성:</strong>
      <br />
      <textarea onChange={onChange}>{value}</textarea>
    </Wrap>
  );
};
```

```tsx
// ReplyList.tsx

// 영역 바깥과 함께, 사용되는 자식 컴포넌트 간의 여백도 모두 책임진다.
const Section = styled.section`
  & > div {
    margin-bottom: 16px;

    &:last-child {
      margin-bottom: 0;
    }
  }

  margin-bottom: 24px;
`;

export const ReplySection: FC<Props> = ({ items }) => {
  return (
    <Section>
      {items.map((item, idx) => (
        <ReplyItem key={idx} {...items} />
      ))}
      <a href="/to-more">더 보기</a>
    </Section>
  );
};
```

# List 안에는 1가지 Item

List 컴포넌트를 작성할 때 아래와 같이 Array.prototype.map 을 이용하여 반복자 처리하는 경우가 많다.

이 때 아래와 같은 상황은 피하도록 하자.

```tsx
// bad

// model.ts

export interface ItemModel {
  name: string;
  age: number;
}

// List.tsx

interface Props {
  items: ItemModel[];
}

export const List: FC<Props> = ({ items }) => {
  return (
    <ul>
      {items.map((item, idx) => (
        <ItemSuper key={idx}>
          <ItemHead>{item.name}</ItemHead>
          <MemberAge age={item.age} />
        </ItemSuper>
      ))}
    </ul>
  );
};
```

보시다시피 List 안에 Item 요소가 children 으로 순회하는 모습이다. 이러면 `ItemSuper` 라는 요소를 어디선가 재사용할 순 있겠지만 때에 따라 저 하위 요소가 방대해질 수 있고, 하위 데이터의 사용 기법을 List 컴포넌트가 알아야 한다.

이럴 땐 그냥 item 자료를 Item 컴포넌트에게 넘겨서 하위 컴포넌트가 `알아서` 하도록 아래와 같이 고치자.

```tsx
// good !

// 모델 선언 생략

// Item.tsx

interface Props {
  item: ItemModel;
}

export const Item: FC<Props> = ({ item }) => {
  return (
    <ItemSuper>
      <ItemHead>{item.name}</ItemHead>
      <MemberAge age={item.age} />
    </ItemSuper>
  );
};

// List.tsx

interface Props {
  items: ItemModel[];
}

export const List: FC<Props> = ({ items }) => {
  return (
    <ul>
      {items.map((item, idx) => (
        {/* 리스트는 아이템에게 그냥 자료를 떠넘기도록 하자. */}
        <Item key={idx} item={item} />
      ))}
    </ul>
  );
};
```

훨씬 깔끔해졌다.

이렇게 되면 기존 Combine 요소였던 `List` 컴포넌트는 추가된 `Item` 컴포넌트로 인하여 Complex 로 승격될 수 있다.

# 내부 컴포넌트에 대한 조건별 렌더링 사용 제한

아래와 같이 컴포넌트를 사용함에 있어 children 을 통한 Wrapping 은 매우 자연스러운 현상이다.

```tsx
// no problem!

const Sample: FC = ({ title, desc, imgSrc }) => {
  return (
    <Section>
      <Heading>{title}</Heading>
      <Article>
        <Image src={imgSrc} />
        <Paragraph>{desc}</Paragraph>
      </Article>
    </Section>
  );
};
```

다만, 아래와 같이 조건에 따른 출력을 하는 경우가 있다.

```tsx
const Sample: FC = ({ title, desc, imgSrc }) => {
  return (
    <Section>
      <Heading>{title}</Heading>
      {!!desc && (
        <Article>
          <Image src={imgSrc} />
          <Paragraph>{desc}</Paragraph>
        </Article>
      )}
    </Section>
  );
};

// -------- or --------

// Article 대신 Fragment 사용
const Sample: FC = ({ title, desc, imgSrc }) => {
  return (
    <Section>
      <Heading>{title}</Heading>
      {!!desc && (
        <>
          <Image src={imgSrc} />
          <Paragraph>{desc}</Paragraph>
        </>
      )}
    </Section>
  );
};
```

이런 경우는 아래와 같이 변경한다.

```tsx
// 추가됨 ! - Fragment 사용 시 예제
const Description: FC<Props> = ({ imgSrc, show, children }) => {
  if (!show) {
    return null;
  }
  return (
    <>
      <Image src={imgSrc} />
      <Paragraph>{children}</Paragraph>
    </>
  );
};

// 변경된 샘플 컴포넌트.
const Sample: FC<Props> = ({ title, desc, imgSrc }) => {
  return (
    <Section>
      <Heading>{title}</Heading>
      <Description show={!!desc} src={imgSrc}>
        {desc}
      </Description>
    </Section>
  );
};
```

즉, 자기 자신에 대한 최종 변경 권한은 자기 자신에게 두어야 한다.

이러한 컴포넌트를 `Controlled dumb Component(제어되는 멍청한 컴포넌트)` 라 부른다.

[참고글](https://stackoverflow.com/questions/34348165/react-conditional-render-pattern)

# 조건별 컴포넌트 출력은 A or B

데이터 로딩중일 때 대신 사용하는 Indicator 나 Skeleton 은 그 조건에 따라 출력하게 된다.

이 때 조건에 따라 출력되는 컴포넌트는 A or B 형태어야 한다.

즉 아래와 같은 형태여야 한다.

```tsx
const ViewContainer: FC = () => {
  const { loading, items } = useSelector(selLoading);

  return loading ? <ListSkeleton /> : <List items={items} />;
};
```

만약 아래와 같이 Fragment 나 children 을 이용하게 된다면 별도 컴포넌트로 분리한다.

```tsx
// bad
const ViewContainer: FC = () => {
  const { loading, items } = useSelector(selLoading);

  return loading ? (
    <ListSkeleton />
  ) : (
    <>
      <Header>
        <Heading>쓱싹쓱싹</Heading>
      </Header>
      <List items={items} />
    </>
  );
};
```

```tsx
// good

// ListPanel.tsx
import { List, ListProps, Heading } from '../combines';

// List 컴포넌트에서 export 한 자기자신의 Props 를 이용한다.
const ListPanel: FC<ListProps> = ({ items }) => {
  return (
    <>
      <Header>
        <Heading>쓱싹쓱싹</Heading>
      </Header>
      <List items={items} />
    </>
  );
};

// ------------------------------------ //

// ViewContainer.tsx

const ViewContainer: FC = () => {
  const { loading, items } = useSelector(selLoading);

  return loading ? <ListSkeleton /> : <ListPanel items={items} />;
};
```

이렇게 하는 까닭은 하나의 컴포넌트가 여러개의 책임을 지게 만드는 상황을 피하기 위함이다.

즉 단일 책임 원칙 (Single Responsibility Principle)을 준수하기 위함이다.

이렇게 책임을 분리 해 놓으면 깔끔한 코드가 완성된다.

대체로 Fragment 로 감싸진 것은 하나의 컴포넌트로 분리된다 보아도 좋다.

# 단순함

UI 컴포넌트는 그 내부에 데이터 조작 로직이 가능한 한 들어가서는 안된다.

단순히 전달 받은 props 를 출력하고, 사용자가 반응하면 그에 따른 이벤트만 방출 하도록 한다.

즉 관심사 분리 (Separation of Concerns) 원칙을 지키도록 한다.

## 잘못된 예시

```tsx
// bad

// 입력 컴포넌트 정의

interface Props {
  // 이렇게 엄청난(?) 데이터 덩이를 컴포넌트에게 던져주지 않는다.
  value?: {
    domain?: {
      profile?: {
        age: string;
        name: string;
        image: any;
      };
    };
  };
  // 외부에 키보드 이벤트 객체를 굳이 넘겨야 하는가?
  onChange: (event: KeyboardEvent) => void;
}

export const SampleInput: FC<Props> = (props) => {
  // 이런 데이터 중 필요한 데이터를 추출하고 검증하는 등 데이터 로직이 들어가면 필연적으로 비대해 진다.
  // 보라! 결국 이 컴포넌트에게 필요한 건 나이 숫자값과 입력값 변경에 대한 이벤트 뿐이다!
  const age =
    props.value &&
    props.domain &&
    props.domain.profile &&
    props.domain.profile.age;
  let value = parseInt(age, 10);

  if (isNaN(value)) {
    value = 0;
  }

  return <input type="number" value={value} onChange={props.onChange} />;
};

// 외부에서 쓸 때 - ExecutionContainer.tsx

export const ExecutionContainer: FC = () => {
  const data: Domain = useSelector(selDomain);

  // 외부에서는 굳이 필요치 않은 이벤트 객체를 받아 하위 객체에 접근 해야하는 부담이 있다.
  const handlerChange = (event: KeyboardEvent) => {
    const value = parseInt(event.target.value, 10) || 0;

    dispatch({ type: 'change', payload: value });
  };

  return <SampleInput value={data} onChange={handleChange} />;
};
```

## 잘 된 예시

```tsx
// good !

// 입력 컴포넌트 정의

interface Props {
  // 이 컴포넌트에게 필요한 만큼 충분히 정제된 자료를 넘겨 받는다.
  value: number;
  // 외부에 변화된 값만 넘겨준다.
  onChange: (value: number) => void;
}

export const SampleInput: FC<Props> = ({ value, onChange }) => {
  // 자신이 전달하는 데이터에 대한 신뢰도를 높이기 위해 최소한의 로직만 넣어둔다.
  const handleChange = (event: KeyboardEvent) => {
    onChange(parseInt(event.target.value, 10) || 0);
  };
  return <input type="number" value={value} onChange={handleChange} />;
};

// 외부에서 쓸 때 - ExecutionContainer.tsx

export const ExecutionContainer: FC = () => {
  // 이미 필요한만큼 정제된 데이터를 전달한다.
  const age: number = useSelector(selDomainProfileAge);

  const handlerChange = (value: number) => {
    dispatch({ type: 'change', payload: value });
  };

  return <SampleInput value={age} onChange={handleChange} />;
};
```

# 7. Props Interface

React 와 TypeScript 를 함께 쓰는 프로젝트 기준, 컴포넌트의 프로퍼티 정의는 인터페이스(interface)를 사용한다.

내부에서 쓰고 마는 것은 `Props` 로 고정이며, 현 컴포넌트의 props 를 공유해야 할 경우엔 `{컴포넌트 명칭}Props` 와 같은 형식으로 export 한다.

```tsx
// 일반적인 상황
interface Props {
  name: string;
  onClick: () => void;
}

export const SampleButton: FC<Props> = ({ name, onClick, children }) => {
  return (
    <Button type="button" name={name} onClick={onClick}>
      {children}
    </Button>
  );
};

// props 를 공유하는 상황
export interface SampleButtonProps {
  name: string;
  onClick: () => void;
}

export const SampleButton: FC<SampleButtonProps> = ({
  name,
  onClick,
  children,
}) => {
  return (
    <Button type="button" name={name} onClick={onClick}>
      {children}
    </Button>
  );
};
```

# export 규칙

SSOT (Single Source Of Truth - 단일진실공급원) 원리에 따라 모든 컴포넌트는 자신이 속한 모듈내 `index.ts` 파일에 자신의 사용처를 제한한다.

이에 따른 장점은, 외부에서는 해당 컴포넌트의 쪼개기 레벨이 얼마인지 관계치 않고 단지 `./components` 출처를 통해서만 접근해서 이용할 수 있다는 점이다.

단점은 각 레벨별 index.ts 를 만들어서 관계된 모든 컴포넌트를 export 해야하며 컴포넌트 레벨에 변동이 있다면 이 변동 내역도 index.ts 에 함께 반영해야 하는 번거로움이 있다.

단, index.ts 내에 주변의 모든 컴포넌트를 export 를 넣어줘야하는 불편함은 vscode 기준, `export-typescript` 확장 기능을 이용하면 매우 편리하게 대처할 수 있다!

## 작성 방법

Page 를 제외한 모든 컴포넌트는 export 시 default 를 쓰지 아니한다.

```tsx
// ExampleButton.tsx

// what ?!
export default (props) => {
  // ..codes
};

// good good !!
export const ExampleButton: FC<Props> = (props) => {
  // ..codes
};
```

만약 memo 나 context provider, 그 외 각종 custom HOC(High-Order Component) 사용 시엔 다음과 같은 규칙으로 작성한다.

```
const {파일명}Component = () => { /* ... */ };

export const {파일명} = with({파일명}Component);
```

```tsx
// ExampleButton.tsx

// bad

const ExampleButton: FC<Props> = (props) => {
  // ..codes
};

export default withCustom(ExampleButton);
```

```tsx
// ExampleButton.tsx

// good good !!

const ExampleButtonComponent: FC<Props> = (props) => {
  // ..codes
};

export const ExampleButton = withCustom(ExampleButtonComponent);
```

# import 시 주의 (순환적 의존)

컴포넌트 끼리가 아닌 외부에서 import 할 때는 `./components` 와 같이 index.ts 를 대상으로 가져와서 사용한다.

만약 컴포넌트 끼리 import 하면, 아래처럼 의도치 않게 circular dependancy 가 발생되기도 하므로 주의한다.

```tsx
// 순환적 의존이 의심된다..
// ./components/complexes/ComplexPanel.tsx

import {
  SampleInput, // atoms/...
  SampleWrap, // atoms/...
  WhatTheHellSelect, // combines/...
  AutoCompleteContainer, // containers/...
} from '../'; // index.ts

export const ComplexPanel: FC<Props> = (props) => {
  return (
    <SampleWrap>
      <SampleInput />
      <WhatTheHellSelect />
      <AutoCompleteContainer />
    </SampleWrap>
  );
};
```

종종 위처럼 작성되는 경우가 있는데, 빌드 오류를 방지하기 위해 아래처럼 바꿔준다.

```tsx
// good~!
// ./components/complexes/ComplexPanel.tsx

import { SampleInput, SampleWrap } from '../atoms';
import { WhatTheHellSelect } from '../combines';
import { AutoComplete } from '../containers';

export const ComplexPanel: FC<Props> = (props) => {
  return (
    <SampleWrap>
      <SampleInput />
      <WhatTheHellSelect />
      <AutoCompleteContainer />
    </SampleWrap>
  );
};
```

이렇게 각 레벨별로 따로 불러오면 해결된다.

순환적 의존은 비단 컴포넌트 만의 문제가 아니라 프로젝트 전반적으로 주의해야 할 사항이다. 😄
