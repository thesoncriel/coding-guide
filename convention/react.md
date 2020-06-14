# React

본 문서는 React.js 코드 규칙을 서술하며 다음과 같은 라이브러리를 함께 사용한다 가정 하여 작성 되어있다.

- [react](https://reactjs.org/)
  - [react-router](https://reacttraining.com/react-router/web/guides/quick-start): url 라우팅을 사용하기 위함.
- [redux](https://redux.js.org/): 상태 관리 라이브러리
  - [redux-thunk](https://github.com/reduxjs/redux-thunk): 비동기 액션을 수행하기 위한 middleware.
- [styled-components](https://www.styled-components.com/): 스타일을 컴포넌트에 응용하기위한 라이브러리.

## Atomic Design

컴포넌트의 재사용성과 느슨한 결합성(loose coupling)을 위해 Atomic Design 이라 불리는 컴포넌트 분리 패턴을 따른다.

아토믹 디자인이란 각 페이지 부터 컴포넌트 까지 쪼개어지는 단위를 마치 화학구조의 그것을 닮도록 구조를 만드는 것에서 비롯된 패턴이다.

- [Atomic Design 개요 및 설명](https://brunch.co.kr/@ultra0034/63)

위 블로그 내용처럼 5가지 구분 단계로 나뉘되 각 단계는 아래와 같이 실제 적용 기준을 변경하여 사용한다.

| 조합 레벨 | 명칭                       | 설명                                                                                                                                                                    | 예시                |
| :-------- | :------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------ |
| 1         | Atoms                      | 단일 컴포넌트 (Single Component). 더 이상 쪼개질 수 없는 최소한의 단위.                                                                                                 | Button, InputBox 등 |
| 2         | Molecules                  | 조합 컴포넌트 (Combine Component). Atom 컴포넌트가 최소 1개 이상 조합됨                                                                                                 | InputGroup 등       |
| 3         | Organisms                  | 복합 컴포넌트 (Complex Component). 최소 1개 이상의 Molecule 컴포넌트와 함께 다른 Atom, Molecule 컴포넌트가 조합됨.<br/>Organisms 컴포넌트 끼리 조합되어도 이 곳에 둔다. |                     |
| 4         | Templates<br/>(Containers) | 하나의 업무를 구성할 수 있는 단위. Store 에서 State 및 Dispatch 한다.                                                                                                   |                     |
| 5         | Pages                      | 하나의 화면을 구성할 수 있는 단위. Query 및 URL Parameter 를 Container 에게 전달 할 수 있다.                                                                            |                     |

### 공통 규칙

아래와 같이 컴포넌트 작성 후 같은 소스 파일 내에서 hoc 를 거쳐 export 할 경우엔 조합 레벨이 상승하지 않는다.

```tsx
const SampleInputComponent: FC<Props> = (props) => {
  /* code... */
};

export const SampleInput = hocDebounce(SampleInputComponent);
```

### Atom - `Single Component` 조건

- 소스 파일 내에서 JSX 및 styled-component 를 이용하여 직접 만들어진 컴포넌트.
- 프로젝트내 다른 atom ~ organism 으로 정의된 컴포넌트를 가져와 쓰지 않는다.
- 이미 잘 만들어진 3rd party library 를 가져와 쓸 수 있다. (예: Material-ui, Bootstrap 등)

## Component

UI 컴포넌트는 가능한한 **함수형 컴포넌트 (Functional Component)** 로 작성하며 hooks 를 적극 이용하여 작성하는 것을 기본으로 한다.

또한 컴포넌트는 아래와 같이 2가지만 책임지도록 한다.

1. 특정 내용을 출력한다.

- 텍스트 및 이미지 출력
- 스타일 변경 및 애니메이션

2. 사용자 입력을 받아들이고 외부에 방출(emit) 한다.

또 한 아래와 같은 사례를 주의 한다.

1. props 로 들어온 데이터는 변경하지 않는다. 즉 있는 그대로 표현한다. (immutable)

- 단, 들어온 자료를 이용하여 format 변경등은 이뤄질 수 있다.

2. 외부에서 특정 컴포넌트에 대하여 직접적인 영향을 끼치지 않는다.

- 외부 Wrap 이나 부모 요소에서 강제로 스타일 변경 금지
- 외부 개입 금지
- 이런게 필요하다면 별도 props 로 추가 하거나 기능을 만들 것.

참고로 본 섹션에서의 컴포넌트란, Container, Page 에서 쓰이는 컴포넌트가 아닌 일반적인 UI 를 구성하는 컴포넌트를 말한다.

### 작성 순서

React 컴포넌트의 구성 요소는 다음의 순서를 따른다. 그 외 문법적인 내용은 eslint 및 prettier 의 설정 내용을 따른다.

```tsx
import React, { FC } from "react";
import styled from "styled-components";
import { SampleButton } from "./SampleButton";
import { withThrottle, withSomething } from "../hoc";

// 1. props 인터페이스
interface Props {
  name: string;
  id: string;
}

// 2. 스타일 내용
const Wrap = styled.div`
  display: inline-block;
`;

// 3. 이 곳에서만 사용되는 추가 요소 및 hoc 사용 컴포넌트.
//    가급적 외부로 분리하여 import 권장
const ThrottledButton = withThrottle(SampleButton);

// 4. 외부 함수. 길어지면 외부로 분리하여 import 권장
function getWidth() {
  return window.innerWidth;
}

// 5. 컴포넌트 본체. hoc 수행 하지 않는다면 여기까지.
export const SampleSection: FC<Props> = (props) => {
  // hook 은 항상 최상단에..
  const [value, setValue] = useState(0);

  useEffect(() => console.log("did mount !"), []);

  const handleClick: MouseEventHandler = () => {
    setValue((value + 1) * getWidth());
  };

  return (
    <Wrap>
      숫자: {value}
      <br />
      {props.children}
      <ThrottledButton onClick={handleClick}>클릭!</ThrottledButton>
    </Wrap>
  );
};

// 6. hoc 사용 한다면?

// 6.1. 이름을 바꾸고 export 하지 않음
const SampleSectionComponent: FC<Props> = (props) => {
  /* code.. */
};

// 6.2. hoc 수행 후 본래 컴포넌트 이름으로 받고 export 수행.
export const SampleSection = withSomething(SampleSectionComponent);
```

### JSX

JSX는 기본적으로 XML의 문법을 기반으로 한다. 따라서 [Well-Formed Document](http://mm.sookmyung.ac.kr/~sblim/lec/xml03/xml03-02.htm) 의 방식을 따르도록 한다.

따라서 아래와 같이 문자열(String)로 값을 받는 프로퍼티를 상수로 전달 시엔 아래와 같이 2중 따옴표 (double quotation)로 작성 한다.

```tsx
const Sample: FC = () => <div className="sample-wrap">{/* ... */}</div>;
```

한편 Context 나 외부 UI Component 를 사용 시 아래와 같이 `.` 을 태그 요소에 포함시키지 않도록 한다.

또 한 Fragment 는 단축 사용법인 `<></>` 으로만 사용한다.

```tsx
// wrong

import React from "react";
import Core from "@some-ui/core";
import { SampleContext } from "./contexts/";

const Sample: FC = () => (
  <SampleContext.Provider value={11}>
    <React.Fragment>
      <Core.Button>Click Here !</Core.Button>
      <Core.Text text="sample" />
    </React.Fragment>
  </SampleContext.Provider>
);
```

```tsx
// good

import React from "react";
import { Button, Text } from "@some-ui/core";
// Context Provider 의 이름을 바꿔서 하나 더 export 한다.
import { SampleContextProvider } from "./contexts/";

const Sample: FC = () => (
  <SampleContextProvider value={11}>
    <>
      <Button>Click Here !</Button>
      <Text text="sample" />
    </>
  </SampleContextProvider>
);
```

Context Provider 를 별도 상수로 받아 렌더링하는 방법도 좋다.

```tsx
// other case

import React from "react";
import { Button, Text } from "@some-ui/core";
import { SampleContext } from "./contexts/";

const CtxProvider = SampleContext.Provider;

const Sample: FC = () => (
  <CtxProvider value={11}>
    <>
      <Button>Click Here !</Button>
      <Text text="sample" />
    </>
  </CtxProvider>
);
```

### Props Interface

React 를 TypeScript 와 함께 운용할 경우 컴포넌트의 속성(Properties - 이하 Props)을 자주 사용하게 된다.

이 때 React 의 propTypes 대신 타입을 선언하고 Generic 으로 사용할 수 있다.

컴포넌트 속성은 interface 로 선언한 뒤 제네릭을 적용한다.

```jsx
// 이전 jsx 방식

const Comp = (props) => (
  <>
    <div>
      {props.name} : {props.age}
    </div>
    <button type="button" onClick={props.onClick} />
  </>
);

Comp.propTypes = {
  name: PropTypes.string,
  age: PropTypes.number,
  onClick: PropTypes.func,
};
```

```tsx
// 인터페이스 선언 및 제네릭 적용

interface Props {
  name: string;
  age: number;
  onClick: () => void;
}

const Comp: FC<Props> = (props) => (
  <>
    <div>
      {props.name} : {props.age}
    </div>
    <button type="button" onClick={props.onClick} />
  </>
);
```

#### Basic

컴포넌트의 속성을 외부에 노출시켜 사용하는 프로퍼티(이하 props)는 아래와 같이 **interface** 로 선언하여 사용한다.

이 때 명칭은 **Props** 로 고정이며 인터페이스 내 각 속성에는 주석을 달도록 한다.

```tsx
interface Props {
  /**
   * 입력할 이름값
   */
  value: string;
  /**
   * 출력될 나이
   */
  age: number;
  /**
   * 이름 변경시 발생되는 이벤트
   */
  onNameChange: (value: string) => void;
}

/**
 * 이름을 입력하는 컴포넌트.
 */
export const NameInput: FC<Props> = (props) => {
  const handleChange: ChangeEventHandler<HTMLInputElement> = (e) => {
    props.onNameChange(e.target.value);
  };

  return (
    <>
      <span>나이: {props.age}세</span>
      <input name="userName" value={props.value} onChange={handleChange} />
    </>
  );
};
```

#### Props Sharing

만약 props 내용이 다른 컴포넌트에서 동일하게 쓰여 공유 해야 할 경우엔 다음과 같이 기존에 사용되던 컴포넌트 명을 함께 명시 한다.

```tsx
// props 좌측에 컴포넌트명을 붙여 주고 export 한다.
export interface NameInputProps {
  // code...
}

// props 가 변경되었으니 변경된 내용을 적용 한다.
export const NameInput: FC<NameInputProps> = (props) => {
  // code...
};
```

## UI Component

자주 쓰이는 UI 컴포넌트 패턴을 설명한다.

### Button

버튼 컴포넌트의 주요 역할은 다음과 같다.

- 서밋(submit) 여부
- 눈에 띄는 색상 설정 (default, primary 혹은 secondary 설정)
- 비활성화 여부
- 사용자가 클릭(혹은 터치) 했는지의 여부

그에 따라 버튼 컴포넌트는 작성 시 아래와 같이 명시 된 ButtonComponentProps 를 주요 프로퍼티로 사용 해야 한다.

```tsx
export interface ButtonComponentProps extends ClassNameComponent {
  /**
   * submit 버튼 적용 여부. 적용 시 form 요소와 연동된다.
   */
  submit?: boolean;
  /**
   * 버튼 색상.
   * - (없음) - 기본 색상
   * - primary - 가장 눈에 띄어야 하는 것
   * - seconary - primary 보다 덜 눈에 띄는 색상
   */
  color?: string | "primary" | "secondary";
  /**
   * 비활성화 여부
   */
  disabled?: boolean;
  /**
   * 클릭 이벤트
   */
  onClick?: () => void;
}
```

아래는 구성된 버튼 예시이다.

```tsx
const StyledButton = styled.button`
  /* styles */
`;

export const Button: React.FC<ButtonComponentProps> = (props) => (
  <StyledButton
    type={props.submit ? "submit" : "button"}
    theme={props.color}
    disabled={props.disabled}
    onClick={props.onClick}
  >
    {props.children}
  </StyledButton>
);
```

### Text Input

입력 요소의 공통점은 사용자가 키보드로 직접 입력한다는 것이다.

입력 후 그 내용이 어딘가 반영되어야 하지만, 그 것이 어디에 반영되어야 하는지의 여부는 최소한의 정보만을 전달 하도록 한다.

즉, 특정 업무 내 사용자의 모든 입력 내용이 Object 혹은 HashMap, Dictionary 형태라 가정하면, 하나의 입력 요소는 단 하나의 key 와 value 쌍 (**key value pair - KVP**) 에만 관여하면 된다 가정하고 UI를 구성한다.

따라서 최소한의 구성 요소는 다음과 같다.

- 입력 명칭 (name) - 이 값이 어디에 쓰어야 하는지 알려준다. KVP 에서 key 에 해당 함.
- 입력 값 (value) - 변경된 값이다. KVP 에서 value 에 해당 함.
- 이벤트 - 사용자가 정상적으로 입력한 값에 대하여 외부에 알려준다.

여기서 name 과 values 는 html 내 다양한 입력 요소의 name 및 value property 를 이용하면 되며, 전반적인 컴포넌트 에 쓰이는 Props 는 아래와 같이 명시된 InputComponentProps 를 따르도록 한다.

아래는 Props interface 의 예시이다.

```tsx
/**
 * 모든 입력 컴포넌트의 공통 프로퍼티를 정의해 놓은 것.
 * 입력 컴포넌트들은 모두 이 인터페이스를 Props 로 이용한다.
 */
export interface InputComponentProps {
  /**
   * 컴포넌트에 적용할 CSS 클래스명.
   */
  className?: string;
  /**
   * 입력값
   */
  value?: string;
  /**
   * 변경 이벤트
   */
  onChange?: (args: InputChangeArgs) => void;
  /**
   * 입력란에 적용되는 name 속성값
   */
  name: string;
  /**
   * 비활성화 여부
   */
  disabled?: boolean;
}
```

이벤트는 아래와 같은 InputChangeArgs 객체를 callback 에 전달 하도록 한다.

```ts
export interface InputChangeArgs {
  /**
   * 컴포넌트의 이름.
   */
  name: string;
  /**
   * 사용자에 의해 변경된 값.
   */
  value: string;
}
```

아래는 입력 요소에 대한 작성 예시이다.

```tsx
export const InputText: FC<InputComponentProps> = (props) => {
  const handleChange = (event: any) => {
    const { name, onChange } = this.props;
    const value = event.target.value;

    if (onChange) {
      onChange({
        name,
        value,
      });
    }
  };

  return (
    <Section
      type="text"
      autoComplete={props.autoCompleteOff ? "off" : "on"}
      id={props.id}
      name={props.name}
      value={props.value}
      className={props.className}
      placeholder={props.placeholder}
      disabled={props.disabled}
      min={props.min}
      pattern={props.pattern}
      onChange={handleChange}
    />
  );
};
```

### Check Box

체크 박스는 그 결과를 반드시 boolean 형식으로 전달 해야 한다.

그럼 중요한 속성은 다음과 같아진다.

- 명칭 (name)
- 체크 여부 (checked)
- 비활성화 여부 (disabled)
- 변경 이벤트

구현 시 아래와 같이 명시된 CheckboxComponentProps 를 이용 한다.

그리고 이벤트 결과는 다음과 같이 CheckedChangeArgs 를 callback 에 전달 하도록 한다.

```ts
/**
 * 체크형 컴포넌트의 변경 이벤트 객체.
 */
export interface CheckedChangeArgs {
  /**
   * 컴포넌트의 이름.
   */
  name: string;
  /**
   * 사용자에 의해 변경된 체크 값.
   */
  checked: boolean;
}

export interface CheckboxComponentProps {
  name: string;
  checked: boolean;
  className?: string;
  disabled?: boolean;
  onChange?: (args: CheckedChangeArgs) => void;
}
```

### Radio Button

라디오 버튼은 구현 시 입력 요소처럼 InputComponentProps 를 이용하여 구현 한다.

대체로 Ui 형태가 여러 목록 중 택1 하는 구조이기 때문에 기본적으로 InputComponentProps 의 options 를 이용하여 여러개를 표현하면 된다.

이벤트 결과는 선택 한 option 에 해당하는 값을 주면 되므로 이 역시 입력 요소처럼 InputChangeArgs 객체를 callback에 전달 하도록 한다.

props 내 options 를 구성하는 모델은 다음과 같다.

```ts
export interface SelectOption {
  /**
   * 출력될 문자열
   */
  text: string;
  /**
   * 선택됐을 때 이용될 값
   */
  value: string;
}
```

### Select Box / Select List / Dropdown List

선택 상자는 선택할 수 있는 여러개의 item 을 펼쳐놓고 택1하는 구조라, 이벤트 및 데이터 구조는 Radio Button 과 똑같다.

## Container

컨테이너는 기본적으로 여러 컴포넌트를 유기적으로 연결 시켜주는 역할을 가진다.

가능한한 스타일 요소를 포함하지 않으며 주로 Store 와 연계되어 state 및 event 를 dispatch 하는 역할을 맡는다.

기본적인 구조는 다음과 같다.

```tsx
interface Props {
  /**
   * 검색 쿼리값
   */
  searchQuery?: string;
}

export const SampleContainer: FC = (props) => {
  // 리듀서에 액션을 디스패치 하기 위한 함수
  const dispatch = useDispatch();
  // 셀렉터로 데이터를 가져온다
  const value = useSelect(selValue);

  // 비동기로 수행되는 액션은 이펙트를 사용
  const handleInit = () => dispatch(effLoadData());
  // 동기적으로 수행되는 일반 액션 사용
  const handleChange = (args: DisplayPanelChangeArgs) =>
    dispatch(actDisplayChange(args));

  return (
    <Wrapper onInit={handleInit}>
      <DisplayPanel searchQuery={props.searchQuery} onChange={handleChange}>
        value = {value}
      </DisplayPanel>
    </Wrapper>
  );
};
```

위와 같이 함수형 컴포넌트에 hook을 활용한다.

이벤트 헨들러 이용 시 전달되는 이벤트 프로퍼티명을 기준으로 **on** 을 **handler** 로 바꾸어 헨들러를 명시하여 사용한다.

만약 외부 Page Component 에서 받아올 파라미터가 필요하다면 위와 같이 Props 인터페이스를 선언하여 사용한다.

## Page

페이지는 필요한 컨테이너를 모아서 최종적으로 사용자 화면에 표현하는 역할을 맡는다.

동시에 Query Parameter 나 Url Parameter 를 받아서 필요한 컨테이너에게 전달하는 역할도 맡는다.

페이지는 레이아웃을 가질 수 있으며 그 레이아웃 컴포넌트는 미리 정의된 PageContainer 를 이용토록 한다.

파라미터 처리는 아래와 같이 [react-router 에서 제공되는 hooks](https://reacttraining.com/react-router/web/api/Hooks/useparams) 를 사용한다.

```tsx
// 파라미터 인터페이스 정의
interface SampleQueries {
  id?: string;
  name?: string;
}

const SamplePage: FC = () => {
  const { id, name } = useParams<SampleQueries>(); // 제네릭 선언하여 타입 유추가 되도록 한다.

  return (
    <PageContainer title="샘플 페이지">
      id: {id}
      <br />
      name: {name}
    </PageContainer>
  );
};
```

만약 [next.js](https://nextjs.org/docs/api-reference/next/router#userouter) 라면 아래와 같은 방법으로 hook 을 쓰도록 한다.

```tsx
// 파라미터 인터페이스 정의
interface SampleQueries {
  id?: string;
  name?: string;
}

const SamplePage: FC = () => {
  const { id, name } = useRouter().query as SampleQueries; // 타입 변환하여 사용한다.

  return (
    <PageContainer title="샘플 페이지">
      id: {id}
      <br />
      name: {name}
    </PageContainer>
  );
};
```

## Component Life Cycle

React 컴포넌트는 생명 주기를 가지는데, 이 때 작성 시 주의할 점을 서술한다.

### setTimeout

컴포넌트 내부에서 setTimeout 을 이용 할 때가 있다. 이 때는 아래와 같이 state 를 별도 선언하고, unmount 할 때 clearTimeout 을 반드시 수행 시켜 준다.

```tsx
// 컴포넌트가 렌더링되고 1초뒤 무언가를 수행할 때의 예제

const TestTimer: FC = () => {
  const [timerId, setTimerId] = useState(0);

  useEffect(() => {
    setTimerId(setTimeout(() => doSomthing(), 1000));

    return () => {
      clearTimeout(timerId);
    };
  }, []);
};
```

## DOM Reference

컴포넌트 작성 시 DOM 참조를 해야 할 경우, 아래와 같이 useRef 를 사용한다.

```tsx
const TestRef: FC = () => {
  // 레퍼런스 객체는 앞에 'ref' 를 접두어로 붙여서 쓴다.
  const refWrap = useRef<HTMLDivElement>();

  return <div ref={refWrap}>테스트 ^^</div>;
};
```

특정 함수형 컴포넌트에 레퍼런스를 넘겨야 할 경우엔 그 컴포넌트는 forwardRef 를 이용하여 작성한다.

```tsx
const Block = forwardRef<HTMLDivElement, Props>((props, ref) => (
  <div {...props} ref={ref}>
    {props.children}
  </div>
));

const TestRef: FC = () => {
  // 레퍼런스 객체는 앞에 'ref' 를 접두어로 붙여서 쓴다.
  const refBlock = useRef<HTMLDivElement>();

  return <Block ref={refBlock}>테스트 ^^</Block>;
};
```

## Using HOC

한편 HOC(High Order Component) 를 사용하여 기능이 첨가될 경우엔 다음을 따른다.

1. 기존 컴포넌트 명칭 우측에 `Component` 를 덧붙이고 export 를 제거 한다.
2. hoc 를 통해 만들어진 컴포넌트 명칭을 기존에 쓰던 명칭으로 적용, export 한다.
3. 기존 주석을 export 되는 컴포넌트로 옮긴다.

아래는 위 단계를 따랐을 때 최종적으로 변경된 예시이다.

```tsx
// memo 를 통해 최적화 할 때의 예제

import React, { memo } from "react";

const NameInputComponent: FC<Props> = (props) => {
  // code...
};

/**
 * 이름을 입력하는 컴포넌트.
 */
export const NameInput = memo(NameInputComponent);
```

만약 Container Component 일 경우엔 선언은 Component 로, export 는 Container 로 한다.

```tsx
// Redux 의 connect 를 이용할 때의 예제

const LoginSectionComponent: FC<Props> = (props) => {
  // code...
};

const mapStateToProps: Props = {
  // code...
};

/**
 * 컨테이너: 로그인 영역을 담당한다.
 */
export const LoginSectionContainer = connect(mapStateToProps)(
  LoginSectionComponent
);
```
