# State Management

상태 관리는 프론트엔드 기능의 핵심이다.

사실상 이곳으로 화면에 맞는 자료를 주기위해 이전의 모든 과정이 있다 봐도 과언이 아니다.

그 와 함께 비동기 자료와 이벤트 처리를 위한 Effect 및 Interactor 도 함께 언급할 것이다.

여기서는 아키텍처에서 주력으로 사용되는 `Context` 와 `Interacotr` 에 대해서 다룬다.

## Context

React 의 기본 기능 중 하나인 Context API 는 하위 컴포넌트에 상태를 내려주기 위한 역할을 한다.

다만, 무턱대고 자료를 내려주는 것은 `단일 책임 원칙`과 `관심사 분리`를 무시하게 되므로 Context 를 이용하는 것은 어디까지나 **Container Component** 에게만 그 권한을 부여한다.

### 컨텍스트 생성

컨텍스트 생성은 `contextInjector` 를 통해 이뤄진다.

이 함수는 컨텍스트의 Provider, useContext, State 및 이 들을 다룰 수 있는 interactor 를 엮어서 하나의 패키지로 이용할 수 있게 해 준다.

아래는 작성 예제이다.

```ts
// sample.context.ts

// 컨텍스트에서 쓰일 상태 타입 정의
export interface SampleState {
  items: SampleUiModelItem[];
  loading: boolean;
}

// 초기 상대를 만들어주는 생성기
export function getInitSampleState(): SampleState {
  return {
    items: [],
    loading: true,
  };
}

// 만들어진 컨텍스트 패키지
export const sampleContext = contextInjector(
  getInitSampleState(),
  sampleInteractor,
);
```

## Interactor

인터렉터는 서비스를 통해 데이터를 받아들이고 컨텍스트의 상태값을 변경하는 역할을 맡는다.

따라서 별도 언급된 `Data Service` 의 주요 사용처다.

아래는 작성 예시이다.

```ts
// sample-server.model.ts

// 아래에 쓰이는 페이로드 타입에 대한 정의 (예시)
export interface SampleUpdatePayload {
  item: SampleUiModelItem;
  index: number;
}

// sample.interactor.ts

export const sampleInteractor = (
  state: SampleState,
  dispatch: Dispatch<SampleState>,
) => ({
  async loadList() {
    dispatch({ ...state, loading: true });

    try {
      const res = await baseApi.loadSampleList();
      const items = toSampleUiModelItems(res.items);

      dispatch({ ...state, loading: false, items });
    } catch (e) {
      didspatch({ ...state, loading: false });
    }
  },
  async updateItem(payload: SampleUpdatePayload) {
    dispatch({ ...state, loading: true });

    try {
      const params = toSampleUpdateParams(payload.item);
      const { name } = await baseApi.updateItem(params);

      const items = [...state.items];

      // lazy patch 를 하고 있다.
      items[payload.index] = {
        ...items[payload.index],
        name,
      };

      dispatch({ ...state, loading: false, items });
    } catch (e) {
      didspatch({ ...state, loading: false });
    }
  },
});
```

## 컨텍스트 패키지 사용 예시

```tsx
// SampleContainer.tsx

const SampleComponent = () => {
  const { items } = sampleContext.useSelectorAll();
  const inter = sampleContext.useInteractor();

  const handleChange = (item: SampleUiModelItem, index: number) => {
    inter.updateItem({ item, index });
  };

  useEffect(() => {
    inter.loadList();
  }, []);

  return (
    <SectionLayer>
      <SampleList items={items} onChange={handleChange} />
    </SectionLayer>
  );
};

export const SampleContainer = sampleContext.withCtx(SampleComponent);
```

## 사용 범위

일반적으로 컨텍스트는 해당 업무영역(Container Section)에 한정되기 마련이다.

다만, 그 내용이 페이지 단위로 이뤄지는 경우엔 페이지 컴포넌트에 직접 `withCtx` 로 Decorate 해서 써야 한다.

그래야 `useSelector` 혹은 `useSelectorAll` 사용 시 그 페이지 내 어느 컨테이너에서도 그 컨텍스트 내용에 access 할 수 있기 때문이다.
