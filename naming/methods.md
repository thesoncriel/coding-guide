# Methods

메서드 및 함수 명칭에 자주 쓰이는 단어(Noun) 및 동사(Verb)를 정의해 놓았다.

기본적으론 Camel Case 로 작성된다.

## Prefix - Verb

함수 혹은 메서드는 어떠한 행위를 수행하기위한 것이다.

그래서 메서드명은 아래와 같이 항상 접두어로 동사(verb)를 이용한다.

한편 메서드를 호출하는 객체가 있고, 그 객체가 충분히 의미론적(Semantic)이라면 제시된 접두어 동사만 사용해도 무관하다.

- 예: database.connect()

| verb | desc.               | examples           | note |
| :--- | :------------------ | :----------------- | :--- |
| get  | 특정 값을 받아온다. | geName<br/>getAge  |      |
| set  | 특정 값을 설정한다. | setName<br/>setAge |      |

### Behavior

| verb                   | desc.                                                                                                                                                                                                                                          | examples                                 | note                                                                                                 |
| :--------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------- | :--------------------------------------------------------------------------------------------------- |
| init                   | 초기화 한다.                                                                                                                                                                                                                                   | init<br/>initStore                       |                                                                                                      |
| try                    | 특정 행위를 시도한다.<br/>내부적으로 try~catch 구문을 가지며 자체적으로 예외처리 하고, 문제 발생 시 기본값을 줄 수 있다.                                                                                                                       | tryParseInt<br/>tryOtherAction           |                                                                                                      |
| retry                  | 특정 행위를 재시도한다.                                                                                                                                                                                                                        | retryConnection                          |                                                                                                      |
| valid<br/>validate     | 어떤 값에 대한 유효성 검사를 진행한다.                                                                                                                                                                                                         | validEmail<br/>validEmpty                | 반환 타입은 boolean 이거나 외부에서 유효성 결과를 참조할 수 있는 별도의 객체를 반드시 내보내야 한다. |
| do                     | 어떠한 행위를 수행한다.<br/>전처리 (preprocess) 가 필요 없으며, 단독으로 즉시 수행하고 끝나는 기능이어야 한다.                                                                                                                                 | doCopy<br/>doMove                        |                                                                                                      |
| run                    | do와 유사하나 전처리 작업이 필요하다.<br/>앞서 여러가지의 전처리 작업들을 수행 한 뒤 마지막으로 그 것을 수행(run) 하는 의미로써 사용한다.<br/>예: 엑셀 내용을 json 으로 바꾸는 과정<br/>readExcelFile → loadRows → toArray → run (marshalling) |                                          |                                                                                                      |
| cancel                 | 행위를 취소한다.                                                                                                                                                                                                                               | cancelLoadData                           |                                                                                                      |
| open<br/>close         | UI를 열거나 닫는다.<br/>혹은 DB나 외부 통신과 연결하거나 닫는다.                                                                                                                                                                               | openModal<br/>dialog.open<br/>closeToast |                                                                                                      |
| connect<br/>disconnect | DB나 외부 통신과 연결하거나 닫는다.                                                                                                                                                                                                            | connectDB                                |                                                                                                      |
| apply<br/>disapply     | 특정 객체가 가진 데이터를 최종적으로 적용 시키거나 적용된 것을 취소 한다.                                                                                                                                                                      | apply<br/>applyUserInput                 |                                                                                                      |
| show<br/>hide          | UI를 화면상에 보여주거나 숨긴다.<br/>Modal, Dialog, 미리보기 등.                                                                                                                                                                               | showModal<br/>hideLabel                  |                                                                                                      |
| enable<br/>disable     | UI를 활성화/비활성화 시킨다.                                                                                                                                                                                                                   | enableInputName                          |                                                                                                      |

### Collection

Map, List, Array, Iterator 등 컬렉션 객체에서 사용되는 메서드 이다.

| verb         | desc.                                                                                             | examples                   | note |
| :----------- | :------------------------------------------------------------------------------------------------ | :------------------------- | :--- |
| add          | 요소를 추가한다.<br/>주로 선형(Linear) 자료구조에 쓰인다.                                         | addItem<br/>addUser        |      |
| remove       | 요소를 삭제한다.                                                                                  | removeItem<br/>removeValue |      |
| indexOf      | 요소의 인덱스 위치를 확인한다.                                                                    |                            |      |
| modify       | 요소를 변경한다.                                                                                  | modifyItem<br/>modifyValue |      |
| get<br/>set  | 요소를 가져오거나 설정한다.                                                                       | getItem<br/>setItem        |      |
| next<br/>has | 요소의 존재 여부를 확인한다.                                                                      |                            |      |
| clear        | 모든 요소를 제거한다.                                                                             |                            |      |
| find         | 특정 요소를 찾는다.                                                                               | findItem                   |      |
| filter       | 특정 요소만을 거른다.<br/>Array.prototype.filter 와 같은 곳에서 사용될 함수를 정의할 때도 쓰인다. |                            |

### Data Manipulation

