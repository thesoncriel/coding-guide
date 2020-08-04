# UI Component Design - 9. 페이지 작성

앞서 사용처(Client)에 대해 언급 한 적이 있습니다.

아토믹 디자인까지 적용된 전체 컴포넌트를 실제 이용하는

즉, 가장 최상위의 컴포넌트를 적용 해 볼건데

이러한 최상위 사용처를 `페이지 컴포넌트(Page Component)` 라 부릅니다.

## 페이지 컴포넌트의 역할

주요 역할은 아래와 같습니다.

- 라우팅 시스템이 접근할 수 있는 유일한 컴포넌트
  - react-router 를 쓸 경우 `<Route>` 에서만 사용 가능,
  - next.js 를 쓸 경우 folder 를 이용한 route system 에서만 사용 가능
- 컨테이너 컴포넌트(Container Component)를 사용하여 업무 구성
- Path Parameter, Query Parameter 를 받아서 하위 컨테이너에 내려주는 역할
- `페이지 템플릿 컴포넌트(Page Template Component)`를 사용할 수 있는 유일한 컴포넌트

반면, 하면 안되는 역할을 다음과 같습니다.

- CSS 같은 스타일링 직접 삽입
- props 나 파라미터를 받아 직접 렌더링
- title 이나 open-graph 같은 meta data 이용
- 이 외 상기 역할에 반하는 모든 행위들

이 중 라우팅 시스템 사용에 대해선 생략 합니다.

이 점 양해 바랍니다. 🙏

## 파라미터 처리

페이지 컴포넌트의 가장 중요한 기능이 바로 파라미터 처리 입니다.

대체로 파라미터라는 자료는 키(key)와 값(value)의 쌍(pair)으로 이뤄진 맵(Map)의 형태를 이루고 있습니다.

하지만 파라미터는 그 사용처에서 원하는 키의 형태가 동일하므로 전에도 그랬던 것 처럼, 인터페이스로 선언하여 이용 할 것입니다.

먼저 어떤 파라미터가 들어올지 고민을 해 봅시다.🤔

### 파라미터 추출

간만에 최초 시안을 다시 소환 하겠습니다!

