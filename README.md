# Software Development - Coding Guide

## 1. 명명 규칙
규칙 | 식별자 유형 | 예시
-----|-------------|-----
Camel Case | Variables<br/>Methods | iCount<br/>myHouse
Upper Camel (Pascal) Case | Classes<br/>Methods (C#) | ProStyle<br/>HongKong
Upper Case | Constants | IMAGE_TRAIL<br/>YOUR_DOGGY
Lower Case | UI Controller Instance<br/>ID<br/>Attribute in HTML<br/>Parameters<br/>Table Fields | button_show<br/>list_ducks<br/>item_name<br/>play_count
Dash Case | CSS Classes | tab-wrap<br/>blue-planet

### 1.1. 변수명 규칙
기본적으로 Hungarian Notation을 기반으로 작성 한다.

Data Type | Metadata Rule | Examples
----------|---------------|---------
Integer | i<br/>j (nesting 'for' only)<br/>index<br/>count | iCount<br/>iValue<br/>iPrice
Long | l (L 의 소문자) | lBigNumber
Double | d<br/>dbl | dRate<br/>dHeight<br/>dWidth
Float | f | fResult
Boolean | b<br/>bool<br/>is<br/>has | isDeleted<br/>bReturnValue<br/>hasFruit
Char | ch | chWord
String | s<br/>str<br/>name | sName<br/>sHouse
StringBuilder | sb | sbNations
Array | a<br/>arr | aHistory<br/>arrYears
Map<br/>HashMap<br/>Dictionary (C#) | m | mHost<br/>mParameters<br/>dicData
List | list | listPersnalData
Object | o<br/>obj | oMetaData
DOM Element | elem | elemBody
DOM Document | doc | docNovel
jQuery Object (JS) | jq<br/>$ (php 에서는 사용에 유의) | jqNavigation<br/>$south
Function (JS) | fn<br/>callback | fnCalcProducts
Delegate (C#) | dele | deleClickEvent
Date | date<br/>dt | dateNow<br/>dateYesterday<br/>dtTomorrow
Regexp | regex | regex<br/>regexNamePattern

> php는 Array 사용 시 순서 있는(indexed) 것은 Array 취급, key-value pair 의 연관 있는(Associative) 것은 Map 취급 한다.

### 1.2. 메서드명 규칙
#### 접미사, 동사 (Prefix, Verb)
Rules | Description | Examples
------|-------------|---------
get | 특정 값을 받아올 때 | getName<br/>getAge
set | 특정 값을 설정할 때 | setName<br/>setAge
load | 해당 객체에서 쓰일 자료를 불러올 때 | load<br/>loadData
save | 해당 객체에서 쓰이는 자료를 저장할 때 | saveData
read | 해당 객체에서 쓰일 자료를 특정 파일, 혹은 URL을 통하여 읽어올 때 | readMyBooks<br/>readCountryList
write | 해당 객체에서 쓰이는 자료를 특정 파일, 혹은 URL을 통하여 저장(쓰기) 할 때 | writeMembers<br/>writeFoods
check | 객체의 특정 상태값을 갱신할 때 | checkCount
on | 특정 이벤트 발동 시 수행되는 이벤트 메서드 | onPageClick<br/>onSelectedChange
remove | 객체 내 특정 데이터 일부를 삭제 할 때 | removeFriend
clear | 객체 내 특정 데이터 모두를 삭제 할 때 | clearSolarSystems
is<br/>has | 객체 내 특정 값의 상태를 알아볼 때<br/> | isHuman<br/>hasAnyPens<br/>disabled<br/>enabled<br/>checked
create | 어떤 객체를 생성할 때.<br/>반드시 그 객체를 반환 해야함.<br/>※ 때에 따라 생성자 역할을 맡을 수도 있음 (ex: Delphi, Factory Pattern) | createProject<br/>createAnimals
make | 무언가를 만든다. | makeTags
generate | 객체 내부일 수도, 외부일 수도 있으나 그것이 반드시 객체일 필요는 없다.<br/>반환 타입은 그 결과물을 줄 수도, 만들어진 결과 상태에 대한 Boolean으로 대신할 수 있다. | makeRadar
do | 어떤 행위를 수행한다. | doCollectBalls
run | 정해진 규칙외 특별한 기능을 수행할 필요가 있을 때 쓰임. | doJump<br/>execute<br/>run<br/>execute
add | 객체 내에서 다뤄지는 Collection에 특정 Item을 추가하고자 할 때 쓰임 | addItem<br/>addData
filter | 객체 내의 데이터를 인수의 값을 통하여 필터링한 후 그 값을 가져오거나 이용하고자 할 때. | filter<br/>filterList
convert | 특정 데이터 인수를 다른 형태로 변환하려 할 때 | convertJSONToXML<br/>convertTo<br/>convertToString
to | 현재 객체, 혹은 객체 내부의 특정 자료를 다른 형태로 변환하여 갖고자 할 때 | toString<br/>toArray<br/>toMap<br/>toAnyOthers
apply | 객체가 가진 데이터를 적용시켜 외부, 혹은 내부적으로 그 현황을 반영하고자 할 때 | apply<br/>applyDayInfo<br/>applyIcecreams
show | 객체를 화면상에 보여주거나, 그 객체의 특정 내용을 노출 시킬 때 | show<br/>showPopup<br/>showContainer
hide | 객체를 화면상에서 없애거나, 그 객체의 특정 내용에 대한 노출을 제한 시킬 때 | hide<br/>hideView<br/>hidePersnality
enable<br/>disable | 화면상에 보이는 객체를 사용할 수 있도록 활성화/비활성화 시킬 때.<br/>enable/disable 둘 중 하나만 쓰인다면 Boolean 형 인수를 받아들여 그 값에 따른 행위를 취한다. | enable<br/>enableControls<br/>disableButton
merge | 데이터를 병합 시킨다.<br/> | mergeYearData<br/>mergePhoneList
find | 객체 내 데이터에서 인수 조건에 맞는 데이터를 찾아서 반환 한다. | findMyData<br/>findSubjectName
search | 검색한다.<br/> | search<br/>searchHostData<br/>searchList
init | Initialize. 초기화를 수행 한다.<br/>초기화 할 내용이 많을경우 다수의 초기화 함수 수행 내용을 포함할 수 있다.<br/>기본적으로 수행 후 자기자신을 반환 한다. | init<br/>initEvent<br/>initSubObject<br/>initProperties
extract | 객체 내 특정 데이터나 인수로 받아들여지는 데이터에서 무언가를 추출 해 낼 때 | extractMyData<br/>extractCompanyList

## 2. 소스코드 규칙
### 2.1. 연산자
#### 2.1.1. 산술 연산자
연산자와 피연산자는 항상 띄워준다.

```javascript
// js
/*wrong*/
var iValue=1*iCht;

/*good*/
var iValue = 1 * iCnt;
```

```java
/*good*/
// java, C#
int iValue = 1 * iCnt;
```

```php
// php
$iValue = 1 * $iCnt;
```

#### 2.1.2. 비교 연산자
JS: 비교 연산자 중 동일비교(Equality)는 반드시 =가 3개가 들어가는 Strict Equality 를 사용하도록 한다.  
JS는 약한타입(Weak Type) 언어 특성상 자동 형변환이 이뤄지기에 제대로된 타입 및 값 검사를 할 필요가 있다.  

```javascript
/* js */

// wrong
var isTrue = (hasFood == hasBread);
// good
var isTrue = (hasFood === hasBread);
```

#### 2.1.3. 문자열 연산
문자열을 합치는 연산자와 피연산자 사이도 항상 띄워준다.
```javascript
/* wrong */
var sTemp="nick name: "+sName;

/* good */
var sTemp = "nick name: " + sName;

// php
$sTemp = "nick name: " . $sName;
```

#### 2.1.4. 조합
연산자 조합 때는 의도하는 내용 순서를 지키기 위해 반드시 소괄호 ()를 감싸준다. 상황에 따라 연산 결과가 의도치 않게 달라질 수 있으므로 반드시 지킨다.
```javascript
// wrong
if ( iCount >= iAge != iYear % 5 == 0 && iMonth <= 8 ) {
    
}
// good
if ( ( iCount >= iAge ) != 
     ( ( iYear % 5 ) == 0 ) && 
     ( iMonth <= 8 ) ) {
    
}
```

### 2.2. Coding Style
#### 2.2.1 들여쓰기
들여쓰기 한번은 기본적으로 4-space 를 기준으로 한다.  
(만약 IDE 나 Editor 의 기본 들여쓰기 길이가 다르다면 별도로 설정 한다.)  

java, js 등은 기본적으로 K&R 스타일로 작성 한다.
```javascript
// class
public class DongDong{
    private int age = 0;
    
    public int getAge(){
        return this.age;
    }
}

// conditionals
if ( iVal1 == iVal2 ){
    doSomeProcess();
}
```
C#은 BSD 스타일로 작성 한다.
```cs
// class
public class DongDong
{
    private int age = 0;
    
    public int getAge()
    {
        return this.age;
    }
}

// conditionals
if ( iVal1 == iVal2 )
{
    doSomeProcess();
}
```

#### 2.2.2 문자열 (String)
문자열은 반드시 "" (Double Quotation) 으로 묶는다.  
JS나 PHP는 예외적으로 Tag 작성시에만 '' (Single Quotation)을 허용한다.  
(HTML, XML 태그 등은 속성 삽입 시 ""를 입력하기 더 좋기 때문.)
```javascript
// wrong
var sTmp = 'abcdefg';

$sTmp = 'abcdefg';

// good
var sTmp = "abcdefg";

$sTmp = "abcdefg";

// tag
var sTag = '<div class="header">Documnents</div>';
```
> HTML 태그의 속성은 어느 상황에서든지 반드시 큰따옴표로 묶어야 한다.

ES6 이상을 이용 할 경우, 문자열 붙이기가 많아질 경우, 혹은 다중 라인 (Multi Line)으로 출력 할 일이 있다면 템플릿 문자열 (Template Strings)을 이용한다.
```javascript
var str = `hi there~
I'm fine thank you.
My Name is ${name}.`;
```

#### 2.2.3. 괄호 (Bracket)
괄호는 그 내부 내용에 대하여 모두 공백(space)을 남기는 것이 미관상 보기 좋으나
직접 사용해 보면 IDE 등에서 이를 지키기가 쉽지는 않다.  
(자동 완성 및 자동 입력된 것등을 다시 수정해야 하기 때문)  
또 한 괄호를 연산자처럼 항상 공백을 좌우로 넣는 것은 호불호가 강한 편이다.  
다만 중첩된 괄호등은 연관된 것 들을 묶어주는 의미로 바깥 괄호와 띄워주면 가독성 향상에 도움이 되므로 권장 한다.
```javascript
// before
var isTrue = ((a === 1) && (b !== 1));
String sRet = list[map[mParam["count"]][12]];

// after
var isTrue = ( (a === 1) && (b !== 1) );
String sRet = list[ map[ mParam["count"] ][12] ];
```

함수나 메서드 사용 시 아래처럼 각 인수별로 띄워주면 보기 좋다.
```javascript
// before
someFunction(123, iVal);

// after (recommend)
someFunction( 123, iVal );
```

들어가는 인수가 너무 많거나 길 경우 각 인수별로 줄을 내리고 들여쓰기 하면 가독성이 좋아진다.
```javascript
// before
someFunction( otherFunction( 654, iYear ), extraFunction( iMonth ), 1234, "blah_blah", sTmpName );

// after
someFunction(
    otherFunction( 654, iYear ),
    extraFunction( iMonth ),
    1234,
    "blah_blah",
    sTmpName
);
```

만약 너무 길어 불편하다면 아래와 같이 줄을 바꿔서 작성 한다.
```javascript

// good ??
bool isResult = ( 
    ( 
        ( 344 * iVal1 ) / iTail 
    ) 
    % ( iYear - iYear2 ) 
 )
  > 100;
```

또한 이렇게 긴 수식을 굳이 **억지로 한 줄에 우겨 넣을 필요 없이** 
아래처럼 각 주요 계산부 끼리 변수를 두어 계산하게 하는 것이 더 보기 좋다.
```javascript
// excellent !!
int iResult1 = ( 344 * iVal1 ) / iTail;
int iResult2 = iYear - iYear2;
bool isResult = ( iResult1 % iResult2 ) > 100;
```
사실 위의 코드 정도는 긴 것은 아니지만, 
실제 저것보다 세네베 길어질 경우는 위 처럼 주요 연산 끼리 나누는 것이 합리적이다.

> 센스 있는 코드 작성은 간단하다. 가능한한 산술적으로 유효한 범위의 코드로 쪼개고 쪼개서 나누고 그것을 순차적으로 나열 해 놓으면 되는 것이다.

#### 2.2.4 변수 (Variables)
변수 선언은 항상 해당 함수의 최상위에 위치 하도록 한다. 또 한 변수 선언부와 하부 로직부는 항상 한줄을 기본으로 띄우도록 한다.  

변수는 미리 선언한 것이 아니면 로직 중간에서 사용하는 것, 혹은 중간에 선언하여 사용하는 일이 없어야 한다.  
ES6는 hoisting 문제 때문에라도 반드시 지키도록 한다.
```
// wrong
function calcSomething( param ){
    int iVal1 = param - 10;
    if ( someCondition == iVal1 ){
        String sTmp = "blah";
        Console.WriteLine( sTmp );
    }
}

// good
function calcSomething( param ){
    int iVal1 = param - 10;
    String sTmp = "";
    
    if ( someCondition == iVal1 ){
        sTmp = "blah";
        Console.WriteLine( sTmp );
    }
}
```

java나 C# 같은 Native Language의 변수는 특수한 상황이 아닌 이상 반드시 초기화(Initialize) 하도록 한다.  
하부 로직에 의해 오류가 날 가능성이 있기 때문이다.
```
// wrong
function calcSomething( param ){
    int iVal;
    String sName;
    
    if ( param == true ){
        sName = "blah";
        iVal = 12;
    }
    
    Console.WriteLine( iVal + sName ); // Exception Occurs !!
}

// good
function calcSomething( param ){
    int iVal = 0;
    String sName = "";
    
    if ( param == true ){
        sName = "blah";
        iVal = 12;
    }
    
    Console.WriteLine( iVal + sName );
}
```

JavaScript는 변수 선언문이 하나(var) 뿐이므로 변수가 여러개일 경우 다음과 같이 연속적으로 작성 한다.
```javascript
// normal
function calcSomething( param ){
    var iVal;
    var sName;
    var dCurrency;
    var mData;
    var isPriday;
    
    // ... some codes
}

// good
function calcSomething( param ){
    var iVal,
        sName,
        dCurrency,
        mData,
        isPriday;
    
    // ... some codes
}
```

아래와 같은 방법도 좋다.
```javascript
// good
function calcSomething( param ){
    var iVal
    , sName
    , dCurrency
    , mData
    , isPriday
    ;
    
    // ... some codes
}
```

ES6는 매우 유연한 언어이기 때문에 외부에서 온 인수(Argument)의 유효성을 함께 검증하면 좋다.
```javascript
function someone(param){
	if (!param){
		// ... other ways
	}
	else{
		// ... logics
	}
}
```

ES6는 기본형 (Primitive Type)일 경우 그 값을 타입에 맞게 초기화 해 주고, 
객체형 (Object Type)일 경우 하단의 로직부에서 undefined 여부를 대신 확인하도록 한다.
```javascript
function calcSomething( param ){
    var iVal = 0,
    , sName = ""
    , dCurrency = 0.0
    , mData // may be undefined...
    , isPriday = false
    ;
    
    // ... some logics
    
    if ( !mData ){
        // ... when incorrect
    }
    else{
        // ... when correct
    }
}
```

PHP는 변수 선언문도 존재하지 않으므로 함수 상단, 혹은 php 스크립트 선언문 상단에 별도의 변수 영역을 두어 미리 선언하고 사용토록 한다.  
물론 그에 따른 변수 초기화는 필수이며, 이 역시 객체형일 경우 기본을 null로 주되, 하단 로직에서 해당 변수에 대한 별도의 확인을 반드시 거치도록 한다.
```php
// var [begin]
$iVal = 0;
$sName = "";
$dCurrency = 0.0;
$mData;
$isPriday = false;
// var [end]

// ... some logics

if ( isset( $mData ) ){
    // ... when incorrect
}
else{
    // ... when correct
}
```

> PHP에서 쓰이는 일부 문법들은 타 언어에서는 유효하지 않는 경우가 종종 있다. 이를 다른 언어들의 특성에 맞춰 언어적 통일성을 유지할 수 있게 작성하면 타 언어 개발자도 필요 시 쉽게 확인이 가능 할 것이다.

#### 2.2.5. 세미콜론 생략 (Missing Semicolon)
JS 같은 경우 종종 한 문장 종료 후 세미콜론 (;)을 뒤에 달지 않아도 문제가 없는 경우가 있다. 
그래서 때에 따라 달거나 달지 않는 식으로 코딩을 하는 경우가 있겠으나 절대 그러지 않길 바란다.  

세미콜론을 문장끝에 작성하는 언어에서는 그 언어 자체에서 강제하는게 아닌 이상 꼭 세미콜론을 문장끝에 작성 하도록 하자.  
(그러므로 굳이 언제 넣고 빼도 되는지에 대한 설명은 생략한다. 몰라도 개발엔 전혀 지장이 없다.)
```
// wrong
function getName(){
	return "my name"
}

// good
function getName(){
	return "my name";
}
```
JS에서 AMD로 활용 시 이전 코드와 엮일 때 문법 오류 (Syntax Error)가 발생되는 경우가 종종 있다.  
다름아닌 이전 코드의 마무리가 세미콜론 등으로 잘 이뤄지지 않아서인 경우가 많다.  
때문에 아래처럼 AMD로 모듈 구성 시 파일 시작은 항상 세미콜론으로 시작되도록 하는 것이 권장 된다.  
물론 뒷쪽도 반드시 세미콜론으로 마무리 해 준다.
```javascript
// before
require("module", ["lib1", "lib2"], function(lib1, lib2){
	// .. some logics
})

// after
;require("module", ["lib1", "lib2"], function(lib1, lib2){
	// .. some logics
});
```

### 2.3. 로직 블록 (Logic Block)
if 와 같은 제어문이나 for 와 같은 반복문은 그 블럭 내용이 한 줄일 경우 블럭문 - 중괄호 ({})를 생략하는 경우가 있다.
하지만 **절대 그러지 않는다.**
```csharp
// wrong
if ( hasTry == true ) Console.WriteLine( "Tried." );

// good
if ( hasTry == true ){
    Console.WriteLine( "Tried." );
}
```

다만 아래처럼 로직 특성상 if ~ else if 문이 연속으로 여러개 달리게 된 상황이라 
오히려 중괄호를 붙이지 않았을 때 가독성이 더 좋다면 그리 하도록 한다.

이 때 if 절과 else if 절의 비교 문장 및 수행 문장은 아래와 같이 열을 맞춰 준다. 
이 것은 상황에 따라 서로가 좋은 쪽으로 맞추도록 한다.
```java
// good
if ( sVal == "cat" ){
    Console.WriteLine( "Kitten" );
}
else if ( sVal == "dog" ){
    Console.WriteLine( "Doggy" );
}
else if ( sVal == "cow" ){
    Console.WriteLine( "Calf" );
}
else if ( sVal == "Horse" ){
    Console.WriteLine( "Colt" );
}
else if ( sVal == "Human" ){
    Console.WriteLine( "Baby" );
}
```
위의 if ~ else if 절을 아래와 같이 깔끔하게 구성하는 경우도 있다.
```java
// better !
if      ( sVal == "cat" )   Console.WriteLine( "Kitten" );
else if ( sVal == "dog" )   Console.WriteLine( "Doggy" );
else if ( sVal == "cow" )   Console.WriteLine( "Calf" );
else if ( sVal == "Horse" ) Console.WriteLine( "Colt" );
else if ( sVal == "Human" ) Console.WriteLine( "Baby" );
```
다만 이는 일종의 **꼼수**에 불과하며 업무 특성상 아래처럼 조건이 중간에 추가되거나 하면 보기 좋지 않다.
```java
// bad
if      ( sVal == "cat" )   Console.WriteLine( "Kitten" );
else if ( sVal == "dog" || sVal == "hot dog" )   Console.WriteLine( "Doggy" );
else if ( sVal == "cow" )   Console.WriteLine( "Calf" );
else if ( sVal == "Horse" || sVal == "donkey" ) Console.WriteLine( "Colt" );
else if ( sVal == "Human" ) Console.WriteLine( "Baby" );
```
그러므로 위의방법을 아래처럼 switch ~ case 문으로 바꾸도록 한다.  
switch ~ case 의 비교값은 그 타입이 생각보다(?) 유연하므로 가능한 한 아래의 것을 사용토록 한다.
```java
// switch ~ case
switch (sVal){
    case "cat":
        Console.WriteLine( "Kitten" );
        break;
    case "dog":
        Console.WriteLine( "Doggy" );
        break;
    case "cow":
        Console.WriteLine( "Calf" );
        break;
    case "Horse":
        Console.WriteLine( "Colt" );
        break;
    case "Human":
        Console.WriteLine( "Baby" );
        break;
}
```
조건이 추가 되어도 깔끔하다!
```java
// switch ~ case
switch (sVal){
    case "cat":
        Console.WriteLine( "Kitten" );
        break;
    case "dog":
    case "hot dog":
        Console.WriteLine( "Doggy" );
        break;
    case "cow":
        Console.WriteLine( "Calf" );
        break;
    case "Horse":
    case "donkey":
        Console.WriteLine( "Colt" );
        break;
    case "Human":
        Console.WriteLine( "Baby" );
        break;
}
```
만약 이렇게 비교할 내용이 몇십~몇백 라인이 넘어가게 된다면 이건 소스코드로 해결할 문제는 아닐 것이다. 
이 때는 그런 데이터들을 여러가지 Collection - Map, Array, List - 등으로 만들어 쓰거나 
그것으로 감당하기에도 역부족이라면 외부의 Text, Excel, XML, DB 등을 통한 
별도의 데이터 관리가 필요하다는 의미로 해석할 수 있을 것이다.

### 2.4. 함수 코드 크기 (Function Code Scale)
특별한 사유가 없는 한 만들어질 함수, 혹은 메서드의 코드 길이는 현재 보이는 화면 해상도의 세로 길이 - 일반적으로 약 800px - 를 넘지 않는다.  
(와이드 모니터를 세로로 놓고 보는 경우는 보이는 화면의 절반으로 제한 한다.)

만약 지나치게 길어진다면 해당 함수의 기능을 세분화 하여 나누어 그것을 다시 재사용 하도록 바꿔야 한다.  
만약 나눌 방법이 없다면 해당 함수를 사용하는 로직에 대하여 재정의를 하거나 재구현 할 필요가 있다.  
(즉 다시 생각하거나 갈아 엎어라...)  

본디 코드는 길어질 수록 버그가 잦고 유지 보수를 하기가 힘들어지는 것이 일반적이기에
가급적 짧고 명료하게 코드를 짤 수 있도록 생각하고 노력하자.
```java
// wrong
function Map clacSomeData( Map data ){
    int iSum = 0;
    double dDcRate = 0.0;
    int iPrice = 0;
    int iCount = 0;
    int iTotalPrice = 0;
    Map mRet = new Map();
    
    if ( data[ "price" ] ){
        for ( int i = 0; i < data[ "price" ].length; i++ ){
            iPrice += data[ "price" ][ i ];
        }
    }
    
    if ( data[ "count" ] ){
        for ( int i = 0; i < data[ "count" ].length; i++ ){
            iCount += data[ "count" ][ i ];
        }
    }
    
    if ( data[ "dcRate" ] ){
        for ( int i = 0; i < data[ "dcRate" ].length; i++ ){
            dDcRate += data[ "dcRate" ][ i ];
        }
        dDcRate = dDcRate / data[ "dcRate" ].length;
    }
    
    if ( data[ "sum" ] ){
        for ( int i = 0; i < data[ "sum" ].length; i++ ){
            iSum += data[ "sum" ][ i ];
        }
    }
    
    iTotalPrice = (int)( iPrice * dDcRate );
    
    mRet[ "price" ]         = iPrice;
    mRet[ "count" ]         = iCount;
    mRet[ "dcRate" ]        = dDcRate;
    mRet[ "sum" ]           = iSum;
    mRet[ "totalPrice" ]    = iTotalPrice;
    
    return mRet;
}


// good
function int calcPrice( int[] data ){
    int iPrice = 0;

    if ( data ){
        for ( int i = 0; i < data.length; i++ ){
            iPrice += data[ i ];
        }
    }
    
    return iPrice;
}

function int calcCount( int[] data ){
    int iCount = 0;

    if ( data ){
        for ( int i = 0; i < data.length; i++ ){
            iCount += data[ i ];
        }
    }
    
    return iCount;
}

function double calcDcRate( int[] data ){
    int dDcRate = 0.0;

    if ( data ){
        for ( int i = 0; i < data.length; i++ ){
            dDcRate += data[ i ];
        }
    }
    
    return dDcRate;
}

function int calcSum( int[] data ){
    int iSum = 0;

    if ( data ){
        for ( int i = 0; i < data.length; i++ ){
            iSum += data[ i ];
        }
    }
    
    return iSum;
}

function int calcTotalPrice( int price, double dcRate ){
    return (int)( iPrice * dDcRate );
}

function Map clacSomeData( Map data ){
    int iSum        = calcSum( data[ "sum" ] );
    double dDcRate  = calcDcRate( data[ "dcRate" ] );
    int iPrice      = calcPrice( data[ "price" ] );
    int iCount      = calcCount( data[ "count" ] );
    int iTotalPrice = calcTotalPrice( iPrice, dDcRate );
    Map mRet        = new Map();
    
    mRet[ "price" ]         = iPrice;
    mRet[ "count" ]         = iCount;
    mRet[ "dcRate" ]        = dDcRate;
    mRet[ "sum" ]           = iSum;
    mRet[ "totalPrice" ]    = iTotalPrice;
    
    return mRet;
}
```
사실 위의 예시는 다소 억지가 있으나 대략적인 방법을 설명하고 제시하는 것이 목적임을 알아두자.
> 함수의 길이는 오류가 발생될 가능성과 비례하며, 그만큼 디버깅에 시간을 많이 투자하게 된다.

### 2.5. 캐싱 (Caching)
캐싱이란 반복적으로 사용될 수 있는 변수에 할당을 미리 해 두고 그 것을 지속적으로 사용하는 것을 뜻한다.  
일반적으론 변수를 활용함에 있어 캐싱을 알게모르게 사용 되고 있을 것이다. 
다만 꼭 필요한 경우 임에도 불구하고 캐싱을 하지 않는 경우가 있는데 다음과 같은 경우다.
```java
// wrong
function int sumData( Map mData ){
    int iSum = 0;

    for ( i = 0; i < mData[ "quantity" ].length; i++ ){
        iSum += (int)mData[ "quantity" ][ i ];
    }
    
    return iSum;
}
```
요즘엔 컴파일러나 인터프리터 성능이 좋아서 저런 코드쯤은 최적화를 해 준다고는 하지만, 모든 상황에서, 
그리고 모든 언어에서 조건없이 다 최적화를 해 주리라 기대해선 안된다. 
특히 저런 것과 유사한 상황이 반복되고 반복문이 연달아 수행되거나 중첩된다면 
해당 로직에 대한 퍼포먼스는 기대하기 어려우므로 아래처럼 캐싱을 수행 하도록 하자.
```java
// good
function int sumData( Map mData ){
    int iSum = 0;
    List listQuantity = mData[ "quantity" ];
    int iLen = listQuantity.length;
    
    for ( i = 0; i < iLen; i++ ){
        iSum += (int)listQuantity[ i ];
    }
    
    return iSum;
}
```
추가적으로 Map에 정의된 특정값을 호출 할 때는 
그 값이 정의되어 있지 않거나 비어있을 수 있으므로 캐싱된 컬렉션을 체크하는 것은 기본이다.
```java
// excellent !
function int sumData( Map mData ){
    int iSum = 0;
    List listQuantity = mData[ "quantity" ];
    int iLen = 0;
    
    if ( listQuantity != null ){
        iLen = listQuantity.length;
    }
    
    for ( i = 0; i < iLen; i++ ){
        iSum += (int)listQuantity[ i ];
    }
    
    return iSum;
}
```

## 3. 주의할 점 (Cautions)
소스코드를 작성함에 있어 사소한 문제와 습관이 연계되어 큰 오류로 번지고는 한다. 대체로 이러한 경우는 프로그래밍 언어에 대한 이해도 및 경험이 부족하거나 바쁜 일정에 쫓기어 코드를 구조화/모듈화를 하지 않는 것이 큰 원인이다.

프로그램은 잘게 쪼개어 나누면 결국 무언가를 저장하는 것(변수)과 무언가를 수행하는 것(함수)로 나뉠 수 있는데 이 둘에 대한 명확한 업무적 구분이 모호한 상태에서 업무에 억지로 적용 시키려다 보니 로직이 꼬이거나 실수가 잦은 것이다.

### 3.1. 전역 변수 및 함수 (Global Variables & Functions)
그 어떤 언어든지 그 어떤 상황에서도 **전역변수는 절대 쓰지 않는다.**  
코드가 지저분해 지는 원인 중 하나가 되고 이곳저곳에서 참조 하기 때문에 상황에 따라 의도치 않은 결과를 이끌어내기 딱 좋다.

전역변수는 상황에 따라 필요악일 수 있으나 그 특성과 장점(어디서든지 접근 가능한)으로 인해 수많은 프로그래머들이 오용하고 있으며 
애초에 전역변수가 아니면 로직을 제대로 구현 못하는 경우도 부지기수 이다.

본인이 전역변수나 static을 너무 남발하는 것이 아닌지 되새겨본 뒤, 그 정도가 심하다면 본인의 코딩 습관과 로직 제작 패턴을 달리 할 필요가 있다.

#### 3.1.1. Solution: JavaScript
JS에서는 특히 전역변수에 대한 문제가 심각한데, 자체적으로 지원되는 Namespace나 Block Scope 개념이 없기 때문이다. 
오직 Function Scope 밖에 존재하지 않으며 이에 대한 해결 법이 다양하게 존재한다.
```javascript
/***** js *****/
//wrong
var g_value = 0.1;

function getValue(){
    return g_value;
}

var g_result = getValue();
```

##### Object Namespace
JS에는 Object Type이 있으며 이 것은 Property라 불리는 Key와 Value의 쌍으로 이뤄져 있다. 이 것을 응용하는 방법이다.
```javascript
/***** js *****/
//good
var project = project || {};

project.ns = {
    value: 0.1,
    
    getValue: function( value ){
        return this.value;
    }
};

var g_result = project.ns.getValue();
```

##### Anonymous Functions
익명함수를 통해 간접적으로 접근한다. 이 때 필연적으로 클로저(Closure)가 사용된다.  
이렇게 작성될 경우 Function Scope 로 바뀌기 때문에 Global Scope와 단절되어 아낌없이 코드를 마음껏 쓸 수 있게 된다.  
(물론 모듈화 하는 작업을 함께 하는 것이 바람직하다.)
```javascript
/***** js *****/
//good
var g_result = (function(){
    var value = 0.1;
    
    function getValue(){
        return value;
    }
    
    return getValue();
})();

// do something..
```
아래와 같이 그냥 익명함수 영역에서 처리하고 끝내버려서 아예 Global Scope와 단절 시키는 것도 좋다.
```javascript
/***** js *****/
//best
(function(){
    var value = 0.1
	, dResult = 0
    ;
    
    function getValue(){
        return value;
    }
    
    dResult = getValue();

    // do something..
})();
```

#### 3.1.2. Solution: Java (or C#)
Java는 특성상 로직을 Class 기반으로 만들어야 하는데, 
이 때문에 Main Class 에 불필요한 멤버 변수를 수없이 배치 해 놓거나 특정 Class를 만들어 놓고 
그 안의 멤버 변수를 public static 으로 도배를 하는 형식으로 전역변수 활용을 꽤하기도 한다.
```java
/***** java *****/
//wrong
package project;

class Main{
    public static double value = 0.1;
    public static int age = 20;
    public static String name = "TheSON";
    
    public void run(){
        // blah blah ...
    }
}

public class Controller{
    public void execute(){
        double iValue = Main.value;
        
        // Logic ...
    }
}
```
Java나 C#같은 OOP가 강조된 언어는 이러한 전역변수 문제와 더불어 여러가지 다양한 해법들을 제시 하는데, 
그런 해법중 하나가 바로 디자인 패턴(Design Pattern)이다. 
물론 꼭 전역변수 때문에 디자인 패턴이 생겨난 것은 아니다. 
단지 디자인 패턴중엔 이런 문제점의 해결책이 포함 되어 있다는 것이다.  
(물론 이게 Java나 C#만 적용시키라고 존재하는 것은 결코 아니다.)

##### Singleton Pattern
다른 곳에서 중복적으로 사용되나 하나만 만들어서 써도 될 경우 이용된다. 개념을 설명하자면, 내부적으로 static을 가지며 이 것은 외부에서 직접적으로 참조할 수 없다. 
다만 getInstance와 같은 외부참조 메서드만을 가지며 이것은 static에 선언된 Instance Object를 반환시키는 역할을 한다.

즉 싱글톤 패턴을 가지는 객체는 스스로 딱1번만 Instance를 하며 그 자체를 외부에서는 절대 간섭하지 못한다. 
만약 간섭이 가능하다면 그 자체는 이미 싱글통 패턴이 아니다.

주의점은 Multi-Thread 환경일 경우 Synchronize 와 Multi-Access 의 문제를 가지므로 이에 대한 적절한 해결책이 필요하다. 
메서드로만 이뤄져 있다면 상관 없겠으나 자체적인 멤버 속성을 가졌다면 Thread-Safe 하게 객체 설계를 할 필요성이 있다.
```java
/***** java *****/
// good

// class
public class ValueMaker{
    private static ValueMaker my = null;
    
    private double value;
    
    private ValueMaker(){
        this.value = 0.1;
    }
    
    public static ValueMaker getInstance(){
        if (ValueMaker.my == null){
            ValueMaker.my = new ValueMaker();
        }
        
        return ValueMaker.my;
    }
    
    public double getValue(){
        return this.value;
    }
}

// execution
class Main{
    public void run(){
        double dValue = ValueMaker.getInstance().getValue();
        
        // Logic ...
    }
}
```
##### Factory Pattern
Global 형태로 필요한 기능이 담긴 객체를 미리 정의하고 필요할 때 마다 불러서 쓰는 패턴이다.

일반적으로 Abstract Class 에 static method 로 create를 만들고 이 메서드에 특정 인수를 넣어줌으로써 
그에 대응되는 Instance Object를 얻는 것이 특징이다. 이 때 얻게되는 Object는 Factory 역할을 하는 Abstract class를 상속받거나 
interface를 구현한 것으로 만들어지는 것이 보통이다.
```java
/***** java *****/
// good

// abstract class or interface
public interface IValueProvider{
    public double getValue();
}

// factory class
public class ValueProviderFactory{
    private class DoubleValueProvider implements IValueProvider{{
        private double value = 0.1;
        
        @override
        public double getValue(){
            return this.value;
        }
    }
    
    private class PriceValueProvider implements IValueProvider{{
        private double value = 10000.101;
        
        @override
        public double getValue(){
            return this.value;
        }
    }
    
    public static IValueProvider create(String name){
        if ( "double".equals( name ) == true ){
            return new DoubleValueProvider();
        }
        if ( "price".equals( name ) == true ){
            return new PriceValueProvider();
        }
        
        return null;
    }
}

// execution
class Main{
    public void run(){
        double dValue = ValueProviderFactory.create( "double" ).getValue();
        
        // Logic ...
    }
}
```

## 코딩 가이드는 규칙이 떠올려 지는대로  계속 작성 합니다 :)