데이터를 생성하거나 조작할 때 쓰이는 메서드이다.

| verb                                                           | desc.                                                                                                                                                            | examples                                                 | note                                      |
| :------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------- | :---------------------------------------- |
| create                                                         | 객체를 생성한다.<br/>만들어질 객체는 interface 및 class 기반이어야 하며, 대체로 Factory Pattern 으로써 쓰인다.<br/> 즉 생성된 객체는 반드시 불변(immutable)이다. | createUiModel                                            |                                           |
| make                                                           | 자료를 만든다.<br/>컬렉션에 기반한 List, Map 등이 만들어져야 한다.                                                                                               | makeMatrix<br/>makeCalendarData                          |                                           |
| fix<br/>correct<br/>refine                                     | 데이터가 잘못된 것을 확인하고 그 내용을 수정한다.<br/>effect 같은 곳에서 방어 코드용으로 쓰일 수 있다.                                                           | fixResData<br/>correctMyInfo<br/>refineDomainData        |                                           |
| parse                                                          | 데이터를 해석해서 알맞은 데이터로 바꾼다.<br/>텍스트나 엑셀등의 문서를 읽고 그 이진 내용을 객체화 할 때 사용할 수 있다.                                          | parseExcel<br/>parseXml                                  |                                           |
| convert<br/>convertToA<br/>convertAToB<br/>adapt<br/>adaptForA | 특정 객체의 형태를 다르게 바꾸고자 할 때 쓰인다.                                                                                                                 | convertMapToArray<br/>convertToDomain<br/>adaptForListUi | 1가지의 데이터만을 대상으로 한다.         |
| merge                                                          | 최소 두개 이상의 데이터를 결합 시킨다.<br/>결합된 데이터는 원본 모델이 단순히 합쳐지거나 파생된(derived), 혹은 확장된(extends) 형태여야 한다.                    | mergeUserInfo                                            | 최소 2가지 이상의 데이터를 대상으로 한다. |
| combine                                                        | 최소 두개 이상의 데이터를 결합 시킨다.<br/>결합된 데이터는 원본 모델과 전혀 상관 없어도 된다.<br/>즉 merge 와 convert가 동시에 이루어질 경우 combine을 사용한다. | combineToPayInfo                                         |                                           |
| compare                                                        | 최소 두개의 같은 타입의 데이터를 비교한다.<br/>그 결과는 같다면 0, 왼쪽이 크다면 -1, 오른쪽이 크다면 1 이다.<br/>또는 같다면 true, 아니면 false 이다.            | compareCopyItems                                         |                                           |

### Data Stream

Promise 의 연쇄적인 then 메서드 수행이나 pipeline pattern 혹은 rxjs Observable 의 pipe operator 등에 사용되는 별개의 함수 선언 시 아래 내용을 참고한다.

| verb | desc.                                                                           | examples                     | note |
| :--- | :------------------------------------------------------------------------------ | :--------------------------- | :--- |
| pipe | 동기적으로 스트림 데이터 변환 시 사용한다.<br/>단순한 pipeline 패턴에 사용된다. | pipeApplySample              |      |
| map  | Promise, Observable 을 통한 동기/비동기 데이터 변환/전달 시 사용한다.           | mapConvert<br/>mapDungToGold |      |
| oper | Observable 의 pipe 에서 독립적으로 수행 가능한 custom operator 에 쓰인다.       | operUpdateInfo               |      |

### Check State

특정 객체나 환경 상태, 혹은 가능여부에 대하여 **boolean** 형태로 알려줄 때 쓰인다.

| verb   | desc.                                                                                          | examples     | note |
| :----- | :--------------------------------------------------------------------------------------------- | :----------- | :--- |
| is     | 해당 객체, 혹은 제시된 객체의 특정 상태를 알아볼 때 쓰인다.<br/>조건은 필요 시 생략될 수 있다. | isYoung      |      |
| has    | 해당 객체, 혹은 제시된 객체가 특정한 것을 가졌는지 여부를 확인한다.                            | hasBooks     |      |
| exists | 해당 객체, 혹은 제시된 객체의 특정 값에 대한 존재 여부를 확인한다.                             | existsMetal  |      |
| should | 해당 객체, 혹은 제시된 객체가 특정한 행위를 해도 되는지의 여부를 판단한다.                     | shouldChange |      |

#### Past Verb

단일로 과거형 동사(past verb)가 쓰이는 것 역시 해당 객체의 상태를 간단히 확인하기 위해 쓰이며 그 결과 값은 항상 **boolean** 형으로 반환 해야 한다.

대체적으로 일반적인 동사들에서 과거형으로만 바꿔서 쓰면 된다.

아래는 몇가지 예시이다.

| verb                       | desc.                                                                                                   |
| :------------------------- | :------------------------------------------------------------------------------------------------------ |
| opened<br/>closed          | UI가 열려있는지 혹은 닫혀 있는지 여부를 확인한다.<br/>혹은 DB나 외부 통신이 연결되어있는지 여부를 확인. |
| connected<br/>disconnected | DB나 외부 통신이 연결되어있는지 여부를 확인.                                                            |
| enabled<br/>disabled       | UI 나 컴포넌트 상태가 활성화/비활성화 되었는지의 여부를 확인.                                           |