![](https://raw.githubusercontent.com/thesoncriel/coding-guide/master/sw-design/images/ui-design-001/ui-design-001_001.png)

위 시안에서 파라미터가 될 만한걸 찾으셨나요?

일반적으로 목록형 화면(List View)은 목록 개수(limit)와 페이징(paging) 기능이 포함 됩니다.

그리고 이들 자료를 원하는대로 볼 수 있게 해 주는 키워드나 정렬 등의 검색 옵션이 들어갑니다.

그런데 현재 시안에서는 `정렬` 밖에 보이지 않습니다.

물론 페이징 대신 무한 스크롤을 통한 자료 불러오기도 가능합니다만 여기선 언급하진 않겠습니다.

그럼 남은건?

네! `정렬(Sort)` 하나 밖에 없군요.

그럼 정렬 필드엔 어떤 값이 들어가야 할까요?

시안에서 눈에 띄는건 `상품` 과 `랭킹` 2가지 입니다.

즉 나머지 선택권은 없다는 얘기가 되겠습니다.

만약 파라미터에 아무 값도 안들어간다면 어떻게 해야 할까요?

음... 🤔

시안은 `랭킹`을 가리키고 있으니 랭킹을 기본 값으로 가지도록 하겠습니다.

정리하면 다음과 같습니다.

- 사용 파라미터: 정렬(sort)
- 가능한 값: 상품, 랭킹
- 기본값: 랭킹

위 처럼 항상 파라미터값은 `기본값`을 염두에 두시기 바랍니다!

만약 기본값을 주지 않을 경우, 그 것을 모두 UI 컴포넌트에서 예외처리 해 주어야 하기 때문입니다.

따라서 특정 파라미터의 객체화(deserialization - 역직렬화) 시도는 별도 변환기(converter)가 필요하며 일반적으로 선택자(selector - 셀렉터)가 맡게 됩니다.

이는 이 후 다시 언급 하겠습니다. 🙂

### 타입 선언

정렬은 오직 2가지 상태만 가집니다.

바로 `상품`과 `랭킹` 입니다.

이들을 코드로 녹이기 위해 영문자료 풀면 `goods`와 `ranking` 이 되겠습니다.

웹프론트엔드 기준, 주력 언어는 TypeScript 입니다.

여기서 여러가지 선택지 중 한가지를 고르게 하는 타입이 있습니다.

바로 `열거형(enum)` 입니다.

열거형은 다음과 같이 기술할 수 있습니다.

```ts
enum PlanetType {
  Earth,
  Mars,
  Venus,
  Jupiter,
}
```

근데 그냥 이렇게 쓸 경우 자체적으로 정수(integer)를 지정하게 되므로 우리가 바라는 문자열로 바로 가져다 쓸 수 없습니다.

이 때는 아래와 같이 별도 값을 지정하면 됩니다.

```ts
enum PlanetType {
  Earth = 'earth',
  Mars = 'mars',
  Venus = 'venus',
  Jupiter = 'jupiter',
}
```

같은 방법으로 정렬도 enum 으로 타입 정의를 하면 다음과 같습니다.

```ts
enum ShopListSortType {
  Goods = 'goods',
  Ranking = 'ranking',
}
```

위 열거형을 다이어그램으로 표현하면 다음과 같습니다.

![](https://raw.githubusercontent.com/thesoncriel/coding-guide/master/sw-design/images/ui-design-009/ui-design-009-enum001.png)

좌측이 사용값 까지 모두 표현한 상태, 우측이 간략화 한 것입니다.

참고로 이러한 클래스 다이어그램에서 상단의 `<<blahblah>>` 처럼 표현한 것을 두고 **스테레오 타입(stereo type)** 이라 부릅니다.

여기서 말하는 스테레오란 `통상적으로 자주 일컫는 것`들을 뜻하며 대표적으로 인터페이스가 저렇게 스테레오 타입 선언되어 있었습니다.

열거형에 대한 스테레오 타입은 enum 이 아니라 `enumeration` 으로 적는 것이 관례이나, 글자가 좀 길어서 짧게 `enum` 으로 사용하니 참고 바랍니다.

선언된 열거형을 바탕으로 쿼리 모델(Query Model)을 작성하면 다음과 같습니다.

![](https://raw.githubusercontent.com/thesoncriel/coding-guide/master/sw-design/images/ui-design-009/ui-design-009-model001.png)

> **참고하세요!**
>
> 쿼리 모델은 UI 모델의 한 종류로써, 화면 출력에 기여하지 않고 오로지 페이지를 통한 패스 파라미터나 쿼리 파라미터의 값만을 보유하는 DTO, VO를 의미 합니다. 하나의 객체이기 때문에 원시형(Primitive Type)으로만 존재하면 안됩니다.

## 페이지 컴포넌트 작성

전에 작성했던 컨테이너인 **HeaderContainer** 와 **ShopListContainer** 를 이용하는 페이지 컴포넌트를 작성 해 보겠습니다.

먼저, 페이지 컴포넌트 이름은 뭘로 할까요?

흐음.. 🤔

간단히 **ShopListPage** 라 명명 하겠습니다.

안에 들어갈 속성은 뭐가 있을까요?

음..

파라미터 처리를 하니 **query** 라는 이름의 멤버가 있으면 좋을거 같습니다!

이것과 더불어 컨테이너 2개를 다이어그램으로 함께 표현 하겠습니다.

혹시 더 있다고 판단되셔도 이 후 내용에서 함께 보시면 좋을거 같아요!

![](https://raw.githubusercontent.com/thesoncriel/coding-guide/master/sw-design/images/ui-design-009/ui-design-009-page001.png)

**ShopListPage** 컴포넌트에서 컨테이너를 여럿 가지기 위해 containers 멤버를 가지며 이에 대한 타입은 `ContainerList` 입니다.

> **참고**
>
> ContainerList 는 편의상 만든 임의의 타입이며, 이게 직관적이지 않다고 느끼신다면 아래와 같이 하셔도 무방합니다!
>
> ![](https://raw.githubusercontent.com/thesoncriel/coding-guide/master/sw-design/images/ui-design-009/ui-design-009-page002.png)
>
> 이 외 여러가지 variation 이 있겠지만 여러분의 상상에 맡기며 생략 하겠습니다. 🙂

## 누구에게 줄까?

파라미터를 페이지에 연관시키긴 했는데, 이걸 하위 컨테이너에게 주어야 합니다.

현재 기준으로 아래와 같은 선택지가 있습니다.

1. HeaderContainer 에게 준다.
2. ShopListContainer 에게 준다.
3. 그냥 아무도 주지 말고 ShopListPage 가 해결한다.

이 선택지에선 어느정도 호불호가 있을거 같습니다!

하지만 저희는 한가지를 선택해야 합니다.

그러기 위해선 나름 합당한(?) 이유가 있어야 할 것입니다. 😅

저는 그 이유를 `누가 가장 관심있어 할까?` 로 정했습니다!

### 이거 가질 사람 손?!

현재 쿼리 모델엔 `sort` 필드 하나밖에 없습니다.

현재 설계 내용을 기준으로 보자면, sort 필드에 가장 관심을 많이 가질 부분은 어딜까요?

HeaderContainer 는 음...

매우 많은 관심을 가지고 있죠?

ShopListContainer 는.. 음..

알아서 뭐하나요 ㅎㅎ

다만 걸리는건, 얘가 스스로 did-mount 할 때 자료 호출을 한다는 것 밖엔 없습니다.

마지막으로 ShopListPage 는 어떤가요?

관심 가져야 할까요?

얘는 이걸 가지고 하는게 아무것도 없는데 말입니다.

결정 났나요?

네! HeaderContainer 가 가지겠습니다!

### 자동 호출 권한은 누구에게?

sort 파라미터는 HeaderContainer 의 몫이 되었습니다.

그럼 현재 did-mount 시 호출되는 자료 요청은 어떻게 되나요?

여전히 ShopListContainer 가 가지고 있습니다.

근데 이것보단 앞으로 sort 파라미터를 가지게 될 HeaderContainer 가 요청하게 하는게 낫지 않을까요?

어떻하죠?

음..

어쩔 수 없군요!

불가피하지만 설계 수정을 하도록 하겠습니다! 😱

~~(안돼)~~

## 설계 수정하기

코드를 고치긴 어렵지만 설계를 고치는건 쉬운 일입니다!

더욱이 이렇게 각자 업무의 시행착오를 줄이기위한 목적으로 종이에 연필로 끄적인 것이라면 말이죠!

그럼 고쳐야 할 두 컨테이너를 다시 보시죠.

![](https://raw.githubusercontent.com/thesoncriel/coding-guide/master/sw-design/images/ui-design-009/ui-design-009-container001.png)

어딜 고쳐야 할까요?

가장 첫번째로, HeaderContainer 에게 sort 값을 받아 오도록 해야 합니다.

여기서 또한 선택지가 있습니다.

객체를 줄까요, 아님 단일 값을 줄까요?

흐음.. 🤔

비록 하나 밖에 없지만, 이후 수정을 최소화 하기 위해 저는 그냥 객체 하나를 다 주도록 하겠습니다. 🙂

![](https://raw.githubusercontent.com/thesoncriel/coding-guide/master/sw-design/images/ui-design-009/ui-design-009-container002.png)

보시다시피 HeaderContainer 에 `query` 멤버를 추가 하였습니다.

이는 실제 아래와 같이 사용될 것입니다.

```tsx
const Page: FC = () => {
  const query = useQueryParams();

  return (
    <>
      <HeaderContainer query={query} />
      <ShopListContainer />
    </>
  );
};
```

다음은 did-mount 기능을 ShopListContainer 에서 뺏어올 차례 입니다.

![](https://raw.githubusercontent.com/thesoncriel/coding-guide/master/sw-design/images/ui-design-009/ui-design-009-container003.png)

어떤가요?

말 그대로 뺏어오기만 했습니다! 🙂

이젠 ShopListContainer 는 정말 스스로 할 수 있는게 거의 없어졌네요! 🤣

자, 그럼 한가지만 더 고쳐 보겠습니다.

사실 이 정도면 되지만 인터렉터에 유사한 기능이 분리되어 있거든요.

### 분리된 메서드 합치기

전에 작성했던 인터렉터를 살펴 보겠습니다.

![](https://raw.githubusercontent.com/thesoncriel/coding-guide/master/sw-design/images/ui-design-006/ui-design-006-Interactor001.png)

현재 기준으로 이게 어떤 것이 문제라 보시나요?

잘 모르시겠다구요? 😅

그럼 간단히 목록으로 표현 해 보겠습니다.

- 기존엔 ShopListContainer 가 did-mount 로 자료를 호출 하였다.
- 현재 sort 파라미터를 받아 처리할 곳이 HeaderContainer 로 지정 되었다.
- ShopListContainer 는 여전히 did-mount 로 자료를 호출 한다.
- 그러나 ShopListContainer 는 sort 를 받아들이지 않는다.
- 그래서 HeaderContainer 로 did-mount 이벤트를 이전 하였다.

여기까지가 함께 수행한 내용입니다.

다음은 변경이 필요한 부분입니다.

- 기존 did-mount 에서 사용하던 것은 단순 자료 호출(loadItems) 이었다.
- 반면, HeaderContainer 는 외부에서 준 sort 를 받아들일 수 있다.
- sort는 기본값(랭킹)이 정해져 있으나 페이지 최초 호출 시 sort 파라미터를 이용하면 `랭킹`이 아닐 수 있다.
- 그래서 2번 호출 하지 않으려면 did-mount 를 통한 최초 호출 시 전달 받은 sort 를 **직접** 이용 해야 한다.

어떤걸 바꿔야 할지 감이 잡히시나요?

또 한가지 있습니다!

- 기존 sort change 이벤트 발생 시 수행된 `changeSort` 는 변경된 sort 값을 Store 에 전달할 뿐만 아니라 스스로가 검색하는 기능도 가지고 있다.
- 기존엔 이벤트 처리부와 자료 호출부 2개로 나뉘었다.
- 반면, 현재는 둘 다 sort 값을 필요로 한다.
- 굳이 2개로 나눠야 하나?

..해서 아래와 같이 `search` 메서드 하나로 통합 하였습니다.

![](https://raw.githubusercontent.com/thesoncriel/coding-guide/master/sw-design/images/ui-design-009/ui-design-009-inter002.png)

이제 이거 하나로 `onSortChange` 이벤트와 `did-mount` 이벤트 때 함께 사용 할 수 있습니다!

![](https://raw.githubusercontent.com/thesoncriel/coding-guide/master/sw-design/images/ui-design-009/ui-design-009-page003.png)

## Query Parameter Selector - QPS

파라미터는 정말 다양한 방법과 값으로 호출됩니다.

이 때 유효한 값인지 여부를 체크하고 올바른 방법으로만 걸러내어 내부에서는 걱정 없이 쓸 수 있도록 하는 데이터 로직이 필요합니다.

또 한 이러한 파라미터들에 대하여 객체화(deserialization - 역직렬화) 시도도 함께 맡게 됩니다.

이러한 것을 수행하는 것을 통틀어 `쿼리 파라미터 선택자(Query Parameter Selector - 이하 QPS)`라 부릅니다.

QPS 는 익히 아시는 선택자 패턴을 이용 합니다.

이것을 페이지에 의존성으로 추가 할 것입니다.

> **참고하세요!**
>
> 선택자 패턴(Selectors Pattern)을 따르는 것이며 대표적인 사용 방법으로 Redux 의 `useSelector` 나 reselect, 혹은 ngRx(Angluar) 의 `selector` 는 물론, vuex 의 `getter` 에서도 똑같은 패턴으로 사용될 만큼 함수형 프로그래밍(Functional Programming)에서는 매우 대중적인 자료 변환 패턴 입니다.

이하 QPS를 현 챕터에 한하여 `선택자`라 부르겠습니다. 🙂

### 선택자 추가

설계 상에서 선택자를 어떻게 추가해야 할까요?

먼저 데이터 흐름을 살펴 보겠습니다.

1. 파라미터 제공자(useQueryParams)를 통해 자료를 받는다.
2. 선택자를 이용하여 필요한 자료로 가공한다.
3. 페이지는 이 자료를 받아서 이용한다.

간단하군요!

기존 설계는 제공자(Provider)가 없었으므로 이를 추가하면 됩니다.

제공자 이름은 무엇으로 하면 될까요?

음.. 🤔

실제론 `useQueryParams` 를 사용하므로 이에 대해서 언급이 있으면 좋을거 같다는 생각이 듭니다.

그럼 `클래스명은 Pascal Case 로 둔다`는 관례를 따라 **UseQueryParams** 로 두겠습니다!

그리고 UseQueryParams 에 selector 를 연결 시키면 되겠죠!

이 때 페이지는 selector 를 가져와 UseQueryParams 과 함께 호출 할 것입니다.

그럼 아래와 같은 모양새가 나옵니다.

![](https://raw.githubusercontent.com/thesoncriel/coding-guide/master/sw-design/images/ui-design-009/ui-design-009-page004.png)

ShopListPage 는 **useQueryParams** 를 이용 할 것입니다.

이 때 useQueryParams 와 함께 사용될 선택자는 hooks 가 원하는 형태의 함수를 제공하므로 일종의 인터페이스에 해당합니다.

추가로 UseQueryParams 와 Selector 간의 관계가 이전에 못봤던 형태임을 확인 할 수 있습니다.

이건 Aggregation(집합) 이라 하며 이전의 Composition 관계와는 달리 속이 빈 하얀 마름모꼴을 하고 있습니다.

![](https://raw.githubusercontent.com/thesoncriel/coding-guide/master/sw-design/images/ui-design-009/ui-design-009-agg001.png)

통칭 `흰 다이아`라 부르며 이 것은 Composition 과는 반대로 A 가 B를 필요로 할 때 A가 죽더라도 B는 영향을 받지 않는 관계 입니다.

단, 이러한 관계가 대체로 모호한 면이 많아서 저는 주로 `멤버 함수 호출 시 특정한 기능을 가진 객체를 잠시만 필요할 때` 씁니다.

이 때 필요한 객체는 DTO, VO 같은 모델이나 엔티티가 아니어야 합니다.

돌아와서, 이러한 관계가 아무리 설계 단계지만 다소 복잡해 보이므로 저는 이번처럼 `useQueryParams`를 사용하는 경우에 한하여 아래와 같이 간소화 하겠습니다.

![](https://raw.githubusercontent.com/thesoncriel/coding-guide/master/sw-design/images/ui-design-009/ui-design-009-page005.png)

보시다사피 UseQueryParams 의 private member 로 **Selector** 가 있는 모습입니다.

그리고 아래는 이번장에서의 최종 페이지 컴포넌트 모습 입니다.

![](https://raw.githubusercontent.com/thesoncriel/coding-guide/master/sw-design/images/ui-design-009/ui-design-009-page006.png)

## 정리하며

여러분과 함께 설계 해 온 컴포넌트가 추가 요청 사항에 대해 변경되는 것을 보셨습니다.

`"아니, 첨부터 이걸 고려해서 하지 왜 이제와서..?"` 라는 반응이 있으실 거 같습니다.

음.. 그런데 말입니다.

![](https://mblogthumb-phinf.pstatic.net/MjAxODAxMTBfNzkg/MDAxNTE1NTc3NTUwNTEy.yaCHlLolwO851k5hsLQbXXz1Mfj9q6Un6iJqjtOIhp8g.tabALYYTOCc4VoE2TZA_NZMFbr0suXNsQT8HRAfICpQg.JPEG.pshlbs/99BE50335A25FBAF2265D7.jpg?type=w2)

원래 설계란 이런 것입니다. 😅

실제 코드에 옮기기 앞서 미리 시행착오를 겪어보고 어떻게 코딩 할지 정리해보는 단계인 것이죠.

이런 체험을 해보는게 초기에 중요하다 생각되어 처음부터 완벽하게 분석 해 나가지 않았던 것입니다.

설계중에 이뤄지는 시행착오는 그냥 종이에 쓱~~ 적어서 `어, 이게 되네? 오호~ 이거 이럼 되겠네?` ..하며 그 자리에서 바로 확인이 가능한 장점이 있습니다.

즉, 중요한 것은 굳이 코드상에 옮기지 않아도 그 결과를 예측할 수 있는 경험인 것입니다.

그 경험,

이 자릴 빌어 계속 알려 드리겠습니다! 😉

~~(미안하다 이거 보여주려고 어그로 끌었다)~~

그럼 다음으로 쓩~~!

## 참고 문헌

- [the selector pattern](https://forax.github.io/2014-01-27-8650156-the_selector_pattern.html)
- [using the selectors design pattern in node js](https://medium.com/swlh/using-the-selectors-design-pattern-in-node-js-356388329aa5)
