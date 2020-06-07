# 서비스 작성

서비스는 다음과 같은 역할을 맡는다.

- 자료 제공 (Data Service)
- 계산 및 처리 (Calcuation & Processing)
- 자료 가공 (Data Manipulation)
- 자료 분석 (Data Parsing)
- 유틸리티 (Utillity)

...좀 장황하게 써 놨지만 저러한 유형이 모두 작성되는 경우는 매우 드물다!

이 곳에서는 빈번하게 쓰이는 자료 제공/가공, 유틸리티에 작성에 대한 내용이다.

## 1. Data Service

데이터 서비스는 아키텍처 기준, interactor 나 effect 에서 쓰이는 각종 자료를 제공하는 역할을 맡는다.

제공되는 자료는 다음과 같은 유형이 있다.

- 사내 백엔드 API 를 통해 전달됨
- 사내 CDN 의 정적 자료 (예: JSON, XML, PlainText, 정적 이미지 등)
- 외부 Open APi 를 통해 전달됨 (예: 네이버 or 다음 주소 검색 API 등)
- 내부 캐시를 통해 전달됨 (예: local/sessionStorage, memory cache, cookie 등)

### Base API

HTTP 비동기 통신을 이용한 자료 제공은 반드시 아래 위치한 파일에 base api 를 먼저 작성한다.

- /src/modules/common/services/baseApi.ts

작성 시 필요한 준비물은 다음과 같다.

- Base Url: API 를 호출할 도메인 주소 및 기본 경로. 환경변수 appConfig 를 통해서 가져온다.
- Header Factory: 각 API 마다 다르게 요구되는 HTTP 헤더를 정의.

대부분 기본 프로젝트 세팅에 포함되어 있으므로 아래 내용을 참고만 해도 좋다.

```ts
// common/services/baseApi.ts

/**
 * 기본적인 요청 헤더를 만든다.
 */
function createBaseHeader() {
  return httpHeaderProviderFactory()(pipeAcceptContentType);
}

/**
 * 로그인 토큰이 쓰이는 헤더를 만든다.
 */
function createAuthHeader() {
  return httpHeaderProviderFactory(barereTokenProvider)(pipeAuthHeaders);
}

/**
 * 기본 API 호출 서비스.
 */
export const baseApi = apiFactory(appConfig.apiBaseUrl, createBaseHeader, true);

/**
 * 로그인 토큰이 필요한 API 서비스.
 */
export const authApi = apiFactory(appConfig.apiBaseUrl, createAuthHeader, true);

/**
 * CDN 을 통해 가져오는 서비스.
 */
export const cdnApi = apiFactory(appConfig.cdnBaseUrl);

/**
 * 공개된 API 를 통해 가져오는 서비스.
 */
export const publicApi = apiFactory('');
```

### 서비스 작성 예시

파일위치는 다음과 같다.

- /src/modules/{feature}/services/{feature}-data.service.ts

데이터 서비스 작성은 다음과 같이 객체 리터럴을 외부에 넘겨주는 형식으로 단순하게 작성한다.

```ts
export const blogDataService = {
  loadBlogList(params: BlogSearchParams) {
    return baseApi.get<ItemsRes<BlogResItem>>('/api/blog', params);
  },
};
```

root object 에 data 나 items 가 있을 경우 아래와 같이 즉석 타입을 넣지 않고 미리 정의된 `DataRes` 나 `ItemsRes` 를 대신 이용한다.

```ts
// bad
export const blogDataService = {
  loadBlogList(params: BlogSearchParams) {
    return baseApi.get<{ items: BlogResItem[] }>('/api/blog', params);
  },
};
```

한편, 데이터 출처가 `localStorage` 라면 아래와 같이 작성된다.

```ts
// object, array 타입 지원을 하는 스토리지를 만든다.
const sto = storageFactory<CartModel>('local', 'cartList');

export const cartDataService = {
  loadCartList() {
    return sto.get();
  },
  saveCartList(data: CartModel) {
    sto.set(data);
  },
  removeCartList() {
    sto.remove();
  },
};
```

네이버나 다음 같은 Open API 일 경우 예시

