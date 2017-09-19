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

### 1.2. 메서드명 규칙
Rules | Discription | Examples
------|-------------|---------
get	특정 값을 받아올 때	getName<br/>getAge
set	특정 값을 설정할 때	setName<br/>setAge
load	해당 객체에서 쓰일 자료를 불러올 때	load<br/>loadData
save	해당 객체에서 쓰이는 자료를 저장할 때	saveData
read	해당 객체에서 쓰일 자료를 특정 파일, 혹은 URL을 통하여 읽어올 때	readMyBooks<br/>readCountryList
write	해당 객체에서 쓰이는 자료를 특정 파일, 혹은 URL을 통하여 저장(쓰기) 할 때	writeMembers<br/>writeFoods
check	객체의 특정 상태값을 갱신할 때	checkCount
on	특정 이벤트 발동 시 수행되는 이벤트 메서드	onPageClick<br/>onSelectedChange
remove	객체 내 특정 데이터 일부를 삭제 할 때	removeFriend
clear	객체 내 특정 데이터 모두를 삭제 할 때	clearSolarSystems
is	객체 내 특정 값의 상태를 알아볼 때	isHuman
has	※ 반환 타입은 반드시 Boolean 이어야 한다.	hasAnyPens<br/>disabled<br/>enabled<br/>checked
create	어떤 객체를 생성할 때.<br/>반드시 그 객체를 반환 해야함.<br/>※ 때에 따라 생성자 역할을 맡을 수도 있음 (ex: Delphi, Factory Pattern)	createProject<br/>createAnimals
make	무언가를 만든다.	makeTags
generate	객체 내부일 수도, 외부일 수도 있으나 그것이 반드시 객체일 필요는 없다.<br/>반환 타입은 그 결과물을 줄 수도, 만들어진 결과 상태에 대한 Boolean으로 대신할 수 있다.	makeRadar
do	어떤 행위를 수행한다.	doCollectBalls
run	정해진 규칙외 특별한 기능을 수행할 필요가 있을 때 쓰임.	doJump<br/>execute<br/>run<br/>execute
add	객체 내에서 다뤄지는 Collection에 특정 Item을 추가하고자 할 때 쓰임	addItem<br/>addData
filter	객체 내의 데이터를 인수의 값을 통하여 필터링한 후 그 값을 가져오거나 이용하고자 할 때.	filter<br/>filterList
convert	특정 데이터 인수를 다른 형태로 변환하려 할 때	convertJSONToXML<br/>convertTo<br/>convertToString
to	현재 객체, 혹은 객체 내부의 특정 자료를 다른 형태로 변환하여 갖고자 할 때	toString<br/>toArray<br/>toMap<br/>toAnyOthers
apply	객체가 가진 데이터를 적용시켜 외부, 혹은 내부적으로 그 현황을 반영하고자 할 때	apply<br/>applyDayInfo<br/>applyIcecreams
show	객체를 화면상에 보여주거나, 그 객체의 특정 내용을 노출 시킬 때	show<br/>showPopup<br/>showContainer
hide	객체를 화면상에서 없애거나, 그 객체의 특정 내용에 대한 노출을 제한 시킬 때	hide<br/>hideView<br/>hidePersnality
enable	화면상에 보이는 객체를 사용할 수 있도록 활성화/비활성화 시킬 때.<br/>disable	enable/disable 둘 중 하나만 쓰인다면 Boolean 형 인수를 받아들여 그 값에 따른 행위를 취한다.	enable<br/>enableControls<br/>disable<br/>disableButton
merge	데이터를 병합 시킨다.<br/>	mergeYearData<br/>mergePhoneList
find	객체 내 데이터에서 인수 조건에 맞는 데이터를 찾아서 반환 한다.	findMyData<br/>findSubjectName
search	검색한다.<br/>	search<br/>searchHostData<br/>searchList
init	Initialize. 초기화를 수행 한다.<br/>초기화 할 내용이 많을경우 다수의 초기화 함수 수행 내용을 포함할 수 있다.<br/>기본적으로 수행 후 자기자신을 반환 한다.	init<br/>initEvent<br/>initSubObject<br/>initProperties
extract	객체 내 특정 데이터나 인수로 받아들여지는 데이터에서 무언가를 추출 해 낼 때	extractMyData<br/>extractCompanyList

