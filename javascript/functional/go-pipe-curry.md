# go, pipe, curry

## go

> 첫 번째 값이 reducer의 acc에 들어가, args에 있는 함수들을 적용하면서 축약해서 반환
>
> 코드 흐름이 위에서 아래로 읽기 쉽게 된다.\(함수 중첩은 오른쪽에서 왼쪽, 거꾸로 거슬러 올라가며 읽어야 함\)
>
> 핵심동작: 첫 번째 값에 리스트에 있는 함수들을 차례로 적용시켜 평가하기\(하나의 값 - 축약\)

```javascript
const reduce = (f, acc, iter) => {
  if (!iter) {
    iter = acc[Symbol.iterator](); 
    acc = iter.next().value;
  }
  for (const a of iter) { // go에서 iter는 함수가 들어있는 이터레이터이다.
    acc = f(acc, a); // f -> (a, f) => f(a), acc에다가 f를 적용시킨다.
  }
  return acc;
};

const go = (...args) => reduce((a, f) => f(a), args);

go(
  add(0, 1),
  a => a + 10,
  a => a + 100,
  log
);
```

## pipe

> go와의 차이점은 평가된 값이 아니라 함수\(인자로 전달한 함수들을 파이프라인으로 연결한 함수\)를 반환하는 것이다.

1. 첫 번째 함수와 나머지 함수를 분리
2. pipe가 반환하는 함수는 첫 함수에 여러 인자를 적용할 수 있다.
3. 첫 함수를 go 함수의 첫번째 값으로 하고, go함수 호출\(나머지 함수들을 차례로 적용시켜나가 값을 하나로 평가\) 하는 함수를 반환한다.
4. 결국, 함수의 평가값을 다음 함수의 인자로 넘기면서 축약하는 기능을 한다.

```javascript
  const pipe = (f, ...fs) => (...as) => go(f(...as), ...fs);
  
  const f = pipe(
    (a, b) => a + b,
    a => a + 10,
    a => a + 100
  );
```

## curry

> 인자를 하나만 받는다면, 이후 인자를 더 받아 실행하는 함수를 반환한다.\(지연\)

`f => (a, ..._) => _.length ? f(a, ..._) : (..._) => f(a, ..._);`

_**\(f, a\)를 클로저 환경으로 가지고 있는 함수를 반환한다. f는 curry 생성에서\(curry를 걸은 함수 \(filter, map, reduce의 로직과 같은..\)\), a는 이전 호출에서 받은 하나의 인자이다.**_

`_.length === 0 -> (..._) => f(a, ..._), 인자가 하나인 경우 추가적으로 인자를 받아 실행하는 함수를 리턴`

`_.length !== 0 -> f(a, ..._), 인자가 여러개인 경우, 그 인자들을 spread 하여 실행한다.`

```javascript
const curry =
  f =>
  (a, ..._) =>
    _.length ? f(a, ..._) : (..._) => f(a, ..._);
    
const mult = curry((a, b) => a * b);
log(mult(3)(2)); // 하나만 전달 -> 함수반환 -> 인자 전달하여 실행
log(mult(3, 2)); // 인자가 2개 이상이기 때문에 즉시 실행

const mult3 = mult(3); // 하나만 전달 했을 떄, 함수가 반환된다.
log(mult3(10));


const filter = curry((f, iter) => {
  let res = [];
  for (const a of iter) {
    if (f(a)) res.push(a);
  }
  return res;
});

```

```javascript
go(
    products,
    products => filter(p => p.price < 20000, products),
    log
  );

go(
    products,
    // (a, ..._) => _.length ? f(a, ..._) : (..._) => f(a, ..._);
    // 함수 하나만 전달 -> (..._) => f(p => p.price < 20000, ..._)
    // (..._) => f(p => p.price < 20000, ..._)
    // (products) => f(p => p.price < 20000, products)
    // curry 함수를 적용하여(첫 인자를 가진 함수를 반환하는), 매개변수를 받고 함수에 그대로 전달하는 문법을 축약할 수 있다.
    filter(p => p.price < 20000),
    log
  );
```

## 코드 가독성 개선하기 - go, pipe, curry

```javascript
  log(
    reduce(
      add,
      map(
        p => p.price,
        filter(p => p.price < 20000, products)
      )
    )
  );
  
  // MEMO: go를 사용하면, 코드를 위에서 아래로, 읽을 수 있게 된다.
  go(
    products,
    products => filter(p => p.price < 20000, products),
    products => map(p => p.price, products),
    prices => reduce(add, prices),
    log
  );
  
 //MEMO: curry 함수를 적용하여(매개변수 분리 실행), 매개변수를 받고 함수에 그대로 전달하는 문법을 축약할 수 있다.
  go(
    products,
    filter(p => p.price < 20000), // (a, ..._) => _.length ? f(a, ..._) : (..._) => f(a, ..._);
    map(p => p.price),
    reduce(add),
    log
  );  
```

## pipe로 함수 조합하기

```javascript
  const pipe =
    (f, ...fs) =>
    (...as) =>
      go(f(...as), ...fs);
      
  const total_price = pipe(
    map(p => p.price), // curry 적용된 함수
    reduce(add) // curry 적용된 함수
  );
  
  map(p => p.price) === (..._) => f(p => p.price, ..._)
  reduce(add) === (..._) => f(add)
  
  total_price === (...as) => go(
                                 (...as) => map(p => p.price, ...as), 
                                 (..._) => reduce(add) 
                               );
```

```javascript
// MEMO: pipe로 함수들을 조합할 수 있다.
  const total_price = pipe(
    map(p => p.price),
    reduce(add)
  );

  const base_total_price = predi => pipe(filter(predi), total_price);

  go(
    products,
    base_total_price(p => p.price < 20000),
    log
  );

  go(
    products,
    base_total_price(p => p.price >= 20000),
    log
  );
```

