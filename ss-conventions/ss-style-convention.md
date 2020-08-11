# Web Frontend - 컴포넌트 스타일 규칙

이 곳은 Web 2.0 적용 대비 하여 작성된 컴포넌트의 각종 스타일 적용 규칙이 기록된 곳입니다.

현재 `React` 와 스타일링 라이브러리인 `styled-components`를 채택하여 사용하고 있으며 이들에 대한 공통적인 사용 방법에 대하여 작성 해 두었습니다.

이 후 컴포넌트 스타일 작성 하실 땐 아래를 참고하여 작업 해주세요! 🙂

(필요에 따라 import 나 기타 코드가 생략되어 있습니다.)

## 스타일 선언은 `styled` 로만 사용

styled-components 에서 기본적으로 제공되는 `styled` 함수를 통하여 CSS 를 선언하고 사용합니다.

사유는 `JSX 문법 내에서 스타일링에 대한 관심사 분리를 하기 위함` 입니다.

아래는 작성 예시 입니다.

```tsx
import React, { FC } from 'react';
import styled from 'styled-components';

const StyledDiv = styled.div`
  display: inline-block;
  background: skyblue;
`;

export const InlineBlock: FC = () => {
  return <StyledDiv>block block ...</StyledDiv>;
};
```

## css props 는 간단히 1가지 속성만을 적용할 때 사용

JSX 의 스타일링 관심사 분리를 위해 styled 사용을 준수해야 하나, 매우 간단한 1가지 css 속성을 붙일 때는 예외적으로 허용 합니다.

아래는 그러한 예시 입니다.

```tsx
import React, { FC } from 'react';
import styled from 'styled-components';

const StyledDiv = styled.div`
  margin-right: 1rem;
`;

export const InlineBlock: FC = () => {
  return <StyledDiv>block block ...</StyledDiv>;
};
```

위를 css props 사용 방법으로 바꾸면 아래와 같습니다.

```tsx
export const InlineBlock: FC = () => {
  return (
    <div
      css={`
        margin-right: 1rem;
      `}
    >
      block block ...
    </div>
  );
};
```

또는 css 함수를 함께 이용할 수 있습니다.

```tsx
import { css } from 'styled-components';

export const InlineBlock: FC = () => {
  return (
    <div
      css={css`
        margin-right: 1rem;
      `}
    >
      block block ...
    </div>
  );
};
```

만약 부득이하게 저러한 스타일링 코드가 2가지 이상이 될 경우 아래와 같이 리팩터링을 합니다.

```tsx
// before
export const InlineBlock: FC = () => {
  return (
    <div
      css={css`
        margin-right: 1rem;
        padding: 1rem;
      `}
    >
      block block ...
    </div>
  );
};

// after
const StyledDiv = styled.div`
  margin-right: 1rem;
  padding: 1rem;
`;

export const InlineBlock: FC = () => {
  return <StyledDiv>block block ...</StyledDiv>;
};
```

## className props 사용

사용될 컴포넌트에 추가적인 스타일링이 필요하다면 아래와 같이 **styled** 함수로 해당 컴포넌트의 스타일을 확장하여 사용 합니다.

```tsx
import React, { FC } from 'react';
import styled from 'styled-components';
import { CardItem as CardItemBase } from '../atoms';

const CardItem = styled(CardItemBase)`
  border-color: darkblue;
  font-size: 2rem;
`;

export const ClientComp: FC = () => {
  return <CardItem title="제목이당" keyword="키워드당" />;
};
```

한편 이렇게 styled 로 스타일을 확장하려면 해당 컴포넌트에 **className** props 가 있어야 합니다.

이건 아래와 같이 미리 선언된 `ClassNameProps` 인터페이스를 이용하여 컴포넌트의 Props 에 extends 하십시요.

```tsx
// 본 인터페이스는 공통 모듈에 선언되어 있습니다.
import { ClassNameProps } from '../../common';

// 인터페이스를 상속 받습니다.
interface Props extends ClassNameProps {
  title?: string;
  keyword?: string;
}

// 추가적인 스타일이 적용될 하위 컴포넌트의 className 에 전달 합니다.
export const CardItem: FC<Props> = ({ keyword, title, className }) => {
  return (
    <div className={className}>
      <h4>{title}</h4>
      <p>{keyword}</p>
    </div>
  );
};
```

## color 나 size 등 고정값은 공용 테마(theme)에 지정된 것을 활용

컴포넌트에 스타일링을 하는 것은 매우 중요한 일입니다.

다만 그 작성된 수치들이 모든 컴포넌트가 제각각일 경우 통제하기가 힘들어집니다.

따라서 반복되는 수치나 고정값등은 아래와 같이 style theme 를 적극 활용합니다.

아래는 예시 입니다. (이 후 정책에 따라 실제 접근하는 필드는 달라질 수 있습니다.)

```tsx
// 테마에 지정된 컬러값을 사용합니다.
const StyledDiv = styled.div`
  color: ${({ theme }) => theme.color.gray40};
`;

export Sample: FC = () => {
  return (
    <StyledDiv>CSS forever!</StyledDiv>
  );
};
```

### font-size 의 값은 theme 활용

사용자의 확대 축소에 유연하게 반응하기 위하여 문자(font)에 한하여 px 보단 `rem` 을 권장합니다.

단, 디자인 시스템 가이드에 따라, 쓰여져야 할 크기는 테마(theme)에 미리 정해져 있으므로 이를 활용합니다.

아래는 예시 입니다. (테마 컬러에 skyblue 라는 값이 있다는 가정)

```tsx
const StyledAnchor = styled.a`
  display: block;
  font-size: ${({ theme }) => theme.color.skyblue};
`;
```

만약 부득이하게 customize 해야 할 경우 [polished](https://github.com/styled-components/polished) 라이브러리에서 제공되는 **rem** 함수로 `px to rem` 변환해서 사용 합니다.

이 때는 별도로 사용하는 이유에 대하여 동료 분들에게 가급적 공유 하도록 합시다. 🙂

```tsx
import { rem } from 'polished';

// 14px 을 그에 맞는 rem 단위로 바꿔줍니다.
const StyledAnchor = styled.a`
  display: block;
  font-size: ${rem(14)};
`;
```

## 기존 테마는 old namespace 로 접근하여 사용 (Draft)

Web 2.0 의 디자인 시스템을 프론트엔드에 접목시키기로 하였습니다.

그에 따라 기존 테마는 legacy 로 남겨두기 위해 `old` 라 명명된 namespace 로 접근하여 사용 합니다.

```tsx
// before
const StyledText = styled.p`
  color: ${({ theme }) => theme.color.gray40};
`;

// after
const StyledText = styled.p`
  color: ${({ theme }) => theme.old.color.gray40};
`;
```
