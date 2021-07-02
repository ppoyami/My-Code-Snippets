# useInputs

여러 input 상태 값을 관리하는\(객체로\) 커스텀 훅이다.

```jsx
import { useState } from 'react';

const useInputs = initialState => {
  const [state, setState] = useState(initialState);

  const onChange = e => {
    const { name, value } = e.target;

    setState({
      ...state,
      [name]: value,
    });
  };
  const reset = () => setState(initialState);
  return [state, onChange, reset];
};

export default useInputs;
```

