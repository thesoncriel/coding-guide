# Atomic Design Guide

컴포넌트의 재사용성과 느슨한 결합성(loose coupling)을 위해 Atomic Design 이라 불리는 컴포넌트 분리 패턴을 따른다.

아토믹 디자인이란 각 페이지 부터 컴포넌트 까지 쪼개어지는 단위를 마치 화학구조의 그것을 닮도록 구조를 만드는 것에서 비롯된 패턴이다.

## 개요

먼저 아래의 내용을 읽고 오길 권장한다.

- [Atomic Design 개요 및 설명](https://brunch.co.kr/@ultra0034/63)

본디 아노믹 디자인은 위 글처럼 원래 **마크업(markup) 작업을 하기 위한 기준** 이다.

그러나 웹프론트엔드는 각각의 기능별로 컴포넌트를 만들어서 사용하기에 이에 맞춰 좀 더 확장된 기준을 제시한다.

## 사용 이유

이유는 명백하다. 컴포넌트를 쪼개야 하기 때문이다.

안쪼개고 그냥 쓴다고?

그럼 내부에 각종 UI 로직과 스타일이 얽히고 섥혀서 CBD(Component Based Development) 기반 개발이 될 수가 없다.

CBD가 되기 위해선 Ui 컴포넌트의 분리가 필요하고, 그 분리할 때의 기준이 필요하다.

그 방법론으로 아토믹 디자인을 이용하는 것이다.

## 쪼개는 방법

### 1. 각 단계별 명칭

위 블로그 내용처럼 5가지 구분 단계로 나뉘되 각 단계는 아래와 같이 실제 적용 기준을 변경하여 사용한다.

| 단계 | 명칭 (원본) | 명칭 (실사용) | 열갈                                                                                                                                                                                                              | 예시                |
| :--- | :---------- | :------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| 1    | Atoms       | 동일          | 단일 컴포넌트 (Single Component). 더 이상 쪼개질 수 없는 최소한의 단위.                                                                                                                                           | Button, InputBox 등 |
| 2    | Molecules   | Combines      | 조합 컴포넌트 (Combine Component). Atom 컴포넌트가 최소 1개 이상 조합됨                                                                                                                                           | InputGroup 등       |
| 3    | Organisms   | Complexes     | 복합 컴포넌트 (Complex Component). 최소 1개 이상의 Molecule 컴포넌트와 함께 다른 Atom, Molecule 컴포넌트가 조합됨.<br/>스타일링이 포함될 수 있는 최고 단계.<br />Organisms 컴포넌트 끼리 조합되어도 이 곳에 둔다. |                     |
| 4    | Templates   | Containers    | 하나의 업무를 구성할 수 있는 단위. Store 에서 State 및 Dispatch 한다.<br />내부에 스타일 코드를 가져선 안된다.<br />컨테이너는 내부에 또 다른 컨테이너를 가질 수 있다.                                            |                     |
| 5    | Pages       | 동일          | 하나의 화면을 구성할 수 있는 단위. Query 및 URL Parameter 를 Container 에게 전달 할 수 있다.<br />Page 내부에 Container 를 가질 수 있으나 그 외 하위 단계의 컴포넌트를 내포하는 것은 가급적 지양한다.             |                     |

### 2. 최대 5개의 요소

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
import { TextField } from "@material-ui/core";

// Material-UI 이용
export const SimpleInput: FC<Props> = ({ value, onChange }) => {
  return <TextField type="text" value={value} onChange={onChange} />;
};
```

### 3. 단순함

UI 컴포넌트는 그 내부에 데이터 조작 로직이 가능한한 들어가서는 안된다.

단순히 전달 받은 props 를 출력하고, 사용자가 반응하면 그에 따른 이벤트만 방출 하도록 한다.

### 4. Props Interface

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

### 5. export 및 import 규칙

Page 를 제외한 모든 컴포넌트는 export 시 default 를 쓰지 아니한다.

```tsx
// bad
export const ExampleButton: FC<C>;
```

만약 memo 나 context provider, 그 외 각종 custom HOC(High-Order Component) 사용 시엔 다음과 같이
