# CBD

CBD (Component Based Development) 는 SW 개발 시 필요한 세부 컴포넌트를 만들고 이를 조합하여 사용하는 방법을 말한다.

이 때 아래와 같은 내용을 지키도록 한다.

- 외부에서 해당 컴포넌트를 사용함에 있어 내부 마크업(Markup)에 관심이 없도록 한다.
- 충분히 독립적이어야 한다.
  - 입력 필수인 프로퍼티를 제외하면, 그냥 갖다 두어도 정상 동작 되어야 한다.
- 쓰여지는 프로퍼티 (Property)는 UL (Ubiquitous Language)에 가까워야 한다.
  - ex) value, text, checked, disabled, onClick, onChange, className, to, src 등
  - 기존 마크업에서 이용되거나 널리 활용되어 특별한 설명이 없어도 이해할 수 있는 간단한 용어로 이뤄져야 함
- 표현 (Presentation)과 사용자 행동 (User Action)에 중점을 두어야 한다.
- 컴포넌트는 외부 Domain Model 에 의존적이지 않아야 한다.
  - Domain 이 현재 UI 에 맞지 않다면 자체적인 Presentation Model 을 두어 상호 변환하여 쓰도록 유도한다.