### Asynchronous

비동기적으로 수행될 때는 아래 내용을 참고한다.

리턴 타입은 Promise 이거나 rxjs 를 사용 시 Observable 로 한정한다.

| verb   | desc.                                                                                                                                                            | examples                      | note                                                                                      |
| :----- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------- | :---------------------------- | :---------------------------------------------------------------------------------------- |
| load   | 해당 객체에서 자료를 불러 온다.<br/>불러오는 자료 주체가 외부일 경우 쓰인다.<br/>예: API 호출                                                                    | loadUserInfo<br/>loadData     |                                                                                           |
| save   | 해당 객체에서 자료를 저장 한다.<br/>저장되는 주체가 외부일 경우 쓰인다.                                                                                          | saveUserInfo<br/>saveData     |                                                                                           |
| read   | 자료를 읽어 온다.<br/>자료 주체가 내부 파일이나 메모리 내 이진 데이터일 경우에 쓰인다.<br/>예: Memory Buffer 에 올려진 Blob 데이터 읽기<br/>예: Stream Data 읽기 | readFile<br/>readBinary       |                                                                                           |
| write  | 자료를 기록 한다.<br/>자료 주체가 내부일 경우 쓰인다.                                                                                                            | writeFile<br/>writeStream     |                                                                                           |
| send   | 자료를 보낸다.<br/>단순히 보내기만 하고 그 결과는 기대치 않는다.                                                                                                 | sendUserInfo<br/>sendValues   |                                                                                           |
| regist | 자료를 등록한다.                                                                                                                                                 | registEventData               |                                                                                           |
| upload | 자료를 업로드한다.<br/>파일 업로드 같은 행위에 쓰인다.                                                                                                           | uploadProfile                 |                                                                                           |
| update | 자료를 변경한다.                                                                                                                                                 | updateHospital                | 외부 자료 변경 시 사용.<br/>API 호출이나 네트워크 통신을 이용하는 것 등이 있다.           |
| modify | 자료를 변경한다.                                                                                                                                                 | modifySchedule                | 내부 자료 변경 시 사용.<br/>파일 변경, 사용자 입력으로 인한 앱내 자료 변경 등이 포함된다. |
| search | 검색한다. 검색 대상은 사용자가 찾으려는 데이터다.                                                                                                                | searchList<br/>searchBookData |                                                                                           |

### Event Handler

이벤트 처리 시 사용되는 핸들러는 아래와 같이 작성한다.

| verb   | desc.                                                                                                                       | examples                                | note |
| :----- | :-------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------- | :--- |
| handle | 이벤트를 처리하는 메서드.<br/>우측에 대응되는 이벤트명을 명시 한다.<br/>보통 이벤트명에서 on 을 빼고 handle 을 넣으면 된다. | handleValueChange<br/>handleButtonClick |      |

## Preposition

전치사 (preposition)은 어떠한 상태로 바꾸거나 조정, 혹은 행위를 함에 있어 특정 대상을 한정짓는 뉘앙스를 준다.

전치사에 대한 자세한 내용은 다음 페이지를 참고 한다. (참고로 이 블로그에 기재된 모든 내용을 사용하는 건 아니다)

- [전치사의 종류와 의미](https://m.blog.naver.com/PostView.nhn?blogId=dimechaser1&logNo=80170502421&proxyReferer=https%3A%2F%2Fwww.google.com%2F)

| preposition | desc.                                                                                                                          | usage                            | examples                                |
| :---------- | :----------------------------------------------------------------------------------------------------------------------------- | :------------------------------- | :-------------------------------------- |
| by          | 수행 시 우측 명사에 대응되는 것을 참조하여 수행한다.                                                                           | {verb}{noun-A}By{noun-B}         | getItemByGoal                           |
| for         | 수행 시 우측 명사에 대응되는 것을 위하여 수행 한다.                                                                            | {verb\|prep.}{noun-A}For{noun-B} | findInfoForUser<br/>toItemsForSending   |
| as          | 수행 시 우측 명사에 대응되는 것으로 변환한다.<br/>변돤되는 타입은 원본 타입에 대하여 extends 혹은 implements 된 것이어야 한다. | as{noun}                         | asPromise<br/>asObservable              |
| on          | 이벤트 메서드를 가르킨다.<br/>우측엔 발생되는 이벤트에 대해 명시 한다.                                                         | on{noun}{verb}                   | onValueChange<br/>onButtonClick         |
| to          | 특정 객체의 형태를 다르게 바꾸고자 할 때 쓰임.<br/>convert 를 단순히 표현하고자 할 때 사용 가능.                               | to{noun}<br/>{noun-A}to{noun-B}  | toArray<br/>toMap<br/>domainToPresenter |
| from        | 원하는 값을 가져올 때 인수로 출처가 되는 객체가 필요하면 넣어 준다.                                                            | from{noun}                       | fromPromise                             |
