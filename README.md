# Software Development - Coding Guide

## 1. 명명 규칙
규칙 | 식별자 유형 | 예시
-----|-------------|-----
Camel Case | Variables  Methods | iCount  myHouse
Upper Camel (Pascal) Case | Classes  Methods (C#) | ProStyle  HongKong
Upper Case | Constants | IMAGE_TRAIL  YOUR_DOGGY
Lower Case | UI Controller Instance  ID  Attribute in HTML  Parameters  Table Fields | button_show  list_ducks   item_name  play_count
Dash Case | CSS Classes | tab-wrap  blue-planet

### 1.1. 변수명
기본적으로 Hungarian Notation을 기반으로 작성 한다.
Data Type | Metadata Rule | Examples
----------|---------------|---------
Integer | i  j (nesting 'for' only)  index  count | iCount  iValue  iPrice
Long | l (L 의 소문자) | lBigNumber  Double | d  dbl | dRate  dHeight  dWidth
Float | f | fResult
Boolean | b  bool  is  has | isDeleted  bReturnValue  hasFruit
Char | ch | chWord
String | s  str  name | sName  sHouse
Array | a  arr | aHistory  aYears
Map  HashMap  Dictionary (C#) | m | mHost  mParameters  dicData
List | list | listPersnalData
Object | o  obj | oMetaData
DOM Element | elem | elemBody
DOM Document | doc | docNovel
jQuery Object (JS) | jq  $ (php 에서는 사용에 유의) | jqNavigation  $south
Function (JS) | fn | fnCalcProducts
Delegate (C#) | dele | deleClickEvent
Date | date  dt | dateNow  dateYesterday  dtTomorrow
Regex | regex | regex  regexNamePattern
