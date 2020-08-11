# Web Frontend Code Convention

좀 더 명확하고 의미 있는 코드를 작성하기 위해 저희 프론트엔드셀 멤버분들 끼리 정해진 코딩 규칙을 이 곳에 적습니다.

이 외 기본적인 규칙은 아키텍처 문서를 참고 해 주세요!

이 후 규칙이 정해지면 계속 이 곳에 남깁니다.

## 복수형 프로퍼티

컴포넌트 내 Props 나 컨텍스트의 State 에서 자료가 복수형일 때는 아래와 같이 `items` 혹은 `items 접미어` 를 사용 합니다.

그리고 해당 items 를 이용하여 순회(looping) 할 경우엔 `item` 을 씁니다.

만약 그 것이 컴포넌트라면 items 의 순회 결과물을 받는 ItemComponent 는 `item` 이라는 props 를 두거나

혹은, 그 item 의 기반 model 을 props 로 삼을 수 있습니다.

### 컴포넌트 예시

```tsx
// 컴포넌트 예시 1

interface Props {
  item: StyledUserUiModel;
}

export const StyledUserItem: FC<Props> = ({ item }) => {
  return (
    <ListItem>
      이름: {item.name}
      <br />
      나이: {item.age}
    </ListItem>
  );
};

interface Props {
  items: StyledUserUiModel[];
}

export const StyledUserList: FC<Props> = ({ items }) => {
  return (
    <>
      {items.map((item, idx) => (
        <StyledUserItem key={idx} item={item} />
      ))}
    </>
  );
};
```

한편, 아래는 위 예시에서 `하위 Item 컴포넌트의 Props 가 Item Model 의 내용을 그대로 extends 했을 때` 의 예시 입니다.

```tsx
// 컴포넌트 예시 2

// 모델을 곧바로 사용하므로 따로 Props 를 선언하지 않습니다.

export const StyledUserItem: FC<StyledUserUiModel> = ({ name, age }) => {
  return (
    <ListItem>
      이름: {name}
      <br />
      나이: {age}
    </ListItem>
  );
};

interface Props {
  items: StyledUserUiModel[];
}

export const StyledUserList: FC<Props> = ({ items }) => {
  return (
    <>
      {items.map((item, idx) => (
        <StyledUserItem key={idx} {...item} />
      ))}
    </>
  );
};
```

### State 및 Model 예시

아래는 단순 UI 모델을 이용하여 컨텍스트 상태 모델을 정의 할 때 ..입니다.

```ts
export interface UserStyleUiModel {
  userId: string;
  userName: string;
  styleType: UserStyleType; // enum 입니다.
}

export interface UserStyleState {
  items: UserStyleUiModel[];
  loading: boolean;
}
```

컬렉션 순회 할 때의 예시 입니다.

```ts
const items: UserResItem[] = getUserItems();

// 슬프게도(?) theson 이 아닌 멤버만 고릅니다..
const filteredItems = items.filter((item) => item.name !== 'theson');
```

하나의 상태에 여러 복수형 필드가 존재하면 아래와 같이 접미어로 `Items` 를 붙여 줍니다.

```ts
// 서버에서 제공받은 내용

interface CafeInfoResItem {
  name: string;
  style: string;
  visitCount: number;
  tasty: CafeTastyType; // enum 입니다.
}

interface OfficeInfoResItem {
  optical: string;
  lightColor: string;
  bookScale: number;
}

// 목표가 될 상태 모델. Ui 가 필요로하는 데이터로 충만합니다!
export interface OfficeInfoState {
  cafeInfoItems: CafeInfoResItem[];
  officeInfoItems: OfficeInfoResItem[];
  loading: boolean;
}

// ------------------------------------------ //

// 서버에서는 카페와 오피스 정보를 뭉탱이로 줍니다..
interface ServerRes {
  cafeInfoData: CafeInfoResItem[];
  officeInfo: OfficeInfoResItem[];
}

// 인터렉터에서 받아서 처리할 때의 예시
const res: DataRes<ServerRes[]> = await baseApi.loadOfficeInfo();
const items: ServerRes[] = res.items;

// 컴포넌트 설계를 해 보니 각 하위 모델들을 기준으로 하는 items 를 각각 만들어 적용시킬 필요가 있었다..
// 그리고 서버 모델이 Ui 친화적이라 convert 는 필요 없었다..
// 라고 가정 합니다.

const cafeInfoItems = extractCafeInfoItems(items);
const officeInfoItems = extractOfficeInfoItems(items);

// 추출된 각 items 을 적용 합니다.
dispatch({
  cafeInfoItems,
  officeInfoItems,
  loading: false,
});
```

## 객체 필드명에 data 를 남발하지 마세요

data 라는 이름의 필드명은 굉장히 자주 쓰입니다.

서버에서 전달되는 응답 자료에는 어쩔 수 없지만, 저희는 필요에 따라 data 를 지양하기로 하였습니다.

### data.data.data ...

객체의 필드를 연쇄적으로 접근하다 보면 `data.data.data ...` 와 같은 형태를 목격하게 됩니다!

레거시 코드가 아닌 한, 새로 작성하게 되는 코드는 `data` 라는 단일 필드명은 짓지 않습니다.

부득이하게 data 로 사용해야만 한다면, 대신 아래와 같이 그 data 의 `interface model 명칭` 을 이용하여 사용 합니다.

참고로 아래는 좀 과장된 경우 입니다 :)

```ts
// 아래와 같은 모델이 있다 가정

interface PetShopInfoModel {
  name: string;
  location: string;
  specific: string;
  desc: string;
  likeCounts: number;
}
```

```ts
// bad

interface DataState {
  data: PetShopInfoModel;
}

interface ResultState {
  data: DataState;
}

const data: ResultState = {
  data: {
    data: {
      name: '포메 사랑',
      location: '대한민국 포메시',
      specific: '포메 많음',
      desc: '포메포메포메...',
      likeCounts: 10000000,
    },
  },
};

// WTF ?!
const petShop = data.data.data;
```

```ts
// good

interface DataState {
  petShopInfo: PetShopInfoModel;
}

interface ResultState {
  state: DataState;
}

const result: ResultState = {
  state: {
    petShopInfo: {
      name: '포메 사랑',
      location: '대한민국 포메시',
      specific: '포메 많음',
      desc: '포메포메포메...',
      likeCounts: 10000000,
    },
  },
};

// -ㅂ-)b
const petShop = result.state.petShopInfo;
```
