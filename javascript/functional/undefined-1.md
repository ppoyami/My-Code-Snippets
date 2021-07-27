# 제너레이터

## 제너레이터

```javascript
  function* gen() {
    yield 1;
    if (false) yield 2; // 문장을 값(yield)으로 만들 수 있다.
    yield 3;
    return 100; // next 가 다 호출된 후 반환된다. done: true 이기 때문에 순회에서는 포함되지 않음
  }

  let iter = gen(); // 이터레이터를 반환한다.
  log(iter[Symbol.iterator]() == iter); // 그 이터레이터는 이터러블하다.
  log(iter.next());
  log(iter.next());
  log(iter.next());
  log(iter.next()); // {value: 100, done: true}

  for (const a of gen()) log(a); // 제너레이터를 재호출하여, 새로운 이터레이터를 생성
```

## 제너레이터 조합하기

> 이터레이터이자 이터러블을 생성하는 함수\(_well_-_formed_ _iterable_\) 
>
> \(next, Symbol.iterator 메서드를 둘 다 가지고 있음\)

```javascript
function* infinity(i = 0) { // next()를 무제 호출 할수 있는 제너레이터
    while (true) yield i++; // next 호출 될 때마다, 조건없이(while(true)) 이전 값 + 1을 반환
  }
  
function* limit(l, iter) { // l까지 value 를 증가시키는 이터레이터를 반환하는 제너레이
    for (const a of iter) {
      yield a;
      if (a === l) return;
    }
  }
  
function* odds(l) { // 홀수 value만 리턴하는 이터레이터를 생
    for (const a of limit(l, infinity(1))) { // limit 또한 이터러블을 반환
      if (a % 2) yield a; // 때문에, for of 의 프로토콜로 순회가 가능하다
    }
  }
  
for (const a of odds(40)) log(a);
```

## for of, spread, destructuring, rest operator

> 제너레이터는 \(_well-formed_\) 이터레이터를 반환하기 때문에, 이터러블 프로토콜을 따르는 많은 경우에서 사용이 가능하다.

```javascript
  log(...odds(10));
  log([...odds(10), ...odds(20)]);

  const [head, ...tail] = odds(5);
  log(head);
  log(tail);

  const [a, b, ...rest] = odds(10);
  log(a);
  log(b);
  log(rest);
```

