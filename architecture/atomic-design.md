# Atomic Design Guide

컴포넌트의 재사용성과 느슨한 결합성(loose coupling)을 위해 Atomic Design 이라 불리는 컴포넌트 분리 패턴을 따른다.

아토믹 디자인이란 각 페이지 부터 컴포넌트 까지 쪼개어지는 단위를 마치 화학구조의 그것을 닮도록 구조를 만드는 것에서 비롯된 패턴이다.

# 개요

먼저 아래의 내용을 읽고 오길 권장한다.

- [Atomic Design 개요 및 설명](https://brunch.co.kr/@ultra0034/63)

본디 아노믹 디자인은 위 글처럼 원래 **마크업(markup) 작업을 하기 위한 기준** 이다.

그러나 웹프론트엔드는 각각의 기능별로 컴포넌트를 만들어서 사용하기에 이에 맞춰 좀 더 확장된 기준을 제시한다.

# 사용 이유

이유는 명백하다. 컴포넌트를 쪼개야 하기 때문이다.

안쪼개고 그냥 쓰게 되면,

내부에 각종 UI 로직과 스타일이 얽히고 섥혀서 CBD(Component Based Development) 기반 개발이 될 수가 없다.

CBD가 되기 위해선 Ui 컴포넌트의 분리가 필요하고, 그 분리할 때의 기준이 필요하다.

이미 하나의 Page 나 Container 안에 온갖 잡다한 로직을 섞어서 쓰고 펑펑(!!) 터진 경험이 여럿 있을 것이다.

이들을 어떻게 하면 해결하지...? 라는 당연히 엔지니어로써의 고민을 안해 본 프론트 개발자는 없을터.

그 방법론으로 아토믹 디자인을 이용하는 것이다.

## 오해?!

아토믹은 View 단을 쪼개는 기준을 제시 할 뿐이지, 비즈니스 로직은 염두에 두고 있지 않다!

비즈니스 로직에 대한 쪼개기는 별도 Flux Architecture 혹은 그에 준하는 아키텍처를 활용하여 처리한다.

여기선 오직 View 측 UI 컴포넌트의 분리에 대해서만 다룬다.

# 작성 방법

이제부터 설명할 아래 내용들을 요약하면 아래와 같은 조건으로 정리된다.

1. 5단계로 쪼개며 각각 atoms, combines, complexes, containers, pages 이다.
2. 하나의 컴포넌트는 최대 5개의 다른 컴포넌트 요소를 가질 수 있다.
3. List 컴포넌트안에는 단 1가지의 Item 컴포넌트만 가질 수 있다.
4. 컴포넌트는 `Controlled dumb Component` 형식으로 만든다. [참고](https://stackoverflow.com/questions/34348165/react-conditional-render-pattern)
5. 컴포넌트 내부에 데이터 조작 및 비즈니스 로직이 포함되면 안된다.
6. 내부 Props 는 `Props` 로 고정, 외부 공유 시 `{컴포넌트명}Props` 로 export 한다.
7. export 는 Page 를 제외, default 를 쓰지 않는다.
8. import 시 circular dependancy 주의

# 1. 각 단계별 명칭

위 블로그 내용처럼 5가지 구분 단계로 나뉘되 각 단계는 아래와 같이 실제 적용 기준을 변경하여 사용한다.

| 단계 | 명칭 (원본) | 명칭 (실사용) | 설명                                                                                                                                                                                                              | 예시                |
| :--- | :---------- | :------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| 1    | Atoms       | 동일          | 단일 컴포넌트 (Single Component). 더 이상 쪼개질 수 없는 최소한의 단위.                                                                                                                                           | Button, InputBox 등 |
| 2    | Molecules   | Combines      | 조합 컴포넌트 (Combine Component). Atom 컴포넌트가 최소 1개 이상 조합됨                                                                                                                                           | InputGroup 등       |
| 3    | Organisms   | Complexes     | 복합 컴포넌트 (Complex Component). 최소 1개 이상의 Molecule 컴포넌트와 함께 다른 Atom, Molecule 컴포넌트가 조합됨.<br/>스타일링이 포함될 수 있는 최고 단계.<br />Organisms 컴포넌트 끼리 조합되어도 이 곳에 둔다. |                     |
| 4    | Templates   | Containers    | 하나의 업무를 구성할 수 있는 단위. Store 에서 State 및 Dispatch 한다.<br />내부에 스타일 코드를 가져선 안된다.<br />컨테이너는 내부에 또 다른 컨테이너를 가질 수 있다.                                            |                     |
| 5    | Pages       | 동일          | 하나의 화면을 구성할 수 있는 단위. Query 및 URL Parameter 를 Container 에게 전달 할 수 있다.<br />Page 내부에 Container 를 가질 수 있으나 그 외 하위 단계의 컴포넌트를 내포하는 것은 가급적 지양한다.             |                     |

## Atoms ~ Complexes

이들 1~3단계 컴포넌트들은 스타일링을 포함시킬 수 있는 유일한 컴포넌트이다.

4~5단계의 Container, Page 에는 가급적 스타일링을 포함시키지 않도록 한다.

스타일링을 포함하므로 스스로가 자기 자신의 스타일에 대해선 전적으로 책임질 수 있어야한다.

margin 이나 padding 같은 주변 컴포넌트와의 간격 조정 등도 외부 props에 의존하지 않도록 고민하여 작성하자.

또한 지나친 확장성은 고려하지 않는다.

예컨데, 각종 글자색과 배경색, 테두리 등이 다르고 나머지 기능이 동일하더라도 이들에게 무리하게 props 를 주지 말고, 각자 개별 컴포넌트로 만들도록하자.

즉 `너무 공용화를 염두에 두며 개발하지 말것` ..이다.

반응형(responsible/scalable)은 한 컴포넌트에서 media query 로 대처하되, 적응형(adaptive)은 이 후 내용을 참고 한다.

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

컨테이너는 Page 를 제외한 모든 레벨의 요소들을 포함할 수 있다.

일반적으로 Atoms ~ Organisms 요소만을 업무에 이용되나 다음과 같은 사례에 유의한다.

1. 한 페이지당 영역(Section)이 지나치게 많아서 (ex: 메인, 홈, 대시보드 등) Container 가 많아지면 업무 성향별로 묶어주고 정리하는 역할, 그 외의 것은 아무것도 하지 않는 경우가 있는데, 이것도 컨테이너로 간주한다.
   - 하지만 이런 경우는 그리 많지 않다.
2. AutoComplete, 외부 자료를 이용한 선택 상자 (Selectbox) 와 같은 역할을 하는 컴포넌트는 필연적으로 API 호출이 필요하기에 Container 로 감싸진 형태를 띄어 자신들의 업무가 완성된다.
   - 이것처럼 Container 레벨이 아닌 그 보다 낮은 레벨의 컴포넌트에 Container 가 소속될 수 있으며 이를 허용한다. 단, `Container 를 포함할 수 있는 컴포넌트는 Complexes (Organisms) 로 제한` 한다.
3. Did-mount 시 자료 호출을 맡을 수 있다.
   - 다만 여러 sibling container 들에 영향을 미치기에 특정 컨테이너를 선택해 두기 애매하다면, 보다 상위 Container 를 만들어 포함 시키던지, 아니면 상위 Page 에서 호출 시키는 것도 나쁘지 않다.

### PageContainer

페이지 컨테이너는 페이지 컴포넌트에서만 쓰이는 특별한 컨테이너 컴포넌트이다.

이들은 아래와 같은 역할을 가진다.

1. 페이지 레이아웃 잡아주기
2. header, footer, navigation 등 기본적으로 반복되는 요소들의 렌더링에 대한 책임
3. title, description, open graph 등의 메타 데이터 관리
   - title 을 제외한 나머지 자료는 Redux Store 나 Context State 에서 제공받는다.

기본적으로 공용으로 사용되지만, 만약 페이지마다 레이아웃이나 메타 데이터등의 요소가 달라진다면 (당연히) 페이지별로 따로 페이지 컨테이너를 만들어 운용한다.

## Page

페이지 컴포넌트는 아래와 같은 일을 주로 맡는다.

1. 현재 페이지 호출 시 함께 전달된 Query Parameter 를 받아서 하위 컨테이너 컴포넌트에게 적절히 분배한다.
2. PageContainer 를 불러와 현재 페이지와 관련된 제목이나 전반적인 레이아웃 처리를 맡는다.
3. (당연하지만) 가급적 스타일링 코드가 포함되면 안된다.
4. 하위에 컨테이너 컴포넌트만 가질 수 있다. 추가적인 레이아웃 처리가 필요하다면 그걸 처리하는 컨테이너를 따로 만들고 아래에 하위 컨테이너를 두는 방향으로 하자.
   - 만약 그 레이아웃 처리가 한 페이지를 모두 덮는 (Wrapping) 형태라면 차라리 페이지 컨테이너를 그렇게 고치자.
     - 현재 Feature 에서 공용으로 쓰고 싶어서 그랬다고? 그냥 페이지별로 따로 만들어라...

## 정리

각 5단계의 레벨별 컴포넌트는 자신만의 역할이 확실히 분리되어 있다.

이들 역할을 반드시 지킬 수 있도록 노력한다.

# 2. 최대 5개의 요소

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
import { TextField } from "@material-ui/core";

export const SimpleInput: FC<Props> = ({ value, onChange }) => {
  return <TextField type="text" value={value} onChange={onChange} />;
};
```

# 3. List 안에는 1가지 Item

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

# 4. 내부 컴포넌트에 대한 조건별 렌더링 사용 제한

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

# 5. 단순함

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

    dispatch({ type: "change", payload: value });
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
    dispatch({ type: "change", payload: value });
  };

  return <SampleInput value={age} onChange={handleChange} />;
};
```

# 6. Props Interface

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

# 7. export 규칙

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

# 8. import 시 주의 (순환적 의존)

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
} from "../"; // index.ts

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

import { SampleInput, SampleWrap } from "../atoms";
import { WhatTheHellSelect } from "../combines";
import { AutoComplete } from "../containers";

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
