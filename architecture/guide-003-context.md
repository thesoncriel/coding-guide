# 상태 관리 작성

상태 관리는 프론트엔드 기능의 핵심이다.

사실상 이곳으로 화면에 맞는 자료를 주기 위해 이전의 모든 과정이 있다 봐도 과언이 아니다.

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

따라서 별도 언급된 `각종 서비스의 주요 사용처`다.

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
  getState: () => SampleState,
  dispatch: Dispatch<Partial<SampleState>>,
) => ({
  // 액션 메서드 내 dispatch 수행 시 변화가 필요한 필드만 명시하여 반영한다.
  // 나머지 object spread 는 라이브러리에서 처리 해 준다.
  async loadList() {
    dispatch({ loading: true });

    try {
      const res = await baseApi.loadSampleList();
      const items = toSampleUiModelItems(res.items);

      dispatch({
        successCount: getState().successCount + 1,
        loading: false,
        items,
      });
    } catch (e) {
      didspatch({ loading: false });
    }
  },
  async updateItem(payload: SampleUpdatePayload) {
    dispatch({ loading: true });

    try {
      const params = toSampleUpdateParams(payload.item);
      const { name } = await baseApi.updateItem(params);

      const items = [...getState().items];

      // lazy patch 를 하고 있다.
      items[payload.index] = {
        ...items[payload.index],
        name,
      };

      dispatch({ loading: false, items });
    } catch (e) {
      dispatch({ loading: false });
    }
  },
});
```

2020년 6월 24일 현재 기준, dispatch 수행 시 spread syntax 는 더이상 필요치 않다. (수행해도 상관은 없음)

만약 `getState` 를 쓸 일이 없다면 아래와 같이 함수명을 underbar(\_) 로 바꿔준다.

```ts
export const sampleInteractor = (
  // state 를 쓸 일이 없다면 요렇게 바꿔줘욥!!
  _: () => SampleState,
  dispatch: Dispatch<Partial<SampleState>>,
) => ({
  async loadList() {
    dispatch({ loading: true });

    try {
      const res = await baseApi.loadSampleList();
      const items = toSampleUiModelItems(res.items);

      dispatch({
        loading: false,
        items,
      });
    } catch (e) {
      didspatch({ loading: false });
    }
  },
});
```

## 컨텍스트 패키지 사용 예시

패키지 내엔 다음과 같은 항목이 포함되어 있다.

- withCtx - 컨텍스트를 해당 컴포넌트에 제공 해 준다.
- CtxProvider - 컨텍스트 제공자. 컴포넌트 코드에 직접 삽입해서 사용한다. 필요에 의해 제공되지만 일반적인 상황에선 이것 보단 `withCtx` 사용을 권장한다.
- useCtxDispatch - 컨텍스트 상태값을 수정하는 함수를 가져온다. 가급적 직접 수행은 피하고 `interactor`를 통해서만 상태 변경을 하도록 권장한다.
- useCtxSelector - 컨텍스트 상태값을 가져온다. 내부에 selector 함수가 필요하다.
- useCtxSelectorAll - 컨텍스트의 모든 상태값을 가져온다.
- useInteractor - 인터렉터를 사용한다.

아래는 실 사용 예시이다.

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

### 주의사항

그럴리 없겠지만, `contextInjector` 를 통한 컨텍스트 패키지 이용시엔 절대 **같은 컨텍스트를 중복 사용해서는 안된다.**

예시를 들면 아래와 같다.

```ts
// duplicate.context.ts
export const duplicateContext = contextInjector(getInitState(), interactor);
```

```tsx
// OneContainer.tsx
const OneComponent = () => {
  const { items } = duplicateContext.useSelectorAll();

  return (
    <SectionLayer>
      <SampleList items={items} />
    </SectionLayer>
  );
};

export const OneContainer = duplicateContext.withCtx(OneComponent);
```

```tsx
// TwoContainer.tsx
const TwoComponent = () => {
  const { items } = duplicateContext.useSelectorAll();

  return (
    <SectionLayer>
      <SampleList items={items} />
    </SectionLayer>
  );
};

export const TwoContainer = duplicateContext.withCtx(TwoComponent);
```

이렇게 사용하면, 가장 최근에 적용한 컨테이너에만 의도대로 동작되는 생각치도 못한 오류가 발생된다.

만약 이렇게 쓸 경우엔 아래와 같이 상위 컨테이너를 만들고 여기에 `withCtx`를 사용한다.

그리고 이 상위 컨테이너에서 하위 컨테이너에게 내려주는 형식으로 변경하자.

```tsx
// ParentContainer.tsx
const ParentComponent = () => {
  return (
    <>
      <OneContainer />
      <TwoContainer />
    </>
  );
};

export const ParentContainer = duplicateContext.withCtx(ParentComponent);
```

```tsx
// OneContainer.tsx
export const OneContainer = () => {
  const { items } = duplicateContext.useSelectorAll();

  return (
    <SectionLayer>
      <SampleList items={items} />
    </SectionLayer>
  );
};
```

```tsx
// TwoContainer.tsx
export const TwoContainer = () => {
  const { items } = duplicateContext.useSelectorAll();

  return (
    <SectionLayer>
      <SampleList items={items} />
    </SectionLayer>
  );
};
```

위와 비슷한 내용인데, SelectBox 나 Autocomplete 같은 하위 Container 에서 패키지 사용시엔 상위 컨테이너에서 Provide 를 해 주고 있다 가정하므로 **withCtx 사용은 생략** 해도 된다.

## 사용 범위

일반적으로 컨텍스트는 해당 업무영역(Container Section)에 한정되기 마련이다.

다만, 그 내용이 페이지 단위로 이뤄지는 경우엔 페이지 컴포넌트에 직접 `withCtx` 로 Decorate 해서 써야 한다.

그래야 `useSelector` 혹은 `useSelectorAll` 사용 시 그 페이지 내 어느 컨테이너에서도 그 컨텍스트 내용에 access 할 수 있기 때문이다.

```tsx
const NowUseComponent = () => {
  return (
    <PageContainer title="스쉐 만쉐이!">
      <SubContainer />
    </PageContainer>
  );
};

export const NowUsePage = subContext.withCtx(NowUseComponent);
```

어쩌다 보면 다수의 컨테이너가 각자의 업무 범위를 가지는데 비해, 다른 컨테이너에게도 공유 시켜야 할 경우가 있다.

말인 즉슨, 여러개의 컨텍스트가 여러 컨테이너에 공유되는 상황이다.

이럴 때는 아래와 같이 각 컨텍스트의 `withCtx`를 최상위 페이지 컴포넌트를 대상으로 중첩하여 사용한다.

```tsx
const NowUseComponent = () => {
  return (
    <PageContainer title="스쉐 만쉐이!">
      <SubContainer />
      <ChildContainer />
      <YourContainer />
      <OurContainer />
    </PageContainer>
  );
};

export const NowUsePage = subContext.withCtx(
  childContext.withCtx(
    yourContext.withCtx(ourContext.withCtx(NowUseComponent)),
  ),
);
```

다소 극단적인 예시이지만, 업무를 구성하다 보면 이런 일도 있지 않을까 한다.

다만, 이런 경우가 발생 되었다면 이 후 저 컨텍스트는 하나로 묶어서 쓰도록 리팩토링이 필요한 시점일 수도 있다.

(아닐 수도 있고...)
