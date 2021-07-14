# Node.js 환경에서의 입력

## BOJ 환경

> 연속 된 값 -&gt; split\(' '\)해서 배열로 사용하고, 하나의 값이면 input\(\) 호출만 해주어 반환받아 사용한다.

```javascript
const fs = require('fs')
const stdin = fs.readFileSync('dev/stdin').toString().split('\n');

const input = (() => {
    let line = 0;
    return () => stdin[line++];
})();

const [v1, v2] = input().split(' ').map(Number)

console.log(v1 / v2);
```

## Local 환경

>

```javascript

```

