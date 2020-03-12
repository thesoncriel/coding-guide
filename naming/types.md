# Classes

클래스는 객체지향 프로그래밍 (Object Oriented Programming - 이하 OOP) 에서 가장 주력으로 쓰이는 대상이다.

클래스는 반드시 [Pascal Case](https://zetawiki.com/wiki/%EC%B9%B4%EB%A9%9C%ED%91%9C%EA%B8%B0%EB%B2%95_camelCase,_%ED%8C%8C%EC%8A%A4%EC%B9%BC%ED%91%9C%EA%B8%B0%EB%B2%95_PascalCase) 로 작성 되어야 한다.

한편, React 내 Component 역시 클래스 범주에 포함되기에 Class Component, Function Component 구별 없이 모두 **Pascal Case** 로 작성한다.

# Interfaces

인터페이스는 OOP 에서 SW의 유연성을 책임지는 것 중 하나이다.

- 참고 : [OOP 추상화 = 모델링](https://expert0226.tistory.com/212)

인터페이스는 반드시 [Pascal Case](https://zetawiki.com/wiki/%EC%B9%B4%EB%A9%9C%ED%91%9C%EA%B8%B0%EB%B2%95_camelCase,_%ED%8C%8C%EC%8A%A4%EC%B9%BC%ED%91%9C%EA%B8%B0%EB%B2%95_PascalCase) 로 작성 되어야 한다.

인터페이스를 이용하는 case 는 다음과 같다.

- DTO (Data Transfer Object) 혹은 VO (Value Object) 의 Data Type 으로 사용
- 다중 상속 (Multiple Inheritance)
  - 인터페이스를 통한 추상화 (Abstraction)

아래는 언급된 case 별 사용 방법을 정리 해 놓은 것이다.

## DTO

DTO 라 불리우는 자료 전달 객체는 크게 2가지로 구분된다.

- UI 로직/표현용 (Presentation Model)
  - Frontend 프로젝트내 구성된 클래스, 컴포넌트 및 함수 끼리만 주고 받는다.
  - Backend API 와는 직접적으로 연관성이 없다. (업무 특성상 유사할 수는 있으나 직접적으로 쓰는 일은 없다.)
- 외부 통신용 (Domain Model)
  - 프로젝트 외부용 데이터들.
  - Backend API 와 연관되며 이를 직/간접적으로 활용하기 위한 수단이 된다.

도메인 모델이 UI 모델에 친화적이면 곧바로 프론트엔드측에서 사용할 수 있다.

### 프론트엔드 UI 로직용

| usage                          | keyword | rule   | desc.                                                          | examples                            |
| :----------------------------- | :------ | :----- | :------------------------------------------------------------- | :---------------------------------- |
| DTO<br/>VO                     | Model   | suffix | 내부 클래스/함수 끼리 주고받는 데이터 모델.                    | UserModel<br/>StudentGroupModel     |
| React Props                    | Props   | suffix | 리액트 컴포넌트의 프로퍼티 타입 정의.                          | RadioButtonProps<br/>PaymentProps   |
| Event Arguments                | Args    | suffix | 이벤트 전달용 객체.                                            | InputChangeArgs<br/>UploadStateArgs |
| Flux Action Payload            | Payload | suffix | Action 수행 시 전달되는 페이로드 데이터.<br/>Flux 에서 쓰인다. | SigninAction<br/>DataLoadedAction   |
| Flux State<br/>Component State | State   | suffix | Store, 혹은 Component 상태를 보관하는 객체.                    | UserInfoState<br/>PaymentState      |

### 프론트엔드 UI 표현용 (Presentation Model)

Backend API 에서 제공되는 자료가 UI 친화적이지 못하여 업무 복잡도가 높아질 때 [Presentation Model Pattern](https://medium.com/@sandofsky/the-presentation-model-6aeaaab607a0) 이나 [Adapter Pattern](https://en.wikipedia.org/wiki/Adapter_pattern) 을 사용하게 된다.

이 때 View 나 Store 단에 전달되어 실제 프론트엔드에서 사용되는 모델들을 별도로 정의하여 사용하게 되는데 그 내용은 다음과 같다.

| usage                        | keyword     | rule   | desc.                                          | examples           |
| :--------------------------- | :---------- | :----- | :--------------------------------------------- | :----------------- |
| UI Presentation Model        | UiModel     | suffix | UI 화면에 출력될 때 쓰이는 전용 모델.          | SigninUiModel      |
| UI Presentation Model (Item) | UiItemModel | suffix | UI 화면에 리스트 형태로 출력될 때 쓰이는 모델. | SettingUiItemModel |

### 외부 전달용 (Domain Model)

| usage                                    | keyword | rule   | desc.                                                                                                                                                                                                    | examples                            |
| :--------------------------------------- | :------ | :----- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :---------------------------------- |
| API Response                             | Res     | suffix | 백엔드 API 결과물.<br/>내부적으로 parsing 하기 전 Raw Data 이다.                                                                                                                                         | SigninRes<br/>BoardListRes          |
| API Response (Item)                      | ResItem | suffix | 백엔드 API 결과물이 Array 로 구성되어 있을 때 각각의 Item Data 를 정의.                                                                                                                                  | BoardResItem<br/>PolicyResItem      |
| API Parameters                           | Params  | suffix | 백엔드 API 호출 시 사용되는 파라미터.<br/>그 용도는 쿼리 파라미터 일 수도,<br/>post 및 put 메서드일 때는 body 파라미터일 수도 있다.<br/>즉 API 사용측에선 굳이 body 파라미터 변환 여부를 알 필요가 없다. | ListLoadParams<br/>UserUpdateParams |
| URL Query Parameters<br/>Path Parameters | Query   | suffix | 외부에서 프론트엔드 영역 수행을 위한 쿼리 파라미터 전달 시 그 데이터를 모델화 시킨 자료이다.                                                                                                             | MainPageQuery                       |

## Multiple Inheritance

일반적으로 하나의 클래스를 작성할 때 필요한 모든 기능을 다 넣기 마련이나, 그 것을 실제 사용하는 외부에선 필요한 부분만 보도록 충분히 추상화 할 필요가 있다.

그래서 OOP 5대 원칙 중 하나인 Interface Segregation Principle - 인터페이스 분리 원칙에 의해 상호 필요한 부분들만 쪼개어 선언한 뒤 사용하게 된다.

이러한 용도의 인터페이스 중 메서드가 있는 클래스를 대상으로 **다중 상속** 을 목적으로 만들어지는 인터페이스는 그 명칭 앞에 대문자로 '**I**' 를 붙인다.

```ts
// class 혹은 object 구현체를 위한 다중상속 목적의 인터페이스 작성 예제
export interface ICherryPicker {
  runPicking: () => Promise<boolean>;
  callOuterProcess: () => void;
  accessData: () => Promise<boolean>;
}
```

아래는 Class 로 구현 했을 때의 예제이다.

```ts
// class 로 구현
export class CherryPicker implements ICherryPicker {
  runPicking(): Promise<boolean> {
    // code...
  }
  callOuterProcess() {
    // code...
  }
  accessData(): Promise<boolean> {
    // code...
  }
}

// class 를 인스턴스화 하여 수행
const picker = new CherryPicker();

picker.runPicking().then(() => {
  /* code... */
});
```

아래는 Factory 로 구현 했을 때의 예제이다.

```ts
// factory 를 이용하여 구현
function createPicker(): ICherryPicker {
  return {
    runPicking(): Promise<boolean> {
      // code...
    },
    callOuterProcess() {
      // code...
    },
    accessData(): Promise<boolean> {
      // code...
    }
  };
}

// factory 로 수행
const picker = createPicker();

picker.runPicking().then(() => {
  /* code... */
});
```

다중 상속을 고려한 인터페이스는 최소 1개 이상의 public method가 포함 되어야 한다.

```ts
// 예시
interface ILoginManager {
  login(): Promise<string>;
  logout(): Promise<void>;
}

interface IUserInfoManager {
  changeInfo(): Promise<void>;
  toUserInfo(): UserInfoModel;
}

// 다중 상속 클래스 예제
class UserInfoComponent implements ILoginManager, IUserInfoManager {
  login(): Promise<string> {
    // code..
  }
  logout(): Promise<void> {
    // code..
  }
  changeInfo(): Promise<void> {
    // code..
  }
  toUserInfo() {
    // code..
  }
}
```

다만, DTO 용 interface 는 'I' 를 붙이지 않으며 인터페이스 간에 다중 상속을 하여도 그 인터페이스의 목적에 따라 명칭을 붙여주면 된다.

```ts
// 내부 용도인 저자 정보
interface AuthorUserModel {
  name: string;
  id: string;
  age: number;
}

// 외부 API로 받아들인 책 정보
interface BookResItem {
  bookId: string;
  title: string;
  summary: string;
}

// BookItem 컴포넌트에서 쓰이는 프로퍼티
interface BookItemProps extends AuthorUserModel, BookResItem {
  imageSrc: string;
}

// BookItem 컴포넌트 내용이 변경될 때 전달되는 이벤트 인자
interface BookItemChangeArgs extends InputChangeArgs, BookResItem {
  index: number;
}
```

다만 위 예제 처럼 여러 인터페이스를 붙였을 때 각 프로퍼티가 중복되지 않도록 주의 하며, 중복되더라도 같은 내용을 의미 하도록 설계를 잘 이루어야 할 것이다.

아래는 잘못 작성된 예시이다.

```ts
// 내부 용도인 저자 정보
interface AuthorUserModel {
  name: string;
  id: string;
  age: number;
}

// 외부 API로 받아들인 책 정보
interface BookResItem {
  bookId: string;
  name: string;
  summary: string;
}

interface AuthorInfoModel extends AuthorUserModel, BookResItem {}

const author: AuthorInfoMode = { ... }l;

author.name; // 저자 정보의 이름인가? 책 이름인가?
```

이렇게 작성하지 않도록 주의 한다 :)
