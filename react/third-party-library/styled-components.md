# styled-components

## Install

`npm install --save styled-components`

## Styling

## Prop

### animation & 조건부 렌더링

```javascript
import styled, { css, keyframes } from 'styled-components';

const fadeIn = keyframes`
  from {
    opacity: 0;
  }

  to {
    opacity: 1;
  }
`;
const fadeOut = keyframes`
  from {
    opacity: 1;
  }

  to {
    opacity: 0;
  }
`;


const BackGround = styled.div`
  /* 생략 */
  animation-name: ${fadeIn};
  animation-duration: 300ms;
  animation-timing-function: cubic-bezier(0.55, 0.055, 0.675, 0.19);
  animation-fill-mode: forwards;

  ${({ invisible }) =>
    invisible &&
    css`
      animation-name: ${fadeOut};
    `}
`;
```

## Global Style

```javascript
import { createGlobalStyle } from 'styled-components';

const GlobalStyled = createGlobalStyle`

  html {
    font-size: 62.5%;
  }  
  body {
    box-sizing: border-box;
    background-color: #cddae6;
    color: #273049;
  }
  *,
  *::after,
  *::before {
    margin: 0;
    padding: 0;
    box-sizing: inherit;
  }

  a {
    text-decoration: none;
    color: inherit;
  }

  ul {
    list-style: none;
  }
`;

export default GlobalStyled;
```

```javascript
import GlobalStyled from './global/GlobalStyled';


function App() {
  return (
    <AppLayout>
      <GlobalStyled />
```

## Theme

```javascript
const theme = {
  colors: {
    color_primary: '#E3E9F0',
    color_primary_dark: '#9098AD',
    color_primary_light: '#cddae6',
    color_primary_light2: '#bcd0e0',
    color_secondary: '#7A6262',
    color_text: '#273049',
    color_check: '#838067',
    color_fail: '#B69B9C',
  },
};

export default theme;

//children에서 theme에 접근할 수 있다.
<ThemeProvider theme={theme}>
  {children}
```

```javascript
const CheckBtn = styled.a`
  width: 2.2rem;
  height: 2.2rem;
  border-radius: 50%;
  border: 1px solid ${({ theme: { colors } }) => colors.color_secondary};

  cursor: pointer;
`;

const Checked = styled(FiCheck)`
  font-size: 2rem;
  color: 1px solid ${({ theme: { colors } }) => colors.color_check};
`;
```

## Expending

```javascript
const Button = styled.button`
  color: palevioletred;
  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid palevioletred;
  border-radius: 3px;
`;

// A new component based on Button, but with some override styles
const TomatoButton = styled(Button)`
  color: tomato;
  border-color: tomato;
`;

render(
  <div>
    <Button>Normal Button</Button>
    <TomatoButton>Tomato Button</TomatoButton>
  </div>
);
```

