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

> 한 줄 입력

```javascript
const readline = require('readline');

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

rl.on('line', function (line) {
  input = line.split(' ');

  const num1 = Number(input[0]);
  const num2 = Number(input[1]);

  console.log(num1 + num2);

  rl.close();
}).on('close', function () {
  process.exit();
});

```

> 여러 줄 입력할 땐 아래와 같은 방법으로 해야한다.

```javascript
const fs = require('fs');
const stdin = fs.readFileSync('/dev/stdin').toString().split('\n');

const input = (() => {
  let line = 0;
  return () => stdin[line++];
})();

const [v1, v2] = input().split(' ').map(Number);
const [v3, v4] = input().split(' ').map(Number);
const [v5, v6] = input().split(' ').map(Number);

console.log(v1 / v2);
console.log(v3 / v4);
console.log(v5 / v6);

```

```javascript
➜  백준 git:(master) ✗ node fs.js
2 3
4 5
2 10
0.6666666666666666
0.8
0.2
```

