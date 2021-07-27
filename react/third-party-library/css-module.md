# css module

## Install

## Styling

### styles 사용하기

```css
.wrapper {
  background: black;
  padding: 1rem;
  color: white;
  font-size: 2rem;
}

/* 글로벌 CSS 를 작성하고 싶다면 */

:global .something {
  font-weight: 800;
  color: aqua;
}
```

```javascript
import React from 'react';
import styles from './CSSModule.module.css';
const CSSModule = () => {
  return (
    <div className={styles.wrapper}>
      안녕하세요, 저는 <span className="something">CSS Module!</span>
    </div>
  );
};

export default CSSModule;
```

### classNames 라이브러리 사용하

```css
.button {
  box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.7);
  cursor: pointer;
  padding: 1.5rem 3rem;
  margin-bottom: 1.5rem;

  background-color: #087f5b;
  color: #fff;

  text-transform: uppercase;
  font-size: 1.2rem;
  font-weight: 800;
  letter-spacing: 3px;
  border: none;
  border-radius: 5px;
}

.button.disabled {
  background-color: #ced4da;
  color: #868e96;
  cursor: text;
}
```

```javascript
import React from 'react';

import classNames from 'classnames/bind';
import styles from './Button.module.css';

const cx = classNames.bind(styles);

export default function Button({ text, disabled, ...rest }) {
  return (
    <>
      <button className={cx('button', { disabled })} {...rest}>
        {text}
      </button>
    </>
  );
}
```

## Prop

```javascript
const MyComponent = ({ highlighted, theme }) => {
  <div 
    className={`MyComponent ${theme} 
    ${highlighted ? 'highlighted' : ''}`}>
      Hello
  </div>
}
```

classNames 라이브러

```javascript
const MyComponent = ({ highlighted, theme }) => {
  <div 
    className={classNames('MyComponent', { highlighted }, theme)}>
      Hello
  </div>
}
```

## Global Style

```css
:global html {
  
}

:global body {
  
}

:global * {
  
}
```

## Theme

## Expending

