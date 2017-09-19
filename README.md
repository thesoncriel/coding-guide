# Software Development - Coding Guide

## 1. 명명 규칙
규칙 | 식별자 유형 | 예시
-----|-------------|-----
Camel Case | Variables<br/>Methods | iCount<br/>myHouse
Upper Camel (Pascal) Case | Classes<br/>Methods (C#) | ProStyle<br/>HongKong
Upper Case | Constants | IMAGE_TRAIL<br/>YOUR_DOGGY
Lower Case | UI Controller Instance<br/>ID<br/>Attribute in HTML<br/>Parameters<br/>Table Fields | button_show<br/>list_ducks<br/>item_name<br/>play_count
Dash Case | CSS Classes | tab-wrap<br/>blue-planet

### 1.1. 변수명
기본적으로 Hungarian Notation을 기반으로 작성 한다.

Data Type | Metadata Rule | Examples
----------|---------------|---------
Integer | i<br/>j (nesting 'for' only)<br/>index<br/>count | iCount<br/>iValue<br/>iPrice
Long | l (L 의 소문자) | lBigNumber<br/>Double | d<br/>dbl | dRate<br/>dHeight<br/>dWidth
Float | f | fResult
Boolean | b<br/>bool<br/>is<br/>has | isDeleted<br/>bReturnValue<br/>hasFruit
Char | ch | chWord
String | s<br/>str<br/>name | sName<br/>sHouse
Array | a<br/>arr | aHistory<br/>aYears
Map<br/>HashMap<br/>Dictionary (C#) | m | mHost<br/>mParameters<br/>dicData
List | list | listPersnalData
Object | o<br/>obj | oMetaData
DOM Element | elem | elemBody
DOM Document | doc | docNovel
jQuery Object (JS) | jq<br/>$ (php 에서는 사용에 유의) | jqNavigation<br/>$south
Function (JS) | fn | fnCalcProducts
Delegate (C#) | dele | deleClickEvent
Date | date<br/>dt | dateNow<br/>dateYesterday<br/>dtTomorrow
Regex | regex | regex<br/>regexNamePattern
