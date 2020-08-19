# 모델 작성 가이드 - part 2

아키텍처를 프로젝트에 적용하여 운용하다 보면 공통 데이터 모델이 겹치고 쌓이게 된다.

이는 다수의 인원이 개발 프로젝트에 투입되면 자연스러운 현상이며, 겹치거나 유사한 클래스 및 모델은 하나의 인터페이스로 묶어서 관리하여 Bottom-Top 방식으로 리팩토링이 권장된다.

이에 인터페이스 분리 원칙 (Interface Segregation Principle - ISP) 에 따라 공용 인터페이스를 하나로 묶고, 추가되는 필드는 확장하여 사용 하게 된다.

이 문서에서는 이러한 아키텍처 내 약속되어 사용되는 여러 형태의 모델들과 그에 따른 운용 방법을 설명한다.

## 서비스 응답 모델 작성

서비스 작성 가이드에도 언급이 되어 있으나 이 곳에는 좀 더 관련 내용을 상세히 다룬다.

사내 백엔드 응답 자료중 그 형태가 `List` 형태를 취할 때는 기본적으로 아래와 같은 형식을 띈다.

```json
{
  "data": [
    {
      "id": 1,
      "name": "고양이"
    },
    {
      "id": 2,
      "name": "강아지"
    }
  ]
}
```

```json
{
  "items": [
    {
      "id": 1,
      "name": "고양이"
    },
    {
      "id": 2,
      "name": "강아지"
    }
  ]
}
```

윗쪽 `data` 필드로 받는 것은 `DataRes` 로, 아래쪽 `items` 필드로 받는 것은 `ItemsRes` 로 공용 interface 로 정의 되어 있다.

이들은 다음과 같이 사용 한다.

아래는 타입만을 선언했을 때의 예시 이다.

```ts
// data 필드 이용 시
type PetRes = DataRes<PetResItem[]>; // data 필드가 단일 object 일 수도 있기 때문에 array 형식일 경우엔 bracket 을 적어 준다.

// items 필드 이용 시
type PetRes = ItemsRes<PetResItem>; // items 필드는 array 임이 명확하기에 별도 bracket 을 적지 아니한다.
```

아래는 실 사용 예시이다.

```ts
export const dataService = {
  // DataRes 사용 예시
  loadPetList() {
    return baseApi.get<DataRes<PetResItem[]>>('/api/demoPets');
  },
  // ItemsRes 사용 예시
  loadPets() {
    return baseApi.get<ItemsRes<PetResItem>>('/api/demoPets');
  },
};
```

### DataRes, ItemsRes 기반으로 추가적인 필드가 필요 할 때

아래와 같이 응답받는 경우이다.

```json
{
  "items": [
    {
      "id": 1,
      "name": "고양이"
    },
    {
      "id": 2,
      "name": "강아지"
    }
  ],
  "totalCount": 2,
  "paging": {
    "next": 45,
    "prev": 24
  }
}
```

이 때는 다음과 같이 DataRes, 혹은 ItemsRes 를 extends 하여 사용한다.

```ts
// 선언

export interface PagingRes {
  next: number;
  prev: number;
}

export interface PetItemsRes extends ItemsRes<PetResItem> {
  totalCount: number;
  paging: PagingRes;
}
```

```ts
// 사용

export const dataService = {
  // ItemsRes 확장 사용 예시
  loadPets() {
    return baseApi.get<PetItemsRes>('/api/demoPets');
  },
};
```

### DataRes, ItemsRes 를 generic 선언된 타입 사용

간혹 아래와 같이 이름이 길어져서 DataRes, ItemsRes 사용이 곤란할 때가 있다.

```ts
export const dataService = {
  // ItemsRes 확장 사용 예시
  loadPets() {
    return baseApi.get<ItemsRes<SmallSizedMyPetsForBreedingInMyHouseResItem>>(
      '/api/demoPets',
    );
  },
};
```

좀 길어도 가급적 위의 예시처럼 쓰길 권장한다.

#### Type Alias (Deprecated)

> 아래 내용은 권장하지 않습니다.

그러나 generic 을 합쳐 type alias 하여 사용하고자 한다면 아래와 같은 규칙을 따른다.

1. DataRes, 혹은 ItemsRes 기반으로 type generic 된 것만을 허용한다.
2. 이들 모델을 기반으로 extends 되었다면 그걸 다시 type alias 하지 말고 선언된 interface 를 직접 사용한다.

```ts
// 선언. 특별한 사유가 없다면 feature.server.ts 모델 파일에 선언한다.
export type MyPetsRes = ItemsRes<SmallSizedMyPetsForBreedingInMyHouseResItem>;

// 사용
export const dataService = {
  // 허용조건 1.
  loadPets() {
    return baseApi.get<MyPetsRes>('/api/demoPets');
  },
};
```

```ts
// 공용 인터페이스를 extends 하여 선언 한다.
// 이 역시 server model 파일 내에 위치 해야 한다.
export interface MyPetsRowRes
  extends ItemsRes<SmallSizedMyPetsForBreedingInMyHouseResItem> {
  totalCount: number;
  rows: number;
  keywords: string[];
}

// do not !! - 이미 선언된 인터페이스를 또 다시 type 으로 alias 하여 사용하지 않는다.
export type MyPetsRowResData = MyPetsRowRes; // ???

// 사용
export const dataService = {
  // 허용조건 2.
  loadPets() {
    return baseApi.get<MyPetsRowRes>('/api/demoPets');
  },
};
```
