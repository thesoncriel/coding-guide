# 모델 작성

본 문서상에서의 모델(Model)이란 DTO(Data Transfer Object) 혹은 VO(Value Object) 를 의미한다.

모델은 특별한 사유가 없는한, 반드시 `interface` 로만 구성한다.

예외적으로 `Function Type`은 다음과 같이 `type` 을 허용한다.

```ts
export type SimpleClickEventHandler = () => void;
```

추가로 이들에게서 쓰이는 별도 `enum` 선언도 아래와 같은 형식으로 포함될 수 있다.

```ts
/**
 * 열차 사용자 타입.
 */
export enum TrainUserType {
  ONE_TIME = 'ONE_TIME',
  MULTI_TIME = 'MULTI_TIME',
  OTHERS = '',
}
```

이러한 모델들은 다음과 같이 크게 2가지로 구분 되어진다.

## Server Model

서버 모델 혹은 도메인 모델(Domain Model) 이라고도 하는 이 것은, 프론트엔드에서 외부에 호출(Request)하여 받아가는 데이터들을 총칭한다.

그 형태(Schema)는 제공되는 Postman 이나 별도 API 문서의 것과 동일한 형태를 가진다.

작성 위치는 다음과 같다.

- /src/modules/{feature}/models/{feature[subFeature]}-server.model.ts

만약 작성량이 많아져서 sub feature 별로 폴더 구분이 필요하다면 다음과 같이 위치할 수 있다.

- /src/modules/{feature}/models/{subFeature}/{otherSubFeature}-server.model.ts

### 용도별 접미어

서버 모델은 다른 모델과의 구분을 위하여 다음과 같은 접미어로 작성한다.

| suffix  | usage                                                                                                | examples                     |
| :------ | :--------------------------------------------------------------------------------------------------- | :--------------------------- |
| Res     | Response. 응답 데이터                                                                                | UserRes BoardRowRes          |
| ResItem | Response Item. 응답 데이터 형태가 Array 일 때 그 구성요소에 대한 정의.                               | StyleResItem CampaignResItem |
| Params  | Parameters. RESTful API 를 통한 자료 요청 시 전달 될 query parameter 나 request body 에 쓰이는 자료. | LiveTvParams ArticleParams   |

### 작성 예시

```ts
// blog-server.model.ts

/**
 * 서버에서 받아오는 블로그 상세 응답 자료.
 *
 * https://www.google.com -- API 문서 링크 (없으면 생략)
 */
export interface BlogDetailRes {
  /*
   * 블로그 ID
   */
  id: string;
  /*
   * 블로그 제목
   */
  title: string;
  /*
   * 블로그 작성자
   */
  author: string;
  /*
   * 작성된 블로그 글
   */
  contents: string;
  /*
   * 작성 일시
   */
  createdAt: string;
}

/*
 * 포스팅된 블로그 목록 자료. 서버에서 가져 온다.
 */
export interface BlogPostResItem {
  /*
   * 블로그 ID
   */
  id: string;
  /*
   * 블로그 제목
   */
  title: string;
  /*
   * 블로그 작성자
   */
  author: string;
  /**
   * 블로그 요약글
   */
  summary: string;
}

/*
 * 블로그 상세 요청 시 필요한 파라미터
 */
export interface BlogDetailParams {
  /*
   * 블로그 ID
   */
  id: string;
  /*
   * 블로그 작성자
   */
  author?: string;
}

/*
 * 블로그 목록 검색 시 사용되는 파라미터
 */
export interface BlogSearchParams {
  /*
   * 작성자
   */
  author?: string;
  /**
   * 블로그 제목
   */
  title?: string;
}
```

#### 중복 항목 정리 (선택사항)

위 예시처럼 모델에 중복되는 필드가 있을 경우 상위 interface 로 그 중복 요소를 묶어주면 좋다.

필드가 매우 많아서 파일이 길어질 경우 정리 차원에서 시도 해 볼 수 있다.

```ts
// blog-server.model.ts

/**
 * 블로그 모델의 공통 요소를 분리한 것
 */
interface BlogCommonRes {
  /*
   * 블로그 ID
   */
  id: string;
  /*
   * 블로그 제목
   */
  title: string;
  /*
   * 블로그 작성자
   */
  author: string;
}

/**
 * 서버에서 받아오는 블로그 상세 응답 자료.
 *
 * https://www.google.com -- API 문서 링크 (없으면 생략)
 */
export interface BlogDetailRes extends BlogCommonRes {
  /*
   * 작성된 블로그 글
   */
  contents: string;
  /*
   * 작성 일시
   */
  createdAt: string;
}

/*
 * 포스팅된 블로그 목록 자료. 서버에서 가져 온다.
 */
export interface BlogPostResItem extends BlogCommonRes {
  /**
   * 블로그 요약글
   */
  summary: string;
}

// 이하 동일
```

## UI Model

뷰 모델(View Model), 혹은 프레젠테이션 모델(Presentation Model) 로도 불리우는 UI 모델은 컴포넌트에서 화면 출력 시 사용되는 자료이다.

