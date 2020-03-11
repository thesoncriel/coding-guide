# Constants

웹프론트엔드 개발 시 자주 쓰이는 상수 명칭을 정리 하였다.

상수는 기본적으로 [UPPER CASE](https://chaseonsoftware.com/most-common-programming-case-types/) 로 작성한다.

## 접두어 계열

| prefix | type   | desc.                                                                                                                  | examples                             | note |
| :----- | :----- | :--------------------------------------------------------------------------------------------------------------------- | :----------------------------------- | :--- |
| MSG    | string | 사용자에게 전달될 메시지                                                                                               | MSG_INVALID_INPUT                    |      |
| COLOR  | string | UI Style 에 사용되는 RGB 값.<br/>기본적으로 Hexa 값이며, alpha channel 이 필요하다면 rgba(r,g,b,a) 형식으로 정의 한다. | COLOR_POINT_YELLOW<br/>COLOR_BG_GRAY |      |

## 접미어 계열

대체로 컬렉션 계열이 필요할 때 쓴다.

내용이 너무 많아지면 json 으로 별도 구성하여 ajax 수행을 권장한다.

| suffix        | type                           | desc.                                                      | examples                        | note |
| :------------ | :----------------------------- | :--------------------------------------------------------- | :------------------------------ | :--- |
| MAP<br/>TABLE | KV Object<br/>HashMap&lt;T&gt; | key 를 주고 value 를 받을 간이 스토리지 및 비교대조표 구성 | RESULT_NAME_MAP<br/>BOOKS_TABLE |      |
| LIST          | Array&lt;T&gt;<br/>T[]         | index 를 주고 value 를 받을 간이 목록 및 배열              | USER_NAMES_LIST<br/>COUPON_LIST |      |

## 열거형 (enum)

타입 명칭은 접미어로 **Type** 을 넣는다.

그리고 열거형의 각 요소들은 Upper Case 로 작성한다.

```ts
// 단순한 형태의 열거형
enum SampleType {
  NONE,
  FIRST,
  SECOND,
  ETC
}
```

```ts
// 특정 모델값을 대체 하는 열거형
const json = `[
  {
    name: "neptune",
    type: "GAS_PLANET"
  },
  {
    name: "venus",
    type: "ROCK_PLANET"
  }
]`;

enum PlanetType {
  UNKNOWN = '',
  GAS_PLANET = 'GAS_PLANET',
  ROCK_PLANET = 'ROCK_PLANET'
}

interface PlanetModel {
  name: string;
  type: PlanetType;
}

// 사용 예제
const arr: PlanetModel[] = JSON.parse(json);
```
