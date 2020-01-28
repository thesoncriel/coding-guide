# React
본 문서는 React.js 코드 규칙을 서술하며 다음과 같은 라이브러리를 함께 사용한다 가정 하여 작성 되어있다.

* [react](https://reactjs.org/)
  - [react-router](https://reacttraining.com/react-router/web/guides/quick-start): url 라우팅을 사용하기 위함.
* [redux](https://redux.js.org/): 상태 관리 라이브러리
  - [redux-thunk](https://github.com/reduxjs/redux-thunk): 비동기 액션을 수행하기 위한 middleware.
* [styled-components](https://www.styled-components.com/): 스타일을 컴포넌트에 응용하기위한 라이브러리.

## Atomic Design

컴포넌트의 재사용성과 느슨한 결합성(loose coupling)을 위해 Atomic Design 이라 불리는 컴포넌트 분리 패턴을 따른다.

아토믹 디자인이란 각 페이지 부터 컴포넌트 까지 쪼개어지는 단위를 마치 화학구조의 그것을 닮도록 구조를 만드는 것에서 비롯된 패턴이다.

* [Atomic Design 개요 및 설명](https://brunch.co.kr/@ultra0034/63)

위 블로그 내용처럼 5가지 구분 단계로 나뉘되 각 단계는 아래와 같이 명칭과 기능을 변경/추가 하여 사용한다.

| 기존 명칭 | 이용 명칭 | 설명 | 예시 |
|:--------|:-------|:-----|:----|
| Atoms | Singles | 더 이상 쪼개질 수 없는 최소한의 단위 | Button, InputBox 등 |
| Molecules | Combines | Single 컴포넌트가 최소 2개 이상 조합됨 | InputGroup 등 |
| Organisms | Complexes | 최소 1개 이상의 Combine 컴포넌트와 함께 다른 Single, Combine 컴포넌트가 조합됨. Complex 컴포넌트 끼리 조합되어도 이 곳에 둔다. | |
| Templates | Containers | 하나의 업무를 구성할 수 있는 단위. Store 에서 State 및 Dispatch 한다. | |
| Pages | Pages | 하나의 화면을 구성할 수 있는 단위. Query 및 URL Parameter 를 Container 에게 전달 할 수 있다. | |

한편, 프로젝트 진행 시 아래와 같은 조건일 경우 Single Component 로 간주 한다.
- JSX 및 styled-component 를 이용하여 직접 만들어진 컴포넌트. 다른 컴포넌트를 가져와 쓰지 않는다.
- 또는 이미 잘 만들어진 3rd party library 를 가져와 쓰되 내부 프로젝트에서 작성된 Component 를 직접적으로 쓰지 아니한다.

## Project Structure
아래는 Single Page Application (이하 SPA) 기준 프로젝트 구조이다.

* environments - 환경변수 파일들. 개발/빌드 진행 시 환경에 알맞은 파일이 /.env 파일로 복사 되어 쓰인다.
  - .env.dev - 개발용 환경변수
  - .env.production - 운영서버용 환경변수
  - .env.test - 개발서버용 환경변수
* gulp
  - css-sprite - CSS Background Image Sprite 를 생성하는 스크립트
    - images - 스프라이트를 생성 할 아이콘 파일(.png)을 두는 곳. 하위 폴더를 두면 생성 뒤 테스트 페이지(sprite-test.html) 에서 그룹별로 나눠진 모습을 볼 수 있다. (실제 아이콘 클래스명에는 영향을 끼치지 않음) 그리고 하위 폴더나 파일명 앞에 @를 붙이면 스프라이트 생성 시 무시(ignore) 한다.
  - s3-upload - AWS S3 에 빌드된 내용을 업로드 하는 스크립트. 수행 시 awsaccess.json 내용을 참고 한다.
* public - 정적 파일 모음
  - sprite - 이미지 스프라이트가 포함된 폴더. 배포전에 sprite.png 파일을 https://tinypng.com/ 에서 반드시 최적화 시키도록 한다.
  - index.html - 프로젝트가 시작되는 html 파일
* src
  - common - 모든 페이지에 공통으로 쓰이는 것들.
    - app.config.ts - 앱 내 환경설정. 상기 environments 의 내용을 따른다.
  - modules - 앱 내 각종 모듈을 정의 해 놓은 곳.
    - _shared - 공통으로 쓰이는 각종 컴포넌트 및 서비스가 모여있는 모듈
    - root - 루트 (/) 경로에서 쓰이는 내용
    - {feature} - 특정 페이지에서만 쓰이는 모듈
      - components - 각종 React Component 가 위치한다.
        - _complex - 복합 컴포넌트 (Molecule)
        - _combine - 조합 컴포넌트 (Organism)
        - _single - 단일 컴포넌트 (Atom)
      - containers - 컨테이너 컴포넌트 모음
      - pages - 페이지 컴포넌트 모음
      - contexts - 특정 컴포넌트들에 자료를 공유 할 때 쓰이는 Contenxt를 정의 해 둔 곳. 가급적 Flux Architecture를 활용하되 특수한 경우 (예: resizing 여부 전파)만 사용한다.
      - hooks - Custom Hook 함수 모음
      - hoc - High Order Component 모음
      - adaptives - 적응형 (Adaptive) 컴포넌트 모음. 사용자 Device 나 해상도에 따라 기능이 달라지는 컴포넌트가 필요할 때 쓰인다.
      - actions - redux 액션들
      - effects - redux 의 side effect 를 처리하기 위한 이펙트 함수 모음
      - reducers - redux 의 state 를 변경하기 위한 역할인 리듀서 모음
      - selectors - redux 의 state 값을 받아오기위한 셀렉터 모음
      - models - 현재 모듈에서만 쓰이는 모델들을 정의 해 둔다.
      - constants - 현재 모듈에서만 쓰이는 상수를 정의 해 둔다. 메시지 내용은 아래 messages 에 정의 한다.
      - messages - 사용자 UI에 출력되는 각종 메시지 상수를 정의 해 둔다.
      - services - 데이터 처리를 위한 각종 함수 및 객체 모음. API Service 도 여기에 포함된다.
      - styles - 현재 모듈에서만 사용되는 CSS Style 을 정의한다. 일반적으론 styled-components 를 사용하기에 쓸일은 없지만, 유사한 스타일이 반복될 경우 여기에 별도 스타일을 두어 사용한다.
  - factories - 개체를 생성하는 Factory 를 모아둔 곳.
  - routes - 라우팅 세팅을 하는 곳.
  - utils - 프로젝트 전반적으로 고루 쓰이는 각종 유틸리티 함수가 모인 곳.

## Component
UI 컴포넌트는 가능한한 **함수형 컴포넌트 (Functional Component)**로 작성하며 hooks 를 적극 이용하여 작성하는 것을 기본으로 한다.

또한 컴포넌트는 아래와 같이 2가지만 책임지도록 한다.

1. 특정 내용을 출력한다.
  - 텍스트 및 이미지 출력
  - 스타일 변경 및 애니메이션
2. 사용자 입력을 받아들이고 외부에 방출(emit) 한다.

또 한 아래와 같은 사례를 주의 한다.

1. props 로 들어온 데이터는 변경하지 않는다. 즉 있는 그대로 표현한다.
  - 단, format 변경등은 이뤄질 수 있다.
2. 외부에서 특정 컴포넌트에 대하여 직접적인 영향을 끼치지 않는다.
  - 외부 Wrap 이나 부모 요소에서 강제로 스타일 변경 금지
  - 외부 개입 금지
  - 이런게 필요하다면 별도 props 로 추가 하거나 기능을 만들 것.

참고로 본 섹션에서의 컴포넌트란, Container, Page 에서 쓰이는 컴포넌트가 아닌 일반적인 UI 를 구성하는 컴포넌트를 말한다.

### Properties
JSX는 기본적으로 XML의 문법을 기반으로 한다.

따라서 아래와 같이 문자열(String)로 값을 받는 프로퍼티를 상수로 전달 시 아래와 같이 2중 따옴표 (double quotation)로 작성 한다.

```tsx
const Sample: FC = () => (
  <div className="sample-wrap">
    {/* ... */}
  </div>
);
```

### Props Interface
React 를 TypeScript 와 함께 운용할 경우 컴포넌트의 속성(Properties - 이하 Props) React 의 propTypes 대신 Generic 으로 선언하여 사용할 수 있다.
따라서 아래와 같이 컴포넌트 속성은 interface 로 선언한 뒤 제네릭을 적용한다.

```jsx
// 이전 jsx 방식

const Comp = props => (
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

const Comp:FC<Props> = props => (
  <>
    <div>
      {props.name} : {props.age}
    </div>
    <button type="button" onClick={props.onClick} />
  </>
);
```

### Wrap
특별한 사유가 없다면, Wrap 은 **컴포넌트 내 스타일을 정의할 수 있는 유일한 컴포넌트** 이다.

Wrap 은 styled components 를 이용해 작성하며, 다음과 같이 작성 할 수 있다.

```tsx
const Wrap = styled.div`
  ${normalBorderStyle}
  padding: 0.5em 0.75em;
  border-radius: 3px;
`;

export const Test: FC<Props> = props => (
  <Wrap>
    {/* 컴포넌트 내용.. */}
  </Wrap>
);
```

Wrap 은 그 원래 용어처럼, 다른 요소를 감쌀 수 있는 요소로 이뤄져야 한다.

단, input 태그 같이 다른 것을 감쌀 수 없는 요소는 아래와 같이 기존 요소명을 따른다.

```tsx
const Input = styled.input`
`;
export const TestInput: FC<Props> = props => (
  <Input {...props} />
);
```

### Button
버튼 컴포넌트의 주요 역할은 다음과 같다.

* 서밋(submit) 여부
* 눈에 띄는 색상 설정 (default, primary 혹은 secondary 설정)
* 비활성화 여부
* 사용자가 클릭(혹은 터치) 했는지의 여부

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
  color?: string | 'primary' | 'secondary';
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

export const Button: React.FC<ButtonComponentProps> = props => (
  <StyledButton type={props.submit ? 'submit' : 'button'} theme={props.color} disabled={props.disabled} onClick={props.onClick}>
    {props.children}
  </StyledButton>
);
```

### Text Input

입력 요소의 공통점은 사용자가 키보드로 직접 입력한다는 것이다.

입력 후 그 내용이 어딘가 반영되어야 하지만, 그 것이 어디에 반영되어야 하는지의 여부는 최소한의 정보만을 전달 하도록 한다.

즉, 특정 업무 내 사용자의 모든 입력 내용이 Object 혹은 HashMap, Dictionary 형태라 가정하면, 하나의 입력 요소는 단 하나의 key 와 value 쌍 (**key value pair - KVP**) 에만 관여하면 된다 가정하고 UI를 구성한다.

따라서 최소한의 구성 요소는 다음과 같다.

* 입력 명칭 (name) - 이 값이 어디에 쓰어야 하는지 알려준다. KVP 에서 key 에 해당 함.
* 입력 값 (value) - 변경된 값이다. KVP 에서 value 에 해당 함.
* 이벤트 - 사용자가 정상적으로 입력한 값에 대하여 외부에 알려준다.

여기서 name 과 values 는 html 내 다양한 입력 요소의 name 및 value property 를 이용하면 되며, 전반적인 컴포넌트 에 쓰이는 Props 는 아래와 같이 명시된 InputComponentProps 를 따르도록 한다.

```tsx
export interface InputComponentProps {
  autoCompleteOff?: boolean;
  id?: string;
  label?: string;
  name: string;
  value: string;
  className?: string;
  placeholder?: string;
  disabled?: boolean;
  options?: SelectOption[];
  min?: string | number;
  max?: string | number;
  maxlen?: number;
  pattern?: string;
  onChange?: (args: InputChangeArgs) => void;
}
```

이벤트는 아래와 같은 InputChangeArgs 객체를 callback에 전달 하도록 한다.

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
export const InputText: FC<InputComponentProps> = props => {
  const handleChange = (event: any) => {
    const {
      name,
      onChange
    } = this.props;
    const value = event.target.value;

    if (onChange) {
      onChange({
        name,
        value,
      });
    }
  };

  return (
    <Wrap
      type="text"
      autoComplete={props.autoCompleteOff ? 'off' : 'on'}
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

* 명칭 (name)
* 체크 여부 (checked)
* 비활성화 여부 (disabled)
* 변경 이벤트

구현 시 아래와 같이 명시된 CheckboxComponentProps 를 이용 한다.

그리고 이벤트 결과는 다음과 같이 CheckedChangeArgs 를 callback 에 전달 하도록 한다.

```ts
/**
 * 체크형 컴포넌트의 변경 이벤트 객체.
 */
export interface CheckedChangeArgs {
  /**
   * 컴포넌트의 일음.
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
  text: string;
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

export const SampleContainer: FC = props => {
  // 리듀서에 액션을 디스패치 하기 위한 함수
  const dispatch = useDispatch();
  // 셀렉터로 데이터를 가져온다
  const value = useSelect(selValue);

  // 비동기로 수행되는 액션은 이펙트를 사용
  const handleInit = () => dispatch(effLoadData());
  // 동기적으로 수행되는 일반 액션 사용
  const handleChange = (args: DisplayPanelChangeArgs) => dispatch(actDisplayChange(args));

  return (
    <Wrapper onInit={handleInit}>
      <DisplayPanel
        searchQuery={props.searchQuery}
        onChange={handleChange}
      >
        value = {value}
      </DisplayPanel>
    </Wrapper>
  );
}
```

위와 같이 함수형 컴포넌트에 hook을 활용한다.

이벤트 헨들러 이용 시 전달되는 이벤트 프로퍼티명을 기준으로 **on**을 **handler**로 바꾸어 헨들러를 명시하여 사용한다.

만약 외부 Page Component 에서 받아올 파라미터가 필요하다면 위와 같이 Props 인터페이스를 선언하여 사용한다.

## Page

페이지는 필요한 컨테이너를 모아서 최종적으로 사용자 화면에 표현하는 역할을 맡는다.

동시에 Query Parameter 나 Url Parameter 를 받아서 필요한 컨테이너에게 전달하는 역할도 맡는다.

페이지는 레이아웃을 가질 수 있으며 그 레이아웃 컴포넌트는 미리 정의된 PageContainer 를 이용토록 한다.

파라미터 처리는 아래와 같이 [react-router 에서 제공되는 hooks](https://reacttraining.com/react-router/web/api/Hooks/useparams) 를 사용한다.

```tsx
// 파라미터 인터페이스 정의
interface QueryParams {
  id?: string;
  name?: string;
}

const SamplePage: FC = () => {
  const {
    id, name,
  } = useParams<QueryParams>(); // 제네릭 선언하여 타입 유추가 되도록 한다.

  return (
    <PageContainer title="샘플 페이지">
      id: {id}<br/>
      name: {name}
    </PageContainer>
  );
}
```

만약 [next.js](https://nextjs.org/docs/api-reference/next/router#userouter) 라면 아래와 같은 방법으로 hook을 쓰도록 한다.

```tsx
// 파라미터 인터페이스 정의
interface QueryParams {
  id?: string;
  name?: string;
}

const SamplePage: FC = () => {
  const {
    id, name,
  } = useRouter().query as QueryParams; // 타입 변환하여 사용한다.

  return (
    <PageContainer title="샘플 페이지">
      id: {id}<br/>
      name: {name}
    </PageContainer>
  );
}
```

## Component Life Cycle
React 컴포넌트는 생명 주기를 가지는데, 이 때 작성 시 주의할 점을 서술한다.

### setTimeout

컴포넌트 내부에서 setTimeout 을 이용 할 때가 있다. 이 때는 아래와 같이 self property 로 _t 를 주고, unmount 할 때 clearTimeout 을 반드시 수행 시켜 준다.

```tsx
// 컴포넌트가 렌더링되고 1초뒤 무언가를 수행할 때의 예제

const TestTimer: FC = () => {
  const [
    timerId,
    setTimerId,
  ] = useState(0);

  useEffect(() => {
    setTimerId(setTimeout(() => doSomthing(), 1000));

    return () => {
      clearTimeout(timerId);
    };
  });
};
```

### DOM Reference

컴포넌트 작성 시 DOM 참조를 해야 할 경우, 아래와 같이 useRef 를 사용한다.

```tsx
const TestRef: FC = () => {
  // 레퍼런스 객체는 앞에 'ref' 를 접두어로 붙여서 쓴다.
  const refWrap = useRef();

  return (
    <div ref={refWrap}>
      테스트 ^^
    </div>
  );
}
```
