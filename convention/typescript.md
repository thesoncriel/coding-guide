# TypeScript

본 가이드는 TS 이용 시 필요한 코드 규칙을 서술한다.

기본적으로 React 와 함께 사용 한다는 조건하에 작성 되어 있다.

## 읽어보기

가이드를 읽기전 미리 확인하면 좋다.

- [TypeScript - intro](https://poiemaweb.com/typescript-introduction)
- [타입스크립트, 써야할까?](https://hyunseob.github.io/2018/08/12/do-you-need-to-use-ts/)
- [TypeScript 현업 적용 후기](https://medium.com/tapjoykorea/typescript-%ED%98%84%EC%97%85-%EC%A0%81%EC%9A%A9-%ED%9B%84%EA%B8%B0-caad266c8142)

## TSLint

개발 진행 시 tslint 를 기본적으로 이용하며,

linting library 는 다음과 같다.

- tslint:latest - 기본 라이브러리. 항상 최신으로 유지.
- tslint-react - react 문법이 추가됨.
- tslint-react-hooks - hook 문법이 추가됨.

작업 시 project root 에 tslint.json 을 만들고 다음 내용을 복사해서 쓰도록 한다.

예외 사항에 대한 사유가 주석으로 추가되어 있으니 함께 참고 한다.

**eslint**가 tslint 기능을 포함하게 되어 업데이트 필요.

## 타입 명시

TS를 사용하는 가장 근본적인 이유는 타입을 명시하여 코드 가독성을 높이는 것이다.

그러므로 아래와 같이 any 타입을 쓰는 경우는 특수한 경우를 제외하면 사용하지 않는다.

```ts
// wrong
const fn = (args: any) => args.min + args.max;

// good
const fn = (args: { min: number; max: number }) => args.min + args.max;

// best
interface FnArgs {
  min: number;
  max: number;
}
const fn = (args: FnArgs) => args.min + args.max;
```

## 모델 인터페이스 작성

Model 이용 시 구조체(struct)대용으로 인터페이스로 선언하여 자주 이용된다.

이 때 아래와 같은 Sub Object 는 반드시 별도로 interface를 명시하여 사용한다.

```ts
// wrong
interface Model {
  items: Array<{ age: number; name: string }>;
  user: {
    id: string;
    name: string;
    info: {
      address: string;
      school?: string;
    };
  };
}
```

```ts
// good
interface Item {
  age: number;
  name: string;
}

interface UserInfo {
  address: string;
  school?: string;
}

interface User {
  id: string;
  name: string;
  info: UserInfo;
}

interface Model {
  items: Item[];
  user: User;
}
```

### 모델 주석

모델은 아래와 같이 모델의 쓰임새와 각 프로퍼티(property)에 대한 주석(comment)이 있어야 한다.

```ts
/**
 * 결제 UI 모델
 */
interface PaymentUiModel {
  /**
   * 결제 가격
   */
  amount: number;
  /**
   * 제품ID
   */
  productId: string;
}
```

만약 모델이 서버에서 전달된 것이면 아래와 같이 **해당 모델의 API 문서 주소**를 함께 주석에 명시하도록 한다.

```ts
/**
 * 결제 UI 모델
 *
 * https://www.google.com
 */
interface PaymentRes {
  /**
   * 결제 가격
   */
  amount: number;
  /**
   * 제품ID
   */
  productId: string;
}
```

## API Service Rules

외부 데이터를 이용함에 있어 프론트엔드 기준, ajax 를 애용하게 된다. 이 때 API 라 불리우는 서비스 모듈을 만들어 사용하게 되는데 아래와 같은 규칙을 지키고 고려 하도록 한다.

1. 서비스를 이용하는 곳 (예: Action, Effect 등)에서 그 데이터의 출처를 굳이 알지 않아도 되도록 충분히 추상화 한다.
   - 즉 출처가 Backend Server, CDN Server, Static File 및 Browser Cache / Storage 여부는 사용처에서는 알 필요가 없으며 관여하지 않는다.
2. 브라우저 캐시 (localStorage, sessionStorage, cookie-비권장 등) 관리도 API 서비스에서 맡는다.
3. 각 기본 api (base api)는 필요한 만큼 만들어 사용한다.
4. 각 기본 api 에 필요한 헤더는 용도에 맞게 만들어 사용 해야한다.
   - 보안 토큰여부, 앱 출처 내용, 업로드 시 필요한 내용 등

## Base API

프로젝트 내 특정 업무를 위한 API 서비스 작성 시 필요한 용도별 Base API 를 작성해야 하며, 이에 대해 설명 한다.

### IHttpApi Interface

API 서비스는 모두 공통 기본 api (base api)를 기반으로 동작 되어야 한다.

서비스가 RESTful로 HTTP 통신을 이용함에 있어, 필요한 기본 동작 내용을 interface 로 정의해 놓았으며 주요 기능은 다음 내용을 정의하여 사용한다.

- get
- post
- put
- delete
- postUpload
- putUpload
- getFile

### Base API Creation

api 서비스를 다루는 base api 서비스는 다음과 같이 **IHttpApi** 인터페이스를 구현한 것을 사용한다.

```ts
/**
 * HTTP 프로토콜을 이용하여 비동기 통신을 수행한다.
 */
export interface IHttpApi {
  /**
   * GET 메서드 호출
   * @param url 호출 경로
   * @param params 전달할 파라미터
   */
  get<T = any>(url: string, params?: any): Promise<T>;
  /**
   * POST 메서드 호출
   * @param url 호출 경로. 만약 쿼리 파라미터가 포함된다면 이 곳에 추가적으로 명시 해 주어야 한다.
   * @param data body 요청으로 보낼 데이터. json 으로 바꿔서 보내게 된다.
   */
  post<T = any>(url: string, data?: any): Promise<T>;
  /**
   * PUT 메서드 호출
   * @param url 호출 경로. 만약 쿼리 파라미터가 포함된다면 이 곳에 추가적으로 명시 해 주어야 한다.
   * @param data body 요청으로 보낼 데이터. json 으로 바꿔서 보내게 된다.
   */
  put<T = any>(url: string, data?: any): Promise<T>;
  /**
   * DELETE 메서드 호출
   * @param url 호출 경로
   * @param params 전달할 파라미터
   */
  delete<T = any>(url: string, params?: any): Promise<T>;
  /**
   * POST 메서드로 업로드 한다.
   * @param url 업로드 경로
   * @param data 업로드에 쓰이는 데이터
   * @param progCallback 업로드 상황을 보내주는 콜백
   */
  postUpload<T = any>(
    url: string,
    data: any,
    progCallback?: (args: UploadStateArgs) => void
  ): Promise<T>;
  /**
   * PUT 메서드로 업로드 한다.
   * @param url 업로드 경로
   * @param data 업로드에 쓰이는 데이터
   * @param progCallback 업로드 상황을 보내주는 콜백
   */
  putUpload<T = any>(
    url: string,
    data: any,
    progCallback?: (args: UploadStateArgs) => void
  ): Promise<T>;
  /**
   * GET 메서드로 파일을 비동기로 가져온다.
   * @param url 파일을 가져올 경로
   * @param params 전달할 파라미터
   * @param filename 파일이 받아졌을 때 쓰여질 파일명.
   */
  getFile(url: string, params?: any, filename?: string): Promise<File>;
}
```

기본 api 서비스 생성은 다음과 같은 apiFactory 를 이용한다.

```ts
/**
 * 헤더 제공자. 인자 없는 함수로써, 수행결과가 해시맵 형태로 제공 되어야 한다.
 */
type HeaderStrategy = () => HashMap<string>;

/**
 * @param baseUrl http 혹은 https 로 시작되는 도메인 네임(혹은 IP) 기반의 웹 주소.
 * @param headerStrategy 본 API 서비스 수행 시 포함될 헤더 정보.
 */
const apiFactory: (
  baseUrl: string,
  headerStrategy?: HeaderStrategy
) => IHttpApi;
```

그 출처 URL은 환경변수 설정 내용(ex: appConfig)을 이용한다.

```ts
import appConfig from "../../common/app.config";
import { apiFactory } from "../../factories/api.factory";

/**
 * 프로젝트에서 쓰이는 기본 호출 서비스.
 */
export const baseApi: IHttpApi = apiFactory(appConfig.apiUrl);
```

보안 토큰이 필요한 경우는 다음과 같이 작성 한다.

```ts
/**
 * 베어러 토큰이 필요한 헤더를 만든다.
 * SSR 환경이 아닐 때의 예제.
 */
const createAuthTokenHeader: HeaderStrategy = () => ({
  authToken: sessionStorage.getItem("auth_token")
});

/**
 * 프로젝트에서 쓰이는 보안토큰이 들어간 호출 서비스.
 */
export const authApi = apiFactory(appConfig.apiUrl, createAuthTokenHeader);
```

만약 기본 api 서비스의 출처가 cdn 일 경우는 다음과 같이 한다.

```ts
/**
 * 특정 CDN 서버의 파일을 호출하는 서비스.
 */
export const cdnApi = apiFactory(appConfig.cdnUrl);
```

base api 는 작성 될 모든 업무별 api 의 기본이되는 것이므로 아래와 같은 경로에만 작성 한다.

- src/services/api/base.ts

## API Service

만들어진 base api 를 기반으로 필요한 API 업무를 구성할 수 있다.

각 API 는 다음과 같은 출처를 가진 구성을 가진다.

- RESTful HTTP Method
  - get, post, put, delete
- cdn - cdn 서버의 정적 자료. get 으로 불러 온다. (json, xml 등)
- static - 현재 서버의 정적 자료. get 으로 불러 온다. (json, xml 등)
- file - 로컬 혹은 서버의 정적 자료. 보통 get 으로 불러오나 때에 따라 post 를 이용할 수도 있다. (이미지, 워드, 엑셀등 2진 파일, - csv 불러올 때도 해당 됨)
- cache - 로컬 내 스토리지, 쿠키 및 메모리 데이터.

### Method Naming

API 서비스 객체를 구성하는 각 메서드는 다음 도표를 따른다.

| verb    | desc.              | examples        | method type             | note                                                                                                 |
| :------ | :----------------- | :-------------- | :---------------------- | :--------------------------------------------------------------------------------------------------- |
| load    | 자료를 불러온다.   | loadUserInfo    | get, cdn, static, cache |                                                                                                      |
| save    | 자료를 저장한다.   | saveTemp        | put, post, cache        |                                                                                                      |
| clear   | 자료를 소거한다.   | clearTemp       | delete, cache           |                                                                                                      |
| send    | 자료를 전달한다.   | sendPayment     | put, post               | 제 3자가 내용을 그대로 보거나 후속 통신이 이뤄짐. 파일 업로드와 각종 정보가 함께 전달될 때도 쓰인다. |
| regist  | 자료를 등록한다.   | registMusicItem | post                    | 신규로 등록 할 때만 쓰인다.                                                                          |
| remove  | 자료를 삭제한다.   | removeCartData  | delete, put, post       |                                                                                                      |
| add     | 자료를 추가한다.   | addCornItem     | put, post               |                                                                                                      |
| update  | 자료를 수정한다.   | updateProfile   | put, post               |                                                                                                      |
| read    | 파일을 읽어온다.   | readImageFile   | file                    | 읽은 내용은 File 객체로 넘겨야 한다.                                                                 |
| upload  | 파일을 업로드한다. | uploadProfile   | post, put               | 오직 파일 업로드에만 쓰인다.                                                                         |
| check   | 상태를 확인한다.   | checkEmail      | get, post               | Email 체크, 중복 ID 체크 등                                                                          |
| signin  | 로그인 한다.       |                 | post                    |                                                                                                      |
| signup  | 가입한다.          |                 | post                    |                                                                                                      |
| signout | 로그아웃 한다.     |                 | any                     |                                                                                                      |

다른 것이 더 필요하다면 Naming Convention 을 참고 한다.

### Specified Types

base api 수행 후엔 반드시 generic 을 지정하여 어떠한 타입을 기대할 수 있는지 명시한다.

```ts
export const sampleApi = {
  getTestSample() {
    return staticApi.get<ResData>("/test/sample.json");
  }
};
```

만약 API 결과물이 List 형태일 경우 아래와 같은 인터페이스(ListRes)를 선언하여 공통으로 활용토록 한다.

```ts
// ListRes 예시
interface ListRes<T> {
  items: T[];
  skip: number;
  limit: number;
}

// API Service
const api = {
  loadTestList(params: Params) {
    return authApi.get<ListRes<BillDataResItem>>("/test/list", params);
  }
};
```

업로드가 이뤄지는 API 라면 다음과 같이 callback 을 받을 수 있도록 만든다.

이 때 업로드 상태를 확인할 수 있는 인터페이스(UploadStateArgs) 를 넘겨주어야 한다.

```ts
// UploadStateArgs 예시
/**
 * 업로드 상태를 확인할 수 있는 객체.
 */
export interface UploadStateArgs {
  /**
   * 진행도. 0~100 까지의 수치.
   */
  progress: number;
  /**
   * 업로드된 바이트수.
   */
  loaded: number;
  /**
   * 업로드 해야 할 총 바이트수.
   */
  total: number;
  /**
   * 완료여부.
   */
  completed: boolean;
}

// API Service
const api = {
  sendBill(params: Params, callback?: (args: UploadStateArgs) => void) {
    return uploadApi.postUpload<void>("/test/create", params, callback);
  }
};
```

### 서비스 사용 예시

상기 언급된 내용들을 바탕으로 실제 사용하는 예시 코드 이다.

React & Redux 기준이며, reducer 는 생략 하였다.

```tsx
// 파라미터 모델 작성
interface Params {
  keyword: string;
}

// 토큰 제공자 작성
const tokenStrategy = () => ({
  authToken: sessionStorage.getItem("auth_token")
});

// 기본 API 작성
const normalApi: IHttpApi = apiFactory("https://api.theson.kr", tokenStrategy);

// 서비스 작성
const exApi = {
  loadList(params: Params) {
    return normalApi.get<ListRes<ExItem>>("/exp/list", params);
  }
};

// 액션 작성
const actExListLoad = createAction("EX_LIST_LOAD")<ListRes<ExItem>>();

const actExListLoadFail = createAction("EX_LIST_LOAD_FAIL")<Error>();

// 이렉트 처리 함수 작성
const effListLoad = createEffect((params: Params, dispatch) => {
  disaptch({ type: EX_LIST_LOAD });

  exApi
    .loadList(params)
    .then(payload => disaptch(actExListLoad(payload)))
    .catch(err => dispatch(actExListLoadFail(err)));
});

// ListContainer 예시
export const ListContainer: FC = () => {
  const list = useSelector(state => state.list);
  const dispatch = useDispatch();

  useEffect(() => dispatch(effListLoad({ keyword: "theson" })));

  return (
    <ul>
      {list.map((item, idx) => (
        <li key={idx}>{item}</li>
      ))}
    </ul>
  );
};
```

## Component Exporting

구성된 모듈이나 폴더 내 반드시 **index.ts** 를 구성하여 외부에서 index 로만 import 할 수 있도록 모듈화 시킨다.

작성된 컴포넌트를 export 구문으로 넘길 때는 default 를 쓰지 않는다. 까닭은 default 로 할 경우, index 에서 export 하는 구문이 복잡 해 질 수 있기 때문이다.

(Component, Container, Page 모두 해당 된다.)

아래는 atomic design 에서 single(atom) 을 맡는 구성요소에 대한 예시이다.

```tsx
// src/components/sample/_single/Loading.tsx

import React from "react";
import styled from "styled-components";

interface Props {
  show?: boolean;
}

const Wrap = styled.div`
  background: #eee;
`;

export const Loading: React.FunctionComponent<Props> = props =>
  props.show ? <Wrap>Loading...</Wrap> : null;

// src/components/sample/_single/Button.tsx
export const Button: FC<ButtonComponentProps> = props => (
  <Wrap
    type={props.submit ? "submit" : "button"}
    theme={props.color}
    disabled={props.disabled}
    onClick={props.onClick}
  >
    {props.children}
  </Wrap>
);
```

\_single 폴더 내 index.

```ts
// src/components/sample/_single/index.ts

export * from "./Loading";
export * from "./Button";
```

sample 폴더 내 index.

```ts
// src/components/sample/index.ts

export * from "./_single";
export * from "./_combine";
export * from "./_complex";
```

Container 에서 실제 사용 예제. 몇몇 코드는 생략 되어 있다.

```tsx
import React, { FC } from "react";
import { useSelector, useDispatch } from "react-redux";
import {
  Button,
  Loading,
  SampleList,
  SampleIcon
} from "../../components/sample"; // index 를 이용함으로써 import 구문이 단순해진다.

export const ContainerComponent: FC = () => {
  const disaptch = useDispatch();
  const loading = useSelector(state => state.loading);
  const list = useSelector(state => state.list);
  const totalCount = useSelector(state => state.totalCount);
  const handleLoading = () => dispatch(actListLoad());

  useEffect(() => {
    handleLoading();

    return () => dispatch(actListClear());
  });

  return (
    <>
      <Button disabled={loading} color="primary" onClick={handleLoading}>
        <SampleIcon name="refresh" />
        다시 불러오기
      </Button>
      <SampleList list={list} totalCount={totalCount} />
      <Loading show={loading} />
    </>
  );
};
```
