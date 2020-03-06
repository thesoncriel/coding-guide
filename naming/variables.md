# Variables

본 문서는 각종 변수명에 쓰이는 명칭을 정리 해 놓은 것이다.

프로그램 작성 시 필요한 내용을 검색해서 사용하자.

## Local Variable

로컬 변수는 기본적으로 [camel-case](https://zetawiki.com/wiki/%EC%B9%B4%EB%A9%9C%ED%91%9C%EA%B8%B0%EB%B2%95_camelCase,_%ED%8C%8C%EC%8A%A4%EC%B9%BC%ED%91%9C%EA%B8%B0%EB%B2%95_PascalCase) 로 작성한다.

### 타입 및 접두어/접미어별 정리

사용 시 특별히 언급이 없다면 [Hungarian-notation](https://ko.wikipedia.org/wiki/%ED%97%9D%EA%B0%80%EB%A6%AC%EC%95%88_%ED%91%9C%EA%B8%B0%EB%B2%95) 으로 작성한다.

#### Primitive Type

| prefix                                                  | type                 | desc.                         | examples                      | note                                                                                |
| :------------------------------------------------------ | :------------------- | :---------------------------- | :---------------------------- | :---------------------------------------------------------------------------------- |
| s<br/>name                                              | string               | 문자열                        | sName<br/>strMyCase           | name 은 단독으로 사용 가능                                                          |
| key<br/>k                                               | string               | 키/값 쌍에 쓰이는 키          | key                           |                                                                                     |
| msg                                                     | string               | 사용자에게 알려줄 메시지      | msgInvalid                    |                                                                                     |
| i<br/>num                                               | number<br/>(integer) | 정수                          | iAge<br/>iWidth               | 정수만 넣어준다.<br/>i 로 단독으로 쓰이는 것은 for, while 같은 반복문에만 사용한다. |
| idx<br/>index                                           | number<br/>(integer) | 인덱스                        | idx<br/>index                 | 객체나 배열의 반복 기능 수행 시 전달되는 인덱스값. 0부터 시작한다.                  |
| seq                                                     | number<br/>(integer) | 시퀀스 (Sequence). 혹은 순번. | seq                           | 특정 목록의 순번을 의미한다. 1부터 시작된다.                                        |
| cnt                                                     | number<br/>(integer) | 카운트 (Count)                | cntPayment<br/>cntCustomer    | 특정 로직에 의하여 횟수를 셀 때 사용한다.                                           |
| d<br/>dbl                                               | number<br/>(double)  | 실수 (Floating Number)        | dPrice<br/>dTall              | 실수만 넣어준다.                                                                    |
| b<br/>is<br/>has<br/>checked<br/>disabled<br/>activated | boolean              | 불리언. true/false 만 가능    | bRet<br/>isChild<br/>hasMoney |                                                                                     |
| loading                                                 | boolean              | 데이터 호출/변환중일 때.      | loading                       | 사용자를 기다리게 하는 모든 상황에 대해서 사용 가능.                                |

#### Collection Type

| prefix              | type                                                    | desc.                                                | examples                    | note                                                          |
| :------------------ | :------------------------------------------------------ | :--------------------------------------------------- | :-------------------------- | :------------------------------------------------------------ |
| a<br/>arr<br/>items | Array&lt;T&gt;<br/>T[]                                  | 배열 혹은 목록(List)                                 | aList<br/>arrData<br/>items | 반드시 Generic 으로 타입을 명시 해준다.                       |
| m                   | KV Object<br/>HashMap&lt;T&gt;<br/>{ [key: string]: T } | 해시맵으로 쓰여지는 객체.                            | mRet<br/>mParams            | Generic 명시 필수.<br/>또 한 명시된 타입만을 쓰도록 유의한다. |
| iter                | Iterator<br/>Iterable                                   | 반복자 객체. Generator 를 통해 만들어진 객체도 포함. | iterCollection<br/>iterData |                                                               |

#### Object Type

| prefix | type             | desc.                                                              | examples                        | note                                                           |
| :----- | :--------------- | :----------------------------------------------------------------- | :------------------------------ | :------------------------------------------------------------- |
| date   | Date             | 날짜 객체                                                          | dateNow                         |                                                                |
| regex  | RegExp           | 정규표현식 객체                                                    | regexName                       |                                                                |
| blob   | Blob             | 바이너리 데이터 덩어리                                             | blobImage                       |                                                                |
| prm    | Promise&lt;T&gt; | 프라미스 객체                                                      | prmFetchData<br/>prmLoadProfile |                                                                |
| file   | File             | input file 로 가져온 파일 객체                                     | fileProfile<br/>fileExcel       |
| err    | Error            | 에러 객체.<br/>throw 구문으로 넘기거나 try~catch 에서 받아내는 값. | errResponse                     | throw 발생시 반드시 new Error() 구문으로 에러를 전달해야 한다. |

#### Function Type

| prefix   | type     | desc.                              | examples            | note                                  |
| :------- | :------- | :--------------------------------- | :------------------ | :------------------------------------ |
| fn       | Function | 로컬 변수에 할당된 함수            | fnProcess<br/>fnExe | 일반적인 함수명 정의때는 쓰지 않는다. |
| callback | Function | 특정 이벤트의 콜백으로 쓰일 함수   | callback            | 대체로 단독으로 쓰인다.               |
| pipe     | Function | Pipeline 패턴에서 쓰이는 전용 함수 | pipeHeaders         |                                       |

#### Event Arguments

하나의 이벤트엔 하나의 인수만 들어오므로 단독 명칭만을 사용한다.

만약 그렇지 못하다면 이벤트 설계를 바로잡아 하나의 객체만으로 운용하도록 변경 해야한다.

| prefix | type         | desc.                                                        | examples | note                                          |
| :----- | :----------- | :----------------------------------------------------------- | :------- | :-------------------------------------------- |
| event  | HTMLDOMEvent | DOM에서 발생된 각종 이벤트 객체                              | event    |                                               |
| args   | Object       | 주어진 기본 이벤트 인수 외 필요에 따라 만들어진 이벤트 객체. | args     | 반드시 인터페이스로 타입을 명시하고 사용한다. |

#### DOM & BOM

| prefix | type             | desc.                    | examples                | note                                                                         |
| :----- | :--------------- | :----------------------- | :---------------------- | :--------------------------------------------------------------------------- |
| doc    | DOMDocument      | HTML 혹은 XML 문서 객체  | doc<br/>docXml          |                                                                              |
| elem   | DOMElement       | HTML 혹은 XML 태그 요소  | elemWrap<br/>elemAnchor |                                                                              |
| node   | DOMNode          | HTML 혹은 XML 노드 객체  | nodeChild<br/>nodeNext  |                                                                              |
| win    | Window           | 웹브라우저의 윈도우 객체 | winOpened<br/>winDialog | SSR 일 경우 서버 환경인지 반드시 체크 하여 사용한다.                         |
| sto    | Storage&lt;T&gt; | 로컬/세션 스토리지 객체  | stoCache<br/>stoToken   | 직접적인 사용은 지양한다.<br/>대신 별도의 storageFactory 를 만들어 사용한다. |

#### Redux Boilerplates

| prefix  | suffix  | type                      | desc.            | examples                          | note                    |
| :------ | :------ | :------------------------ | :--------------- | :-------------------------------- | :---------------------- |
| act     |         | Function                  | 액션 함수        | actListLoad<br/>actUserInfoUpdate |                         |
| eff     |         | Async Function            | 이펙트 처리 함수 | effParseData<br/>effSignin        |                         |
| sel     |         | Function                  | 셀렉터 함수      | selProdList<br/>selUserInfo       |                         |
|         | Reducer | Function                  | 리듀서 함수      | sampleReducer<br/>productReducer  |                         |
| payload |         | Primitive Type<br/>Object | 페이로드 객체    | payload                           | 단독 명칭으로 사용한다. |

#### Rx.js

비동기 데이터 스트림 라이브러리인 ReactiveX 는 본 섹션을 참고한다.

| prefix | suffix | type                | desc.                                        | examples                        | note |
| :----- | :----- | :------------------ | :------------------------------------------- | :------------------------------ | ---- |
| obs    | \$     | Observable&lt;T&gt; | 구독 가능한 옵저버블 객체.                   | obsValue<br/>value$<br/>items$  |      |
| sub    |        | Subscription        | 구독된 후 구독 중지가 필요할 때 사용될 객체. | subValue<br/>subItems           |      |
| sbj    |        | Subject&lt;T&gt;    | 구독 가능한 서브젝트 객체.                   | sbjPageEvent<br/>sbjFetchedData |      |
| oper   |        | Function            | Rx 오퍼레이터로 이용 가능한 커스텀 함수.     | operParseData                   |      |

#### Styled Components

SC작성 시 아래와 같은 명명 규칙을 따른다.

| prefix                                         | suffix | type                       | desc.                      | examples               | note                                                          |
| :--------------------------------------------- | :----- | :------------------------- | :------------------------- | :--------------------- | ------------------------------------------------------------- |
| css                                            |        | FlattenSimpleInterpolation | SC 에서 스타일만 담은 객체 | cssBlock<br/>cssButton | 반드시 SC의 css 함수를 통해서 만들어져야 한다.                |
| Wrap<br/>Anchor<br/>Button<br/>Input<br/>etc.. |        | StyledComponent            | 스타일이 적용된 컴포넌트.  |                        | 필요 시 각 태그 요소의 명칭을 Pascal Case 로 바꾸어 사용한다. |

div, span 등 container 역할을 맡을 수 있는 요소로 만들경우 그 SC 명칭은 Wrap 으로 통일한다.

```tsx
const Wrap = styled.div`
  border: 1px solid #888;
`;

export const BorderedBox: FC = () => <Wrap>테두리 박스</Wrap>;
```

button, input, anchor 등 단독으로 쓰일 경우엔 각 태그 요소 명칭을 Pascal Case 로 바꿔서 사용한다.

```tsx
const Input = styled.input`
  &:focus {
    outline: 1px solid blue;
  }
`;

export const BlueFocusCheckbox: FC = () => <Input checked={true} />;
```

```tsx
const Anchor = styled.a`
  &:hover {
    color: red;
  }
`;

export const RedHoverLink: FC = () => (
  <Anchor href="https://www.google.com">구글로 가자!</Anchor>
);
```

한편 SC는 그 것이 사용되는 파일내에 두되, 아래와 같이 내부 스타일 코드 길어지면 파일을 분리하고 import 하여 사용한다.

```tsx
// Before

// CenteredBox.tsx
const Wrap = styled.section`
  width: 100%;
  max-width: 800px;
  margin: 0 auto;
  /* ...codes */
  /* about 100 line lol~ */
`;

export const CenteredBox: FC = ({ children }) => <Wrap>{children}</Wrap>;
```

```tsx
// After

// CenteredBoxStyles.tsx
export const CenteredBoxWrap = styled.section`
  width: 100%;
  max-width: 800px;
  margin: 0 auto;
  /* ...codes */
  /* about 100 lines */
`;

// CenteredBox.tsx
import { CenteredBoxWrap } from "./CenteredBoxWrap";

export const CenteredBox: FC = ({ children }) => (
  <CenteredBoxWrap>{children}</CenteredBoxWrap>
);
```

#### ETC

| suffix | type     | desc.                                                                     | examples               | note                                                                                             |
| :----- | :------- | :------------------------------------------------------------------------ | :--------------------- | ------------------------------------------------------------------------------------------------ |
| Api    | IHttpApi | 외부 Http API를 호출하는 서비스 객체                                      | baseApi<br/>uploadApi  | 생성시 IHttpApi 인터페이스 기능을 준수하는 객체를 만드는 apiFactory 를 별도로 정의하여 사용한다. |
| t      | number   | setTimeout 혹은 setInterval 호출 시 clear 수행을 위해 발급된 timer id 값. | tDelay<br/>tAfterTimer |                                                                                                  |

```ts
// Api 생성 예제
const cdnApi = apiFactory("https://cdn.sample.com");
const appApi = apiFactory("https://api.sample.com");

// timer id 이용 예제
const tTimeout = setTimeout(() => console.log("timeout!!"), 2000);

document
  .getElementById("btn_stop")
  .addEventListener("click", () => clearTimeout(tTimeout));
```

#### 상황별 정리

각 변수의 역할별로 쓰여지는 명칭을 정리 해 놓았다.

| prefix                              | suffix         | type   | desc.                                               | examples                                       | note                                                                                                     |
| :---------------------------------- | :------------- | :----- | :-------------------------------------------------- | :--------------------------------------------- | :------------------------------------------------------------------------------------------------------- |
|                                     | Ret<br/>Result | any    | 함수의 수행 결과를 바깥으로 전달하는 변수.          | iRet<br/>sRet<br/>mRet<br/>aRet<br/>dataResult | Ret 좌측에 타입을 명시하는 패턴으로 사용한다.                                                            |
| params                              | Params         | Object | Api Service 에 요청 파라미터를 보낼 때 쓰이는 자료. | params<br/>sendingParams                       | 일반적으로 params 단독으로 쓰이며 부득이하게 파라미터가 여러 변수로 존재한다면 접미사 형식으로 사용한다. |
| value<br/>values<br/>data<br/>datum |                | any    | 임시로 보관할 값.                                   | value                                          | 단독으로 쓰인다.<br/>써야 할 변수명을 특정하기 힘들거나 애매할 때 사용.                                  |

```ts
// 단독 파라미터
async function execApi(params: ExParams) {
  return await cdnApi.get<string[]>("/data.json");
}

// 파라미터 여러개
async function execApi(exParams: ExParams, innerParams: InnerParams) {
  const;
}
```