```ts
export const addressDataService = {
  async findAddressByCoods(coods: Coordinate) {
    const naverAddress = await loadThirdPartyLib(
      `https://api.naver.com/address.js?key=${appConfig.naverKey}`,
    );

    return naverAddress.getAddressByCoods(coods);
  },
};
```

## 2. Data Manipulation

데이터 조작기는 크게 3가지 유형으로 분류된다.

- create : 자료를 생성한다. 테스트용 mock data 나 복잡한 detail model 이나 item model 등을 만들어준다.
- convert : 자료를 변환한다. `Presentation Model Pattern`에 따라 주로 서버 모델 -> Ui 모델로의 변환이 이뤄진다. 반대의 경우도 발생된다. (예: put, post 처리를 위한 request body 자료 변환용)
- extract : 자료에서 특정 자료를 추출한다. 대체로 convert 의 하위 작업으로 발생된다. 여기서 추출이라 함은 depth 가 깊은 특정 객체의 필요한 부분만 가져오거나, 길고 긴 자료의 필요한 부분만 추출하는 등의 행위를 의미 한다. 조건별 필터링 등도 해당된다.

### 작성 요령

이미 만들어진 서버 및 UI 모델의 상호 변환에 중점을 두면 된다.

즉 데이터 조작은 엄연히 `UI 모델과 서버 모델(파라미터)의 상호 변환 과정이 전부`라 보면 된다.

그래서 모델을 먼저 잘 작성하는 것이 중요하며, 설사 그 과정이 부족했다 할지라도 이 후 모델 추가 작성/변경 후 다시 이 부분을 만져주면 된다.

여기서 주의점은 이 자료 변환기는 **절대 Data Service, Component 에서 쓰이면 안된다**는 것이다.

(단, Mockup 자료를 위한 테스트 용도로써 잠시 Data Service 에서의 활용은 가능하다.)

### 작성 예시

```ts
// blog.create.ts

export function createBlogDetailUiModel(): BlogDetailUiModel {
  return {
    id: 0,
    title: '',
    author: '',
    contents: '',
  };
}

// 목업용 임시 자료 생성
export function createBlogListUiModelItem_mock(): BlogListUiModelItem {
  return {
    id: 0,
    title: '포메는 사랑입니다!',
    tagList: [],
    imageUrl: 'https://www.pompom.com/image/12356',
  };
}
```

```ts
// fashion.extract.ts

// 패션 정보에서 컬렉션을 추출한다.
export function extractCollectionFromFashion(
  item: FashionResItem,
): FashionCollectionModel {
  return {
    id: item.collection.id,
    name: item.collection.name,
    expiredDate: item.expired
      ? item.collection.lastDate
      : dateFormat('yyyy-MM-dd', addDate(new Date(), 90)),
    images: item.collection.pictures,
  };
}
```

```ts
// store.convert.ts

// 구형 카트 정보를 신형 카트 정보로 변환한다.
export function toNewCartUiModelItem(res: CartResItem): NewCartUiModelItem {
  return {
    goodsId: res.id,
    goodsName: res.name,
    subGoods: res.subList.map(toSubGoods),
    addedDate: res.date || new Date(),
  };
}
```

## 3. Utillity

유틸리티는 상기 언급된 자료 변환함수는 물론 별도 언급되는 `Interactor`에서 쓰이는 유틸리티성 함수들의 모음이다.

유틸리티가 자료 변환기와 차별되는 점은, 유틸리티는 `컴포넌트에서도 사용할 수 있음` 이다.

가령 10000 이라는 자료를 컴포넌트측에서 서식 지정으로 10,000 으로 표현 가능한데, 이정도는 컴포넌트에서 맡을 수 있고, 이런 함수는 컴포넌트가 아니라 여기, util 폴더 내에 위치 해야 한다.

만약 업무상 유틸리티 함수가 필요 없다면 굳이 작성하지 않아도 된다.

유틸리티는 대체로 데이터 로직의 한 축을 담당하기 때문에 테스트가 필요하 경우가 많을 것이다.

그러므로 매우 단순한 것이 아닌 한, 가능한 한 테스트 코드를 작성하길 권장한다.

유틸리티는 `/util` 내에 포함되며 예외적으로 아래와 같은 패턴을 따른다.

```
util.{type}.ts
```

type 은 다음 표를 참고 한다.

| type       | desc                                       | filename examples  |
| :--------- | :----------------------------------------- | :----------------- |
| array      | 배열 관련                                  | util.array.ts      |
| component  | 컴포넌트 관련                              | util.component.ts  |
| validate   | 유효성 검증 관련                           | util.validate.ts   |
| collection | 배열 외 자료구조 관련                      | util.collection.ts |
| string     | 문자열 관련                                | util.string.ts     |
| format     | 서식(포맷) 관련                            | util.format.ts     |
| etc        | 그 외 어느 카테고리에도 포함되지 않는 것들 | util.etc.ts        |

기 작성된 유틸리티가 다른 곳에서도 쓰인다면, common module 로 옮겨서 공용화 할 수 있다.