(참고로 여기서의 뷰 모델은 MVVM 패턴의 ViewModel 을 의미하지 않는다)

기존 서버 모델 대비, UI 모델을 따로 두는 이유는 `Presentation Model Pattern` 으로 내부 UI 로직을 외부 도메인 업무로부터 보호하기 위함이다.

그 형태는 출처인 서버 모델이 UI 친화적이라면 동일하거나 유사할 수 있다.

반면, 다음과 같은 경우엔 부득이하게 서버 모델과 불일치 될 수 있다.

- 서버 모델에 불필요한 필드가 많음
- 서버 모델의 Object Depth 가 깊어서 접근하기 어렵고 예외 사항이 자주 발생됨
- 서버 모델에서 사용되는 특정 항목의 정보가 빈약하여 추가적인 조치(추출, 병합)가 이뤄졌을 때
- 프론트와 백엔드 업무가 parallel 하게 진행되어 프론트측이 Mock-up 작업이 먼저 이뤄졌을 때

작성 위치는 다음과 같다.

- /src/modules/{feature}/models/{feature[subFeature]}-ui.model.ts

만약 코드가 길어져 하위 기능별로 따로 폴더 구분이 필요하다면 아래와 같이 나눈다.

- /src/modules/{feature}/models/{subFeature}/{otherSubFeature}-ui.model.ts

### 용도별 접미어

UI 모델 역시 다른 모델과의 사용처 구분을 위하여 다음과 같은 접미어로 작성한다.

| suffix      | usage                                                                                                    | examples            |
| :---------- | :------------------------------------------------------------------------------------------------------- | :------------------ |
| UiModel     | 일반적인 UI 모델.                                                                                        | PostDetailUiModel   |
| UiModelItem | List 형태의 UI 에 쓰이는 모델.                                                                           | BoardRowUiModelItem |
| Props       | Properties. React Component 에서 프로퍼티 정의에 쓰이는 모델.                                            | LoginPanelProps     |
| Model       | 화면내에서 직접적으로 쓰이는 일이 없는 로직용 자료에 대한 모델.                                          | ImageProcessModel   |
| State       | component 의 내부 상태, 혹은 Context/Redux 의 상태 자료에 대한 정의.                                     | BookStoreState      |
| Args        | Event Arguments. 이벤트 발생 시 전달될 자료.                                                             | ClickEventArgs      |
| Payload     | Flux Architecture 에 따른 액션 페이로드 자료. 혹은 본 `아키텍처` 내 interactor 의 action method 전달 값. | NotificationPayload |
| QueryModel  | Page Component 를 통해서 전달되는 쿼리 파라미터.                                                         | BoardPageQueryModel |

### 작성 요령

상호 협의하거나 기존 내용을 그대로 가져다 쓰면 되는 서버 모델과 비교하여 UI 모델은 기획과 디자인 내용에 기반을 둔다.

때문에 구성된 기획 및 디자인 시안을 기반으로 모델링을 먼저 구성하고 작성하는 것을 추천 한다.

1. 기획/시안을 바탕으로 필요한 항목(field)를 추출한다.
2. 추출된 필드를 CRUD 기반의 화면 형태(View Type)에 따라 모델을 구분 짓는다.
   - Create/Update
     - 상세(Detail)모델을 먼저 구성한다.
   - Read
     - 아이템(Item) 모델을 구성한다.
3. 보여지는 화면내 발생되는 액션을 수집하여 이벤트 모델을 구성한다.

작성 시 가장 신경써야 할 부분은 `이 모델을 컴포넌트가 아무런 변형 없이 가져다 쓸 수 있는가` 의 여부이다.

컴포넌트는 `제어되는 바보같은 컴포넌트(Controlled Dumb Component)` 로 가능한 한 매우 단순하게 만들 것이기 때문이다.

즉, **각 컴포넌트가 필요로 하는 항목은 애초에 미리 다 만든다** 여기고 모델을 설계 해야한다.

### 작성 예시

```ts
// blog-ui.model.ts

/**
 * 블로그 상세에서 쓰일 표현 모델
 */
export interface BlogDetailViewUiModel {
  /**
   * 블로그 ID
   */
  id: string;
  /**
   * 작성자
   */
  author: string;
  /**
   * 블로그 주소
   */
  blogHref: string;
}

/**
 * 블로그 목록에서 쓰일 표현 모델
 */
export interface BlogListViewUiModelItem {
  /**
   * 블로그 ID
   */
  id: string;
  /**
   * 작성자
   */
  author: string;
}

/**
 * 블로그 목록에서 블로그를 선택 했을 때 전달될 이벤트 객체
 */
export interface BlogSelectArgs {
  /**
   * 선택된 블로그 자료
   */
  item: BlogListViewUiModelItem;
}

/**
 * 블로그 업데이트 시 필요한 페이로드
 */
export interface BlogUpdatePayload extends BlogDetailViewUiModel {
  /**
   * 추가된 태그 목록
   */
  addedTags: string[];
}
```

Ui 모델 역시 중복되는 항목은 묶어서 별도 관리 할 수 있다. (예시 생략)
