# go, pipe, curry

## go

> 첫 번째 값이 reducer의 acc, args에 있는 함수들을 적용하면서 축약해서 반
>
> 핵심동작: 첫 번째 값에 리스트에 있는 함수들을 차례로 적용시켜 평가하기\(하나의 값 - 축약\)

```javascript
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

> 매개변수를 분리하여 실행할 수 있다.

```javascript
const curry = f => (a, ..._) => _.length ? f(a, ..._) : (..._) => f(a, ..._);

const mult = curry((a, b) => a * b);
log(mult(3)(2)); // 하나만 전달 -> 함수반환 -> 인자 전달하여 실행
log(mult(3, 2)); // 인자가 2개 이상이기 때문에 즉시 실행

const mult3 = mult(3); // 하나만 전달 했을 떄, 함수가 반환된다.
log(mult3(10));
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
    filter(p => p.price < 20000),
    map(p => p.price),
    reduce(add),
    log
  );
  
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

