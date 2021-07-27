# emotion

## Install

### With CRA \(feat.craco\)

`npm install --save @emotion/react`

1. **babel plugin 사용**
   1. craco 라이브러리 설치 → craco 라이브러리로, cra 프로젝트의 설정파일을 오버라이드하기
      1. `npm install @craco/craco --save`
   2. emotion babel plugin 설치
      1. `npm i --save-dev @emotion/babel-preset-css-prop`
      2. `npm install --save-dev @emotion/babel-plugin`
   3. craco 설정

      ```text
      // [project-root]/craco.config.js
      module.exports = {
        babel : {
          "presets": ["@emotion/babel-preset-css-prop"],
          "plugins": ["@emotion"]
        }
      }
      ```

   4. 스크립트 변경하기 package.json 에서 react-scripts -&gt; craco 로 변경하기

      ```text
      "scripts" : {
          "start": "craco start",
          "build": "craco build",
          "test": "craco test",
      	  "eject": "craco eject"
        }
      ```

   5. typescript를 사용하면 tsconfig.json 을 수정해주어야 합니다.

      ```text
      {
        "compileOptions" : {
          // ...  생략
          "jsxImportSource": "@emotion/react",
        }
      }
      ```
2. **JSX Pragma, css prop을 사용하는 파일마다 아래와 같은 코드를 삽입**

   최신 CRA는 `/** @jsxImportSource @emotion/react */` 로 주석이 변경되었음

   ```text
   /** @jsx jsx */
   import { jsx } from '@emotion/react'

   /** @jsxImportSource @emotion/react */
   import { css, useTheme, jsx } from '@emotion/react';
   ```

## Styling

#### 1. Tagged Template Literal, object 방식을 통한 CSS 정의 및 적용 방법

_**사용자 정의 컴포넌트에서는 사용이 불가능**_

객체, 템플릿 리터럴 방식의 css 속성 작성법이 다르다.

```javascript
const TextStyle = css`
  font-size: 18px;
`;

return (
  <div
    css={{
      backgroundColor: 'hotpink',
      '&:hover': {
        color: 'lightgreen'
      }
    }}
  >
    <div css={TextStyle}>{title}</div>
  </div>
)
```

#### 2. Tagged Template Literal 방식을 통한 Styled Component 생성 방법

```javascript
import styled from '@emotion/styled';

const Text1 = styled.div`
  font-size: 20px;
`;

const Example = styled('span')`
  color: lightgreen;
  & > a {
    color: hotpink;
  }
`

return (
    <div>
      <Text1>{description}</Text1>
    </div>
  );
```

#### 3. 객체를 통한 Styled Component 생성 방법

* Camel Case를 사용
* 스타일 값은 무조건 String Type

```javascript
const Text2 = styled('div')(() => ({
  fontSize: '15px',
  color: 'blue',
}));
```

## Prop

### styled-component prop

```javascript
import styled from '@emotion/styled'

const Button = styled.button`
  color: ${props =>
    props.primary ? 'hotpink' : 'turquoise'};
`

const Container = styled.div(props => ({
  display: 'flex',
  flexDirection: props.column && 'column'
}))
```

typescript 에서는 &lt;&gt;를 사용..?

```javascript
const Text1 = styled.div<{ disable: boolean }>`
  font-size: 20px;
  font-weight: 700;
  text-decoration: ${({ disable }) => (disable ? 'line-through' : 'none')};
`;

const Text2 = styled('div')<{ disable: boolean }>(({ disable }) => ({
  fontSize: '15px',
  color: 'blue',
  textDecoration: disable ? 'line-through' : 'none',
}));
```

styled\(parameter\)에 컴포넌트를 직접 전달하여, props를 분리하여 적용할 수 있다. active 속성은 css 내부에서만 사

```javascript
const CategoryItem = styled(({ active, to, ...props }) => (
  <Link to={to} {...props} />
))`
  margin-right: 20px;
  padding: 5px 0;
  font-size: 18px;
  font-weight: ${({ active }) => (active ? '800' : '400')};
  cursor: pointer;

  &:last-of-type {
    margin-right: 0;
  }
`;

// 
<CategoryItem
   to={`/?category=${name}`}
   active={name === selectedCategory}
   key={name}
>
```

css prop과 조합이 가능하다.

```javascript
import styled from '@emotion/styled'
import { css } from '@emotion/react'

const dynamicStyle = props =>
  css`
    color: ${props.color};
  `

const Container = styled.div`
  ${dynamicStyle};
`
render(
  <Container color="lightgreen">
    This is lightgreen.
  </Container>
)
```

### css prop

css props에 화살표 함수를 사용하여, theme 인자를 넘긴다.

기본스타일을 정의 하는 방법: className에 있는 기존 emotion 소스가 덮어버리기 때문에 불러와서 작성한다?

```javascript
/** @jsx jsx */
import { jsx } from '@emotion/react'

const P = props => (
  <p
    css={{
      margin: 0,
      fontSize: 12,
      lineHeight: '1.5',
      fontFamily: 'Sans-Serif',
      color: 'black'
    }}
    {...props} // <- props contains the `className` prop
  />
)

const ArticleText = props => (
  <P
    css={{
      fontSize: 14,
      fontFamily: 'Georgia, serif',
      color: 'darkgray'
    }}
    {...props} // <- props contains the `className` prop
  />
)
```



## Global Style

```javascript
import { css, Global } from '@emotion/react';
import { Theme } from './Theme';

const GlobalStyles = (theme: Theme) => css`
  body {
    width: 100vw;
    height: 100vh;
    background-color: ${theme.bgColor};
    color: ${theme.fontColor};
  }
`;

export default GlobalStyles;

return (
    <div>
      <Global styles={globalStyle} />
      {title} {description} {author}
    </div>
  );
```

## Theme

테마 값 제

```javascript
import { css, Global, ThemeProvider } from '@emotion/react';
import {default as THEME} from 'styles/Theme';

return (
    <>
      <ThemeProvider theme={THEME[theme]}>
```

ThemeProvider가 제공하는 theme 값 사용법

```javascript
/** @jsxImportSource @emotion/react */
import { jsx, ThemeProvider } from '@emotion/react'
import styled from '@emotion/styled'

/////////////////////////// CSS /////////////////////////////

const theme = {
  colors: {
    primary: 'hotpink'
  }
}

render(
  <ThemeProvider theme={theme}>
    <div css={theme => ({ color: theme.colors.primary })}>
      some other text
    </div>
  </ThemeProvider>
)

/////////////////////////// STYILED /////////////////////////////

const SomeText = styled.div`
  color: ${props => props.theme.colors.primary};
`

render(
  <ThemeProvider theme={theme}>
    <SomeText>some text</SomeText>
  </ThemeProvider>
)
```

중첩되어 있을 때, useTheme hook로 가져온다.

```javascript
import { css, useTheme } from '@emotion/react';
import { Theme } from 'styles/Theme';

const Profile: React.FC = () => {
  const theme = useTheme(); // ThemeProvider에서 제공하는 값을 가져온다.
  return (
```

## Expending

