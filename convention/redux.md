# Redux

React 에서 Flux Architecture 로 상태 관리를 맡을 수 있는 Redux 의 상세한 사용 방법을 기술한다.

## 읽어보기

가이드 확인 전, 미리 알아두면 좋을 내용을 링크 해 두었다.

* [페이스북의 결정: 그렇다면 Flux 다!](https://blog.coderifleman.com/2015/06/19/mvc-does-not-scale-use-flux-instead/)
* [Flux vs. MVC](https://dogbirdfoot.tistory.com/14)

## FSA

Redux를 TypeScript 와 조화롭게 사용하는 방법을 적용 하기위해 [FSA(Flux Standard Action)](https://github.com/acdlite/flux-standard-action) 규칙을 도입하고 준수 하도록 한다.

* [참고 글 - velog](https://velog.io/@velopert/use-typescript-and-redux-like-a-pro)

그에 따라 아래 라이브러리를 함께 포함한다.

* [typesafe-actions](https://github.com/piotrwitek/typesafe-actions): 리덕스 액션 이용 시 FSA를 활용하여 boiler plate를 줄이기 위한 라이브러리.

## Boiler Plates

Redux 에서 쓰이는 각종 요소들(boiler plate - 보일러 플레이트)은 다음과 같다.

* actions - 액션 함수를 정의 한다. Container 및 다른 액션에서 dispatch 할 때 추가적으로 사용할 수 있다.
* effects - redux-thunk 를 이용한 비동기 액션 및 그에 따른 이펙트 함수를 정의한다. 이 역시 Container 나 다른 액션에서 dispatch 에 사용될 수 있다.
* reducer - dispatch 된 액션을 바탕으로 상태(State)를 변경 한다.
* selector - 리듀서에 정의된 State 에서 필요한 값만 받아오는 함수를 정의한다.

### Actions
액션 명칭은 Camel Case 로 구성되며 다음과 같은 규칙을 따른다.

```
act{feature}{objective}{action}
```

액션 생성은 다음과 같이 typesafe-actions의 createAction 을 이용한다.

```ts
import { createAction, ActionType } from 'typesafe-actions';

const actTestListLoad = createAction('TestListLoad')();
const actTestListSucc = createAction('TestListSucc')<
  ListRes<TestItemRes>
>();
const actTestListFail = createAction('TestListFail')<
  Error
>();

const actions = {
  actTestListLoad,
  actTestListSucc,
  actTestListFail,
};

export type TestActions = ActionType<typeof actions>;
```

### Effects
주로 사이드 이펙트를 처리하기 위한 로직이 구성된 비동기 함수이다.

주요 역할은 데이터 받아오기와 같은 API 호출, 데이터 변환, 그리고 유효성 검증등이 포함된다.

redux-thunk 를 이용하여 타입 유추를 하기엔 제네릭 설정이 복잡하니 다음과 같은 함수를 정의하여 사용한다.

```ts
export type AsyncDispatch<A extends AnyAction = AnyAction> = ThunkDispatch<AppState, {}, A>;

/**
 * 비동기 액션을 수행하는 이펙트 함수를 만든다. 쿼리 인자가 필요하다.
 * - Q: 함께 전달될 페이로드 타입. 필요 없다면 generic 에 void 선언 하거나 생략한다.
 * - R: Promise 로 반환될 타입. 기본 void
 * - A: 지정할 Action 타입. 기본 AnyAction
 * @param fnProcess type 이 들어간 액션 객체를 비동기로 반환 해야 한다.
 */
export function createEffect<Q = void, R = any, A extends AnyAction = AnyAction>(fnProcess: (payload: Q, dispatch: AsyncDispatch<A>, getState: () => AppState) => Promise<R>):
  ActionCreator<ThunkAction<Promise<R>, AppState, any, A>> {
  /**
   * 액션을 수행한다.
   * @param payload
   */
  return (payload: Q) => (dispatch: AsyncDispatch<A>, getState: () => AppState) => {
    try {
      const prm = fnProcess(payload, dispatch, getState);

      if (prm instanceof Promise) {
        return prm;
      }

      return Promise.resolve(prm);
    } catch (error) {
      if (error.message && !isServer()) {
        alert(error.message);
      }
      return Promise.reject(error);
    }
  };
}
```

아래는 createEffect 사용 예제 이다.

```tsx
// 이펙트 작성
const effTestListLoad = createEffect<string>(
  async (payload, dispatch) => {
    dispatch(actTestListLoad());

    testApiService
      .loadList(payload)
      .then(res => dispatch(actTestListSucc(res)))
      .catch(error => dispatch(actTestListFail(error)));
  },
);

// 페이로드 및 비동기 데이터를 전달하는 이펙트일 경우 제네릭을 아래와 같이 2가지로 선언 한다.
// 좌측이 페이로드, 우측이 리턴 타입.
const effTestReturn = createEffect<TestPayload, TestReturn>(
  async (payload, dispatch) => {
    dispatch(actTestReturn());

    return testApiService
      .fetchSomething(payload)
      .then(res => {
        dispatch(actTestReturnSucc(res));

        return res;
      });
      // catch는 생략 - 받아들이는 곳에서 대신 책임진다.
  },
);

// 페이로드가 없는 이펙트일 경우 제네릭을 생략하고, 콜백 첫번째 인자를 _ (언더바)로 바꿔준다.
const effTestListLoad = createEffect(
  async (_, dispatch) => {
    dispatch(actTestSave());

    testApiService
      .saveSomething()
      .then(res => dispatch(actTestSaveSucc(res)))
      .catch(error => dispatch(actTestSaveFail(error)));
  },
);

// 이펙트 사용
const TestContainer: FC = () => {
  const dispatch = useDispatch();
  const items = useSelector(selTestItems);

  useEffect(() => dispatch(effTestListLoad('theson')));

  return (
    <TestListComponent items={items} />
  );
};
```

## Reducer
리듀서는 typesafe-actions 의 createReducer 를 이용하여 작성하며 코드 파일 내 다음과 같은 구성 요소를 가진다.

* State Model (Interface)
* 상태 초기화 함수
* 리듀서 함수

각각을 구현하면 다음 예제와 같은 구조가 된다.

```ts
// State Model
export interface TestState {
  items: SampleItemModel[];
  totalCount: number;
  loading: boolean;
}

// State Initializer
function getInitState(): TestState {
  return {
    list: [],
    totalCount: 0,
    loading: false,
  };
}

// Reducer Function
export const testReducer = createReducer<
  TestState,
  TestActions,
>(getInitState())
  .handleAction(actTestListLoad, state => ({
    ...state,
    loading: true,
  }))
  .handleAction(actTestListSucc, (state, action) => ({
    ...state,
    items: action.payload.items,
    loading: false,
  }))
  .handleAction(actTestListFail, state => ({
    ...state,
    loading: false,
  }));
```

### Selectors
필요에 따라 [reselector](https://github.com/reduxjs/reselect) 를 이용하여 성능 향상도 할 수 있다.

다만 여기서의 셀렉터는 일반적인 함수(pure function)로 이용하는 방법을 서술한다.

```tsx
// AppState 는 스토어 선언 시 만들어지는 전체 상태를 담은 객체이다.
import { AppState } from '../../../stores';

export const selTestItems = ({ test }: AppState): SampleItemModel[] => (
  test.items,
);

export const selTestTotalCount = ({ test }: AppState): number => (
  test.totalCount,
);

export const selTestLoading = ({ test }: AppState): boolean => (
  test.loading,
);

// 사용 예제
const TestContainer: FC = () => {
  const items = useSelector(selTestItems);
  const totalCount = useSelector(selTestTotalCount);

  return (
    <TestListComponent
      items={items}
      totalCount={totalCount}
    />
  );
};
```
